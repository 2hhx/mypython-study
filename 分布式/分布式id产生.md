[TOC]



## 一 什么是分布式系统唯一ID

**在复杂分布式系统中，往往需要对大量的数据和消息进行唯一标识。**

如在金融、电商、支付、等产品的系统中，数据日渐增长，对数据分库分表后需要有一个唯一ID来标识一条数据或消息，数据库的自增ID显然不能满足需求，此时一个能够生成全局唯一ID的系统是非常必要的。



## 二、分布式系统唯一ID的特点

![img](https://img2018.cnblogs.com/blog/1350514/201906/1350514-20190623220501289-1738885742.png)

 

1. **全局唯一性**：不能出现重复的ID号，既然是唯一标识，这是最基本的要求。
2. **趋势递增**：在MySQL InnoDB引擎中使用的是聚集索引，由于多数RDBMS使用B-tree的数据结构来存储索引数据，在主键的选择上面我们应该尽量使用有序的主键保证写入性能。
3. **单调递增**：保证下一个ID一定大于上一个ID，例如事务版本号、IM增量消息、排序等特殊需求。
4. **信息安全**：如果ID是连续的，恶意用户的扒取工作就非常容易做了，直接按照顺序下载指定URL即可；如果是订单号就更危险了，竞对可以直接知道我们一天的单量。所以在一些应用场景下，会需要ID无规则、不规则。

同时除了对ID号码自身的要求，业务还对ID号生成系统的可用性要求极高，想象一下，如果ID生成系统瘫痪，这就会带来一场灾难。

**由此总结下一个ID生成系统应该做到如下几点：**

1. 平均延迟和TP999延迟都要尽可能低（TP90就是满足百分之九十的网络请求所需要的最低耗时。TP99就是满足百分之九十九的网络请求所需要的最低耗时。同理TP999就是满足千分之九百九十九的网络请求所需要的最低耗时）；
2. 可用性5个9（99.999%）；
3. 高QPS。

#### 补充：QPS和TPS

**QPS**：Queries Per Second意思是“每秒查询率”，是一台服务器每秒能够相应的查询次数，是对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准。
**TPS**：是TransactionsPerSecond的缩写，也就是事务数/秒。它是软件测试结果的测量单位。一个事务是指一个客户机向服务器发送请求然后服务器做出反应的过程。客户机在发送请时开始计时，收到服务器响应后结束计时，以此来计算使用的时间和完成的事务个数



## 三、分布式系统唯一ID的实现方案

![img](https://img2018.cnblogs.com/blog/1350514/201906/1350514-20190623220536727-283238663.png)

#### **1.UUID**

UUID(Universally Unique Identifier)的标准型式包含32个16进制数字，以连字号分为五段，形式为8-4-4-4-12的36个字符，示例：550e8400-e29b-41d4-a716-446655440000，到目前为止业界一共有5种方式生成UUID，详情见IETF发布的UUID规范 A Universally Unique IDentifier (UUID) URN Namespace。

**优点：**

* 性能非常高：本地生成，没有网络消耗。

**缺点：**

* 不易于存储：UUID太长，16字节128位，通常以36长度的字符串表示，很多场景不适用。

* 信息不安全：基于MAC地址生成UUID的算法可能会造成MAC地址泄露，这个漏洞曾被用于寻找梅丽莎病毒的制作者位置。

* ID作为主键时在特定的环境会存在一些问题，比如做DB主键的场景下，UUID就非常不适用

  UUID主要有五个算法，也就是五种方法来实现

  python的uuid模块提供的UUID类和函数uuid1()，uuid3()，uuid4()，uuid5() 来生成1, 3, 4, 5各个版本的UUID ( 需要注意的是：python中没有uuid2()这个函数)。

  * uuid.uuid1(node clock_seq)

  

  ```
  #   基于时间戳
  #   使用主机ID, 序列号, 和当前时间来生成UUID, 可保证全球范围的唯一性.
  #   但由于使用该方法生成的UUID中包含有主机的网络地址, 因此可能危及隐私.
  #   该函数有两个参数, 如果 node 参数未指定, 系统将会自动调用 getnode() 函数来获取主机的硬件(mac)地址.
  #   如果 clock_seq  参数未指定系统会使用一个随机产生的14位序列号来代替.
  import uuid
  print(uuid.uuid1())
  ```

  

  * uuid.uuid3(namespace, name)

  ```
  #   通过计算名字和命名空间的MD5散列值得到，保证了同一命名空间中不同名字的唯一性，
  #   和不同命名空间的唯一性，***但同一命名空间的同一名字生成相同的uuid****。
  print(uuid.uuid3(uuid.NAMESPACE_URL,'python'))
  print(uuid.uuid3(uuid.NAMESPACE_URL,'python'))
  ```

  * uuid.uuid4() : 基于随机数

  ```
  # 　　通过随机数来生成UUID. 使用的是伪随机数有一定的重复概率.
  print(uuid.uuid4())
  ```

  * uuid.uuid5(namespace, name)

  ```
  # 　　通过计算命名空间和名字的SHA-1散列值来生成UUID, 算法与 uuid.uuid3() 相同
  print(uuid.uuid5(uuid.NAMESPACE_URL,'python'))
  ```

  四 总结

  1 Python中没有基于 DCE 的，所以uuid2可以忽略
  2 uuid4存在概率性重复，由无映射性
  3 若在Global的分布式计算环境下，最好用uuid1
  4 若有名字的唯一性要求，最好用uuid3或uuid5

#### **2.数据库生成**

**以MySQL举例，利用给字段设置auto_increment_increment和auto_increment_offset来保证ID自增，每次业务使用下列SQL读写MySQL得到ID号。**

**![img](https://img2018.cnblogs.com/blog/1350514/201906/1350514-20190623220707487-418813896.png)**

 

这种方案的优缺点如下：

**优点：**

* 非常简单，利用现有数据库系统的功能实现，成本小，有DBA专业维护。
* ID号单调自增，可以实现一些对ID有特殊要求的业务。

**缺点：**

* 强依赖DB，当DB异常时整个系统不可用，属于致命问题。配置主从复制可以尽可能的增加可用性，但是数据一致性在特殊情况下难以保证。主从切换时的不一致可能会导致重复发号。
* ID发号性能瓶颈限制在单台MySQL的读写性能。

#### **3.Redis生成ID**

当使用数据库来生成ID性能不够要求的时候，我们可以尝试使用Redis来生成ID。

这主要依赖于Redis是单线程的，所以也可以用生成全局唯一的ID。可以用Redis的原子操作 INCR和INCRBY来实现。

比较适合使用Redis来生成每天从0开始的流水号。比如订单号=日期+当日自增长号。可以每天在Redis中生成一个Key，使用INCR进行累加。

**优点：**

1）不依赖于数据库，灵活方便，且性能优于数据库。

2）数字ID天然排序，对分页或者需要排序的结果很有帮助。

**缺点：**

1）如果系统中没有Redis，还需要引入新的组件，增加系统复杂度。

2）需要编码和配置的工作量比较大。

#### **4.利用zookeeper（分布式应用程序协调服务）生成唯一ID**

zookeeper主要通过其znode数据版本来生成序列号，可以生成32位和64位的数据版本号，客户端可以使用这个版本号来作为唯一的序列号。

很少会使用zookeeper来生成唯一ID。主要是由于需要依赖zookeeper，并且是多步调用API，如果在竞争较大的情况下，需要考虑使用分布式锁。因此，性能在高并发的分布式环境下，也不甚理想。

#### **5.snowflake（雪花算法）方案**

这种方案大致来说是一种以划分命名空间（UUID也算，由于比较常见，所以单独分析）来生成ID的一种算法，这种方案把64-bit分别划分成多段，分开来标示机器、时间等，比如在snowflake中的64-bit分别表示如下图（图片来自网络）所示：

![img](https://img2018.cnblogs.com/blog/1350514/201906/1350514-20190623220924488-1264328589.png)

41个bit位的时间可以表示（1L<<41）/(1000L*3600*24*365)=69年的时间，10-bit机器可以分别表示1024台机器。如果我们对IDC划分有需求，还可以将10-bit分5-bit给IDC，分5-bit给工作机器。这样就可以表示32个IDC，每个IDC下可以有32台机器，可以根据自身需求定义。12个自增序列号可以表示2^12个ID，理论上snowflake方案的QPS约为409.6w/s，这种分配方式可以保证在任何一个IDC的任何一台机器在任意毫秒内生成的ID都是不同的。

这种方式的优缺点是：

**优点：**

* 毫秒数在高位，自增序列在低位，整个ID都是趋势递增的。
* 不依赖数据库等第三方系统，以服务的方式部署，稳定性更高，生成ID的性能也是非常高的。
* 可以根据自身业务特性分配bit位，非常灵活。

**缺点：**

* 强依赖机器时钟，如果机器上时钟回拨，会导致发号重复或者服务会处于不可用状态。

## 雪花算法-Snowflake

Snowflake是Twitter提出来的一个算法，其目的是生成一个64bit的整数:
![img](https://img2018.cnblogs.com/blog/1552472/201911/1552472-20191115134418390-159822005.png)

* 1bit:一般是符号位，不做处理
* 41bit:用来记录时间戳，这里可以记录69年，如果设置好起始时间比如今年是2018年，那么可以用到2089年，到时候怎么办？要是这个系统能用69年，我相信这个系统早都重构了好多次了。
* 10bit:10bit用来记录机器ID，总共可以记录1024台机器，一般用前5位代表数据中心，后面5位是某个数据中心的机器ID
* 12bit:循环位，用来对同一个毫秒之内产生不同的ID，12位可以最多记录4095个，也就是在同一个机器同一毫秒最多记录4095个，多余的需要进行等待下毫秒。
  上面只是一个将64bit划分的标准，当然也不一定这么做，可以根据不同业务的具体场景来划分，比如下面给出一个业务场景：

1. 服务目前QPS10万，预计几年之内会发展到百万。
2. 当前机器三地部署，上海，北京，深圳都有。
3. 当前机器10台左右，预计未来会增加至百台。
   这个时候我们根据上面的场景可以再次合理的划分62bit,QPS几年之内会发展到百万，那么每毫秒就是千级的请求，目前10台机器那么每台机器承担百级的请求，为了保证扩展，后面的循环位可以限制到1024，也就是2^10，那么循环位10位就足够了。

机器三地部署我们可以用3bit总共8来表示机房位置，当前的机器10台，为了保证扩展到百台那么可以用7bit 128来表示，时间位依然是41bit,那么还剩下64-10-3-7-41-1 = 2bit,还剩下2bit可以用来进行扩展。

![img](https://img2018.cnblogs.com/blog/1552472/201911/1552472-20191115134454552-13309564.png)

* 时钟回拨
  因为机器的原因会发生时间回拨，我们的雪花算法是强依赖我们的时间的，如果时间发生回拨，有可能会生成重复的ID，在我们上面的nextId中我们用当前时间和上一次的时间进行判断，如果当前时间小于上一次的时间那么肯定是发生了回拨，算法会直接抛出异常.

```python
# Twitter's Snowflake algorithm implementation which is used to generate distributed IDs.
# https://github.com/twitter-archive/snowflake/blob/snowflake-2010/src/main/scala/com/twitter/service/snowflake/IdWorker.scala

import time
import logging

from .exceptions import InvalidSystemClock


# 64位ID的划分
WORKER_ID_BITS = 5
DATACENTER_ID_BITS = 5
SEQUENCE_BITS = 12

# 最大取值计算
MAX_WORKER_ID = -1 ^ (-1 << WORKER_ID_BITS)  # 2**5-1 0b11111
MAX_DATACENTER_ID = -1 ^ (-1 << DATACENTER_ID_BITS)

# 移位偏移计算
WOKER_ID_SHIFT = SEQUENCE_BITS
DATACENTER_ID_SHIFT = SEQUENCE_BITS + WORKER_ID_BITS
TIMESTAMP_LEFT_SHIFT = SEQUENCE_BITS + WORKER_ID_BITS + DATACENTER_ID_BITS

# 序号循环掩码
SEQUENCE_MASK = -1 ^ (-1 << SEQUENCE_BITS)

# Twitter元年时间戳
TWEPOCH = 1288834974657


logger = logging.getLogger('flask.app')


class IdWorker(object):
    """
    用于生成IDs
    """

    def __init__(self, datacenter_id, worker_id, sequence=0):
        """
        初始化
        :param datacenter_id: 数据中心（机器区域）ID
        :param worker_id: 机器ID
        :param sequence: 其实序号
        """
        # sanity check
        if worker_id > MAX_WORKER_ID or worker_id < 0:
            raise ValueError('worker_id值越界')

        if datacenter_id > MAX_DATACENTER_ID or datacenter_id < 0:
            raise ValueError('datacenter_id值越界')

        self.worker_id = worker_id
        self.datacenter_id = datacenter_id
        self.sequence = sequence

        self.last_timestamp = -1  # 上次计算的时间戳

    def _gen_timestamp(self):
        """
        生成整数时间戳
        :return:int timestamp
        """
        return int(time.time() * 1000)

    def get_id(self):
        """
        获取新ID
        :return:
        """
        timestamp = self._gen_timestamp()

        # 时钟回拨
        if timestamp < self.last_timestamp:
            logging.error('clock is moving backwards. Rejecting requests until {}'.format(self.last_timestamp))
            raise InvalidSystemClock

        if timestamp == self.last_timestamp:
            self.sequence = (self.sequence + 1) & SEQUENCE_MASK
            if self.sequence == 0:
                timestamp = self._til_next_millis(self.last_timestamp)
        else:
            self.sequence = 0

        self.last_timestamp = timestamp

        new_id = ((timestamp - TWEPOCH) << TIMESTAMP_LEFT_SHIFT) | (self.datacenter_id << DATACENTER_ID_SHIFT) | \
                 (self.worker_id << WOKER_ID_SHIFT) | self.sequence
        return new_id

    def _til_next_millis(self, last_timestamp):
        """
        等到下一毫秒
        """
        timestamp = self._gen_timestamp()
        while timestamp <= last_timestamp:
            timestamp = self._gen_timestamp()
        return timestamp


if __name__ == '__main__':
    worker = IdWorker(1, 2, 0)
    print(worker.get_id())
```

同文件夹下建立exceptions.py

```python
class InvalidSystemClock(Exception):
    """
    时钟回拨异常
    """
    pass
```

配置文件中添加,对应的是机器ID和序列号

```python
    # Snowflake ID Worker 参数
    DATACENTER_ID = 0
    WORKER_ID = 0
    SEQUENCE = 0
```

