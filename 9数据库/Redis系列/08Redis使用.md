# [Redis 中的布隆过滤器](https://segmentfault.com/a/1190000016721700)

[redis](https://segmentfault.com/t/redis)

发布于 2018-10-18   约 5 分钟

> 原文链接：[https://jaychen.cc/redis/2018...](https://jaychen.cc/redis/2018/10/17/bloom-filter-in-redis.html)
> 作者：JayChen

![img](https://segmentfault.com/img/remote/1460000016721703?w=1029&h=538)

## 什么是『布隆过滤器』

布隆过滤器是一个神奇的数据结构，**可以用来判断一个元素是否在一个集合中**。很常用的一个功能是用来**去重**。在爬虫中常见的一个需求：目标网站 URL 千千万，怎么判断某个 URL 爬虫是否宠幸过？简单点可以爬虫每采集过一个 URL，就把这个 URL 存入数据库中，每次一个新的 URL 过来就到数据库查询下是否访问过。

```
select id from table where url = 'https://jaychen.cc'
```

但是随着爬虫爬过的 URL 越来越多，每次请求前都要访问数据库一次，并且对于这种字符串的 SQL 查询效率并不高。除了数据库之外，使用 Redis 的 set 结构也可以满足这个需求，并且性能优于数据库。但是 Redis 也存在一个问题：耗费过多的内存。这个时候布隆过滤器就很横的出场了：这个问题让我来。

相比于数据库和 Redis，使用布隆过滤器可以很好的避免性能和内存占用的问题。

布隆过滤器本质是一个**位数组**，位数组就是数组的每个元素都只占用 1 bit 。每个元素只能是 0 或者 1。这样申请一个 10000 个元素的位数组只占用 10000 / 8 = 1250 B 的空间。布隆过滤器除了一个位数组，还有 K 个哈希函数。当一个元素加入布隆过滤器中的时候，会进行如下操作：

* 使用 K 个哈希函数对元素值进行 K 次计算，得到 K 个哈希值。
* 根据得到的哈希值，在位数组中把对应下标的值置为 1。

举个🌰，假设布隆过滤器有 3 个哈希函数：f1, f2, f3 和一个位数组 `arr`。现在要把 `https://jaychen.cc` 插入布隆过滤器中：

* 对值进行三次哈希计算，得到三个值 n1, n2, n3。
* 把位数组中三个元素 arr[n1], arr[n2], arr[3] 置为 1。

当要判断一个值是否在布隆过滤器中，对元素再次进行哈希计算，得到值之后判断位数组中的每个元素是否都为 1，如果值都为 1，那么说明这个值在布隆过滤器中，如果存在一个值不为 1，说明该元素不在布隆过滤器中。

> 看不懂文字看下面的灵魂画手的图解释👇👇👇

![img](https://segmentfault.com/img/remote/1460000016721704?w=1430&h=846)

看了上面的说明，必然会提出一个问题：当插入的元素原来越多，位数组中被置为 1 的位置就越多，当一个不在布隆过滤器中的元素，经过哈希计算之后，得到的值在位数组中查询，有可能这些位置也都被置为 1。这样一个不存在布隆过滤器中的也有可能被误判成在布隆过滤器中。但是如果布隆过滤器判断说一个元素不在布隆过滤器中，那么这个值就一定不在布隆过滤器中。简单来说：

* 布隆过滤器说某个元素在，可能会被误判。
* 布隆过滤器说某个元素不在，那么一定不在。

这个布隆过滤器的缺陷放到上面爬虫的需求中，可能存在某些没有访问过的 URL 可能会被误判为访问过，但是如果是访问过的 URL 一定不会被误判为没访问过。

## Redis 中的布隆过滤器

redis 在 4.0 的版本中加入了 module 功能，布隆过滤器可以通过 module 的形式添加到 redis 中，所以使用 redis 4.0 以上的版本可以通过加载 [module](https://github.com/RedisLabsModules/rebloom) 来使用 redis 中的布隆过滤器。但是这不是最简单的方式，使用 docker 可以直接在 redis 中体验布隆过滤器。

```
> docker run -d -p 6379:6379 --name bloomfilter redislabs/rebloom
> docker exec -it bloomfilter redis-cli
```

redis 布隆过滤器主要就两个命令：

* `bf.add` 添加元素到布隆过滤器中：`bf.add urls https://jaychen.cc`。
* `bf.exists` 判断某个元素是否在过滤器中：`bf.exists urls https://jaychen.cc`。

上面说过布隆过滤器存在误判的情况，在 redis 中有两个值决定布隆过滤器的准确率：

* `error_rate `：允许布隆过滤器的错误率，这个值越低过滤器的位数组的大小越大，占用空间也就越大。
* `initial_size `：布隆过滤器可以储存的元素个数，当实际存储的元素个数超过这个值之后，过滤器的准确率会下降。

redis 中有一个命令可以来设置这两个值：

```
bf.reserve urls 0.01 100
```

三个参数的含义：

* 第一个值是过滤器的名字。
* 第二个值为 `error_rate` 的值。
* 第三个值为 `initial_size` 的值。

使用这个命令要注意一点：**执行这个命令之前过滤器的名字应该不存在，如果执行之前就存在会报错：(error) ERR item exists**

# [python基于redis实现分布式锁](https://www.cnblogs.com/angelyan/p/11523846.html)





**阅读目录**

* [一、什么是分布式锁](https://www.cnblogs.com/angelyan/p/11523846.html#_label0)
* [二、基于redis实现分布式锁](https://www.cnblogs.com/angelyan/p/11523846.html#_label1)

 

------

[回到顶部](https://www.cnblogs.com/angelyan/p/11523846.html#_labelTop)

## 一、什么是分布式锁

我们在开发应用的时候，如果需要对某一个共享变量进行多线程同步访问的时候，可以使用我们学到的锁进行处理，并且可以完美的运行，毫无Bug！

注意这是单机应用，后来业务发展，需要做集群，一个应用需要部署到几台机器上然后做负载均衡，大致如下图：

![img](https://img2018.cnblogs.com/blog/1449147/201909/1449147-20190915192400145-1228507196.png)

 

 

上图可以看到，变量A存在三个服务器内存中（这个变量A主要体现是在一个类中的一个成员变量，是一个有状态的对象），如果不加任何控制的话，变量A同时都会在分配一块内存，三个请求发过来同时对这个变量操作，显然结果是不对的！即使不是同时发过来，三个请求分别操作三个不同内存区域的数据，变量A之间不存在共享，也不具有可见性，处理的结果也是不对的！

如果我们业务中确实存在这个场景的话，我们就需要一种方法解决这个问题！

为了保证一个方法或属性在高并发情况下的同一时间只能被同一个线程执行，在传统单体应用单机部署的情况下，可以使用并发处理相关的功能进行互斥控制。但是，随着业务发展的需要，原单体单机部署的系统被演化成分布式集群系统后，由于分布式系统多线程、多进程并且分布在不同机器上，这将使原单机部署情况下的并发控制锁策略失效，单纯的应用并不能提供分布式锁的能力。为了解决这个问题就需要一种跨机器的互斥机制来控制共享资源的访问，这就是分布式锁要解决的问题！

分布式锁应该具备哪些条件：

1、在分布式系统环境下，一个方法在同一时间只能被一个机器的一个线程执行；
2、高可用的获取锁与释放锁；
3、高性能的获取锁与释放锁；
4、具备可重入特性；
5、具备锁失效机制，防止死锁；
6、具备非阻塞锁特性，即没有获取到锁将直接返回获取锁失败

[回到顶部](https://www.cnblogs.com/angelyan/p/11523846.html#_labelTop)

## 二、基于redis实现分布式锁

1、选用Redis实现分布式锁原因：

（1）Redis有很高的性能；
（2）Redis命令对此支持较好，实现起来比较方便

2、使用命令介绍：

（1）SETNX

SETNX key val：当且仅当key不存在时，set一个key为val的字符串，返回1；若key存在，则什么都不做，返回0。

（2）expire

expire key timeout：为key设置一个超时时间，单位为second，超过这个时间锁会自动释放，避免死锁。

（3）delete

delete key：删除key

在使用Redis实现分布式锁的时候，主要就会使用到这三个命令。

3、实现思想：

（1）获取锁的时候，使用setnx加锁，并使用expire命令为锁添加一个超时时间，超过该时间则自动释放锁，锁的value值为一个随机生成的UUID，通过此在释放锁的时候进行判断。

（2）获取锁的时候还设置一个获取的超时时间，若超过这个时间则放弃获取锁。

（3）释放锁的时候，通过UUID判断是不是该锁，若是该锁，则执行delete进行锁释放。

4、 分布式锁的简单实现代码：



```
#连接redis
import time
import uuid
from threading import Thread

import redis

redis_client = redis.Redis(host="localhost",
                           port=6379,
                           # password=123,
                           db=10)

#获取一个锁
# lock_name：锁定名称
# acquire_time: 客户端等待获取锁的时间
# time_out: 锁的超时时间
def acquire_lock(lock_name, acquire_time=10, time_out=10):
    """获取一个分布式锁"""
    identifier = str(uuid.uuid4())
    end = time.time() + acquire_time
    lock = "string:lock:" + lock_name
    while time.time() < end:
        if redis_client.setnx(lock, identifier):
            # 给锁设置超时时间, 防止进程崩溃导致其他进程无法获取锁
            redis_client.expire(lock, time_out)
            return identifier
        elif not redis_client.ttl(lock):
            redis_client.expire(lock, time_out)
        time.sleep(0.001)
    return False

#释放一个锁
def release_lock(lock_name, identifier):
    """通用的锁释放函数"""
    lock = "string:lock:" + lock_name
    pip = redis_client.pipeline(True)
    while True:
        try:
            pip.watch(lock)
            lock_value = redis_client.get(lock)
            if not lock_value:
                return True

            if lock_value.decode() == identifier:
                pip.multi()
                pip.delete(lock)
                pip.execute()
                return True
            pip.unwatch()
            break
        except redis.excetions.WacthcError:
            pass
    return False
```



**5、测试刚才实现的分布式锁**

例子中使用50个线程模拟秒杀10张票，使用–运算符来实现商品减少，从结果有序性就可以看出是否为加锁状态。



```
count=10

def seckill(i):
    identifier=acquire_lock('resource')
    print("线程:{}--获得了锁".format(i))
    time.sleep(1)
    global count
    if count<1:
        print("线程:{}--没抢到，票抢完了".format(i))
        return
    count-=1
    print("线程:{}--抢到一张票，还剩{}张票".format(i,count))
    release_lock('resource',identifier)


for i in range(50):
    t = Thread(target=seckill,args=(i,))
    t.start()
```



 