## 一 布隆过滤器简介

bloomfilter：是一个通过多哈希函数映射到一张表的数据结构，能够快速的判断一个元素在一个集合内是否存在，具有很好的空间和时间效率。（典型例子，爬虫url去重）

原理：  
BloomFilter 会开辟一个m位的bitArray(位数组)，开始所有数据全部置 0
。当一个元素过来时，能过多个哈希函数（h1,h2,h3....）计算不同的在哈希值，并通过哈希值找到对应的bitArray下标处，将里面的值 0 置为 1
。

关于多个哈希函数，它们计算出来的值必须 [0,m) 之中。

例子：

有这么一个网址

假设长度为 20的bitArray，通过 3 个哈希函数求值。如下图：

![timg](https://tva1.sinaimg.cn/large/006y8mN6gy1g9dwunnbcyj30hk06vjrg.jpg)

另外说明一下，当来查找对应的值时，同样通过哈希函数求值，再去寻找数组的下标，如果所有下标都为1时，元素存在。当然也存在错误率。（如：当数组全部为1时，那么查找什么都是存在的），但是这个错误率的大小，取决于数组的位数和哈希函数的个数。

## 二 Python中使用布隆过滤器

    
    
    #python3.6 安装
    #需要先安装bitarray
    pip3 install bitarray-0.8.1-cp36-cp36m-win_amd64.whl（pybloom_live依赖这个包，需要先安装）
    #下载地址：https://www.lfd.uci.edu/~gohlke/pythonlibs/
    pip3 install pybloom_live

### 示例一

    
    
    #ScalableBloomFilter 可以自动扩容
    from pybloom_live import ScalableBloomFilter
    
    
    bloom = ScalableBloomFilter(initial_capacity=100, error_rate=0.001, mode=ScalableBloomFilter.LARGE_SET_GROWTH)
    
    url = "www.cnblogs.com"
    
    url2 = "www.liuqingzheng.top"
    
    bloom.add(url)
    
    print(url in bloom)
    
    print(url2 in bloom)

示例二

    
    
    #BloomFilter 是定长的
    from pybloom_live import BloomFilter
    
    bf = BloomFilter(capacity=1000)
    url='www.baidu.com'
    bf.add(url)
    
    print(url in bf)
    
    print("www.liuqingzheng.top" in bf)

