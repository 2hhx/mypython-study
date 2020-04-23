要想使用redis提供的布隆过滤器，必须添加redis 4.0版本以上的插件才行，具体参照网上安装步骤。

## 一 Docker安装

RedisBloom需要先进行安装，推荐使用Docker进行安装，简单方便:

    
    
    docker pull redislabs/rebloom:latest
    docker run -p 6379:6379 --name redis-redisbloom redislabs/rebloom:latest
    docker exec -it redis-redisbloom bash
    # redis-cli
    # 127.0.0.1:6379> bf.add xiaoyuanqujing hello

### 二 直接编译

当然也可以直接编译进行安装:

    
    
    git clone https://github.com/RedisBloom/RedisBloom.git
    cd RedisBloom
    make //编译 会生成一个rebloom.so文件
    redis-server --loadmodule /path/to/rebloom.so
    redis-cli -h 127.0.0.1 -p 6379

此模块不仅仅实现了布隆过滤器，还实现了 CuckooFilter（布谷鸟过滤器），以及 TopK 功能。CuckooFilter 是在
BloomFilter 的基础上主要解决了BloomFilter不能删除的缺点。先来看看 BloomFilter，后面介绍一下 CuckooFilter。

## 三 基本命令

    
    
    bf.add 添加元素到布隆过滤器
    bf.exists 判断元素是否在布隆过滤器
    bf.madd 添加多个元素到布隆过滤器，bf.add只能添加一个
    bf.mexists 判断多个元素是否在布隆过滤器

例子：

    
    
    > bf.add rurl www.baidu.com
    > bf.exists rurl www.baidu.com
    > bf.madd rurl www.sougou.com www.jd.com
    > bf.mexists rurl www.jd.com www.taobao.com

布隆过滤器在第一次add的时候自动创建基于默认参数的过滤器，Redis还提供了自定义参数的布隆过滤器。

在add之前使用bf.reserve指令显式创建，其有3个参数，key，error_rate，
initial_size，错误率越低，需要的空间越大，error_rate表示预计错误率，initial_size参数表示预计放入的元素数量，当实际数量超过这个值时，误判率会上升，所以需要提前设置一个较大的数值来避免超出。

默认的error_rate是0.01，initial_size是100。

利用布隆过滤器减少磁盘 IO 或者网络请求，因为一旦一个值必定不存在的话，我们可以不用进行后续昂贵的查询请求。

### 四 测试误判率

接下来来测试一下误判率:

    
    
    import redis
    client = redis.Redis()
    size = 100000
    count = 0
    for i in range(size):
        client.execute_command("bf.add", "lqz", "xxx%d" % i)
        result = client.execute_command("bf.exists", "lqz", "xxx%d" % (i + 1))
        if result == 1:
            print(i)
            count += 1
    print("size: {} , error rate: {}%".format(size, round(count / size * 100, 5)))

测试结果如下:

    
    
    #第一次测试
    size: 1000 , error rate: 1.0%
    #第一次测试
    size: 10000 , error rate: 1.25%
    #第一次测试
    size: 100000 , error rate: 1.304%

size=1000，就出现1%的误判率，size越高误判率会越高，那有没有办法控制误判率了，答案是有的。

实际上布隆过滤器是提供自定义参数，之前都是使用默认的参数，此模块还提供了一个命令`bf.reserve`，提供了三个参数， key,
error_rate和initial_size。错误率越低，需要的空间越大，initial_size参数表示预计放入布隆过滤器的元素数量，当实际数量超出这个数值时，误判率会上升。
默认的参数是 error_rate=0.01, initial_size=100。

接下来测试一下:

    
    
    import redis
    client = redis.Redis()
    size = 10000
    count = 0
    client.execute_command("bf.reserve", "lqz", 0.001, size) # 新增
    for i in range(size):
        client.execute_command("bf.add", "lqz", "xxx%d" % i)
        result = client.execute_command("bf.exists", "lqz", "xxx%d" % (i + 1))
        if result == 1:
            print(i)
            count += 1
    print("size: {} , error rate: {}%".format(size, round(count / size * 100, 5)))
    

新增一行代码，简单测试一下效果:

    
    
    #第一次执行
    size: 10000 , error rate: 0.0%
    #第二次执行
    size: 100000 , error rate: 0.001%

误判率瞬间少了1000多倍。

但是要求误判率越低，所需要的空间是需要越大，可以有一个公式计算，由于公式较复杂，直接上类似计算器，感受一下:

如果一千万的数据，误判率允许 1%， 大概需要11M左右

如果要求误判率为 0.1%，则大概需要 17 M左右。

但这空间相比直接用set存1000万数据要少太多了

