## 一 安装JDK环境

因为ElasticSearch是用Java语言编写的，所以必须安装JDK的环境，并且是JDK 1.8以上，具体操作步骤自行百度

安装完成查看java版本

    
    
    java -version

## 二 官网下载最新版本

下载地址[<https://www.elastic.co/cn/downloads/elasticsearch>],选择相应版本下载即可

![image-20191204173716931](https://tva1.sinaimg.cn/large/006tNbRwgy1g9kts83vzbj31o80tsjvo.jpg)

## 三 下载其他版本

直接点击<https://www.elastic.co/cn/downloads/past-releases#elasticsearch>

![image-20191204173847321](https://tva1.sinaimg.cn/large/006tNbRwgy1g9kttpffz5j31320u0ags.jpg)

## 三 下载完成，启动

解压文件，切换到解压文件路径下，执行

    
    
    cd elasticsearch-<version> #切换到路径下
    ./bin/elasticsearch  #启动es
    #如果你想把 Elasticsearch 作为一个守护进程在后台运行，那么可以在后面添加参数 -d 。
    #如果你是在 Windows 上面运行 Elasticseach，你应该运行 bin\elasticsearch.bat 而不是 bin\elasticsearch

## 四 测试启动是否成功

在浏览器输入以下地址：<http://127.0.0.1:9200/>

即可看到如下内容：

    
    
    {
      "name" : "lqzMacBook.local",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "G1DFg-u6QdGFvz8Z-XMZqQ",
      "version" : {
        "number" : "7.5.0",
        "build_flavor" : "default",
        "build_type" : "tar",
        "build_hash" : "e9ccaed468e2fac2275a3761849cbee64b39519f",
        "build_date" : "2019-11-26T01:06:52.518245Z",
        "build_snapshot" : false,
        "lucene_version" : "8.3.0",
        "minimum_wire_compatibility_version" : "6.8.0",
        "minimum_index_compatibility_version" : "6.0.0-beta1"
      },
      "tagline" : "You Know, for Search"
    }

## 五 关闭es

    
    
    #查看进程
    ps -ef | grep elastic
    #干掉进程
    kill -9 2382（进程号）
    #以守护进程方式启动es
    elasticsearch -d

