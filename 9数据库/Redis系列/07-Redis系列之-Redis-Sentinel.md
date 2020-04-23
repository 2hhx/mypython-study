## 一 主从复制高可用

```python
#主从复制存在的问题：
#1 主从复制，主节点发生故障，需要做故障转移，可以手动转移：让其中一个slave变成master
#2 主从复制，只能主写数据，所以写能力和存储能力有限
```

## 二 架构说明

可以做故障判断，故障转移，通知客户端（其实是一个进程），客户端直接连接sentinel的地址

<img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gadzuhodicj30oe0dc0y9.jpg" alt="image-20191229230823911" style="zoom:50%;" />

1 多个sentinel发现并确认master有问题

2 选举触一个sentinel作为领导

3 选取一个slave作为新的master

4 通知其余slave成为新的master的slave

5 通知客户端主从变化

6 等待老的master复活成为新master的slave

## 三 安装配置

```python
1 配置开启主从节点
2 配置开启sentinel监控主节点（sentinel是特殊的redis）
3 应该是多台机器
```

```python
#配置开启sentinel监控主节点
mkdir -p redis4/conf redis4/data redis5/conf redis5/data redis6/data redis6/conf

vi sentinel.conf


port 26379
daemonize no
dir /data
protected-mode no
bind 0.0.0.0
logfile "redis_sentinel.log"

sentinel monitor mymaster 127.0.0.1 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000


docker run -p 26379:26379 --name redis_26379 -v /home/redis4/conf/sentinel.conf:/etc/redis/sentinel.conf -v /home/redis4/data:/data -d redis redis-sentinel /etc/redis/sentinel.conf

docker run -p 26378:26379 --name redis_26378 -v /home/redis5/conf/sentinel.conf:/etc/redis/sentinel.conf -v /home/redis5/data:/data -d redis redis-sentinel /etc/redis/sentinel.conf

docker run -p 26377:26379 --name redis_26377 -v /home/redis6/conf/sentinel.conf:/etc/redis/sentinel.conf -v /home/redis6/data:/data -d redis redis-sentinel /etc/redis/sentinel.conf



redis-sentinel sentinel.conf

info
配置会重写，自动发现slave
```

```python

sentinel monitor mymaster 127.0.0.1 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
sentinel monitor <master-name> <ip> <redis-port> <quorum>
告诉sentinel去监听地址为ip:port的一个master，这里的master-name可以自定义，quorum是一个数字，指明当有多少个sentinel认为一个master失效时，master才算真正失效

sentinel auth-pass <master-name> <password>
设置连接master和slave时的密码，注意的是sentinel不能分别为master和slave设置不同的密码，因此master和slave的密码应该设置相同。

sentinel down-after-milliseconds <master-name> <milliseconds> 
这个配置项指定了需要多少失效时间，一个master才会被这个sentinel主观地认为是不可用的。 单位是毫秒，默认为30秒

sentinel parallel-syncs <master-name> <numslaves> 
这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，这个数字越小，完成failover所需的时间就越长，但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。

sentinel failover-timeout <master-name> <milliseconds>
failover-timeout 可以用在以下这些方面：     
1. 同一个sentinel对同一个master两次failover之间的间隔时间。   
2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。    
3.当想要取消一个正在进行的failover所需要的时间。    
4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了。
```



## 四 客户端连接

```python
import redis
from redis.sentinel import Sentinel

# 连接哨兵服务器(主机名也可以用域名)
# 10.0.0.101:26379
sentinel = Sentinel([('10.0.0.101', 26379),
                     ('10.0.0.101', 26378),
                     ('10.0.0.101', 26377)
		     ],
                    socket_timeout=5)


print(sentinel)
# 获取主服务器地址
master = sentinel.discover_master('mymaster')
print(master)




# 获取从服务器地址
slave = sentinel.discover_slaves('mymaster')
print(slave)




# 获取主服务器进行写入
# master = sentinel.master_for('mymaster', socket_timeout=0.5)
# w_ret = master.set('foo', 'bar')
#
#
#
#
# slave = sentinel.slave_for('mymaster', socket_timeout=0.5)
# r_ret = slave.get('foo')
# print(r_ret)

```



## 五 实现原理

## 六 常见问题