



## 投简历，不管在什么平台，只要简历投出去，投了10家，一家都没有回复，简历有问题

## 年龄20，21，22    12-18k，不写，24岁，25岁，毕业时间，专业（不是计算机相关专业，就别写了），（重点本科）（统招本科）专科（不写，直接写本科）

django缓存  cache.get(对象)  pickle转成二进制，存到redis中      cache.set()

orm查询数据，ret=Book.object.all()   查出来，没有放缓存

封装一个方法 ret= getdata(Book,True)

## 项目 没得可写？

尽量不要再写网站（自动化运维项目）抖音（web），尽量详细

## 个人总结：抗压能力，举个例子（一个月内打卡时间为300小时），自学能力强，公司某个项目go语言写完了，docker部署



## 面试：整个面试过程一定录音，如果面了5家，一家有意向的都没有，一定处在面试过程中

## 不要拒绝任何offer，手里3个offer，11k，13，15



## 两个offer不知道该怎么选？选哪个都对，不用纠结





# 进入内容

## Redis操作

5大数据类型（面试），每种数据类型能做什么？



布隆过滤器：用很少的空间来去重

GEO：地理位置信息（微信附近的人，本质是有序集合）

事务：要么都成功，要么都失败，原子性的，pipline

window上其实是没有redis的，作者不支持windows

## 1 安装，详见笔记

## 2 Redis三种启动方式

配置文件详解去网上搜

停止服务：kill -9 进程号、  在客户端敲 shutdown

## 3 单线程为什么这么快？

1 纯内存（这是它最重要的）

2 非阻塞IO （epoll），自身实现了事件处理，不在网络io上浪费过多时间

3 避免线程间切换和竞态消耗







## 4 发布订阅，观察者模式



## 5 GEO 地理位置信息

附近的人，查找附近的商铺，计算两个点的直线距离



## 6 持久化方案

### rdb 和 aof

AOF 重写

##  7 主从复制

一台机器上可以启动多个redis服务，监听的端口不同

./src/redis-cli -p 6378   连接6378

### 通过命令来做

slaveof 127.0.0.1 6379

### 通过配置文件（写到配置文件）

slaveof ip port #配置从节点ip和端口
slave-read-only yes #从节点只读，因为可读可写，数据会乱



## 8 哨兵高可用

### 1 搭一个一主两从

```python
#创建三个配置文件：
#第一个是主配置文件
daemonize yes
pidfile /var/run/redis.pid
port 6379
dir "/opt/soft/redis/data"
logfile “6379.log”

#第二个是从配置文件
daemonize yes
pidfile /var/run/redis2.pid
port 6378
dir "/opt/soft/redis/data2"
logfile “6378.log”
slaveof 127.0.0.1 6379
slave-read-only yes
#第三个是从配置文件
daemonize yes
pidfile /var/run/redis3.pid
port 6377
dir "/opt/soft/redis/data3"
logfile “6377.log”
slaveof 127.0.0.1 6379
slave-read-only yes


#把三个redis服务都启动起来
./src/redis-server redis_6379.conf
./src/redis-server redis_6378.conf
./src/redis-server redis_6377.conf
```

### 2 搭建哨兵

```python
# sentinel.conf这个文件
# 把哨兵也当成一个redis服务器
创建三个配置文件分别叫sentinel_26379.conf sentinel_26378.conf  sentinel_26377.conf

#内容如下(需要修改端口，文件地址日志文件名字)
port 26379
daemonize yes
dir ./data3
protected-mode no
bind 0.0.0.0
logfile "redis_sentinel3.log"
sentinel monitor mymaster 127.0.0.1 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000


#启动三个哨兵
./src/redis-sentinel sentinel_26379.conf
./src/redis-sentinel sentinel_26378.conf
./src/redis-sentinel sentinel_26377.conf


# 主动停掉主redis 6379，哨兵会自动选择一个从库作为主库
#等待原来的主库启动，该主库会变成从库




```





作业：

1 使用笔记中的地理位置数据，打印出附近90km的城市

2 搭建redis一主两从

3 搭建哨兵实现高可用，主库挂掉，自动选择从库作为主库

































