## 一 介绍

Apache Benchmark(简称ab) 是Apache安装包中自带的压力测试工具 ，简单易用。  
使用起来非常的简单和方便。  
不仅仅是可以apache服务器进行网站访问压力测试，还可以对其他类型的服务器进行压力测试。  
比如nginx,tomcat,IIS等

## 二 安装

### 2.1 windows安装

    
    
    #下载地址：
    #https://www.apachehaus.com/cgi-bin/download.plx
    #下载完成后解压
    #修改解压根目录下的conf/httpd.conf文件的端口配置，默认是80端口，应该是被占用了，无法安装，可以自行修改为其他，改为8088端口
    #安装
    httpd -k install
    
    #提示如下错误，详见windows 提示缺少VCRUNTIME140.dll错误：
    ---------------------------
    abs.exe - 系统错误
    ---------------------------
    由于找不到 VCRUNTIME140.dll，无法继续执行代码。重新安装程序可能会解决此问题。 
    ---------------------------
    确定   
    ---------------------------
    #使用
    ab -n 2 -c 2 https://www.cnblogs.com/index.html

### 2.2 Linux安装

    
    
    - centOS7 下的安装
        yum -y install httpd-tools
    - centos6.5 默认已安装了ab

## 三 使用

    
    
    #使用：-n 表示请求数，-c 表示并发数.
    ab -n 100 -c 10 http://www.baidu.com/s
    #返回内容：
    
    Server Software:        BWS/1.1   
    ##服务器软件和版本
    Server Hostname:        www.baidu.com  
    ##请求的地址/域名
    Server Port:            80   
    ##端口
    Document Path:          /s  
    ##请求的路径
    Document Length:        112435 bytes  
    ##页面数据/返回的数据量
    Concurrency Level:      10   
    ##并发数
    Time taken for tests:   4.764 seconds  
    ##共使用了多少时间 
    Complete requests:      100  
    ##请求数 
    Failed requests:        99  
    ##失败请求  百度为什么失败这么多，应该是百度做了防范  
       (Connect: 0, Receive: 0, Length: 99, Exceptions: 0)
    Total transferred:      11342771 bytes  
    ##总共传输字节数，包含http的头信息等 
    HTML transferred:       11247622 bytes  
    ##html字节数，实际的页面传递字节数 
    Requests per second:    20.99 [#/sec] (mean) 
     ##每秒多少请求，这个是非常重要的参数数值，服务器的吞吐量 
    Time per request:       476.427 [ms] (mean)   
    ##用户平均请求等待时间 
    Time per request:       47.643 [ms] (mean, across all concurrent requests)  
    ##服务器平均处理时间，也就是服务器吞吐量的倒数 
    Transfer rate:          2325.00 [Kbytes/sec] received
     ##每秒获取的数据长度
    
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:       22   41  12.4     39      82
    ##连接的最小时间，平均值，中值，最大值
    Processing:   113  386 211.1    330    1246
    ##处理时间
    Waiting:       25   80  43.9     73     266
    ##等待时间
    Total:        152  427 210.1    373    1283
    ##合计时间
    
    Percentage of the requests served within a certain time (ms)
      50%    373   
    ## 50%的请求在373ms内返回 
      66%    400   
    ## 60%的请求在400ms内返回 
      75%    426
      80%    465
      90%    761
      95%    930
      98%   1192
      99%   1283
     100%   1283 (longest request)
    

### 参数介绍

    
    
    -n  即requests，用于指定压力测试总共的执行次数。
    -c  即concurrency，用于指定的并发数。
    -t  即timelimit，等待响应的最大时间(单位：秒)。
    -b  即windowsize，TCP发送/接收的缓冲大小(单位：字节)。
    -p  即postfile，发送POST请求时需要上传的文件，此外还必须设置-T参数。
    -u  即putfile，发送PUT请求时需要上传的文件，此外还必须设置-T参数。
    -T  即content-type，用于设置Content-Type请求头信息，例如：application/x-www-form-urlencoded，默认值为text/plain。
    -v  即verbosity，指定打印帮助信息的冗余级别。
    -w  以HTML表格形式打印结果。
    -i  使用HEAD请求代替GET请求。
    -x  插入字符串作为table标签的属性。
    -y  插入字符串作为tr标签的属性。
    -z  插入字符串作为td标签的属性。
    -C  添加cookie信息，例如："Apache=1234"(可以重复该参数选项以添加多个)。
    -H  添加任意的请求头，例如："Accept-Encoding: gzip"，请求头将会添加在现有的多个请求头之后(可以重复该参数选项以添加多个)。
    -A  添加一个基本的网络认证信息，用户名和密码之间用英文冒号隔开。
    -P  添加一个基本的代理认证信息，用户名和密码之间用英文冒号隔开。
    -X  指定使用的和端口号，例如:"126.10.10.3:88"。
    -V  打印版本号并退出。
    -k  使用HTTP的KeepAlive特性。
    -d  不显示百分比。
    -S  不显示预估和警告信息。
    -g  输出结果信息到gnuplot格式的文件中。
    -e  输出结果信息到CSV格式的文件中。
    -r  指定接收到错误信息时不退出程序。
    -h  显示用法信息，其实就是ab -help。

