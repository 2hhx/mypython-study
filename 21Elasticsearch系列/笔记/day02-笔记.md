



# 简历问题

```python
#1 关于你的照片放不放的问题？长相特别年轻（尤其指男生），比较老的照片（女生长得漂亮，可以放）
#2 你的年龄，如果很小，不要写（2000年，1999年），可以写大一些，为了有面试机会
#3 找实习，尽量真实一些（找实习，应届压力很大），招实习的人，要求并不比正式低多少。群面
#4 专科（别写了，写个本科）
#5 个人技能给我多写（多到我给你看的时候删东西），熟悉python的什么？python的三大器，元类，魔法方法（__new__,__enter__）,匿名函数，   熟悉python字典底层实现，装饰器的本质，闭包
# 数据库mysql，curd，主从搭建，读写分离，分库分表，索引原理（b+）....
#6  redis :启动方式，配置参数，5大类型基本使用，基于字符串做缓存，基于incby做统计，基于集合去重，基于有序集合排行榜，发布订阅，rdb和aof持久化，aof重写，地理位置信息实现附近的人，一主多从，哨兵实现高可用
#7  go：数组，切片，map，可变长函数，多返回值，信道，goroutine，异常处理，
#8 django：缓存的本质（pickle），处理跨越（插件，我自己写的）
#9 语文水平，令我发指：      
	-对http有一定的了解：http协议，请求首行，请求头（refer，。。。。），请求体，响应状态码（1，2，3，4，301，302）cookie，session   ，token实现原理
  -websockt 协议，       跨越问题，同源策略，csrf，cors，预防
  
  网络协议：5层模型，tcp/ip 三次握手。。。
	-知道drf的使用，
  
# 10 项目部分：
	-XX商场，XX平台，XX在线教育，XXcmdb，xx客户关系管理系统，xx电影，xx生  鲜   平台，xx平台，xx项目
  
  
  
  -1 去应用市场找小众的app
  -2 自动化运维相关devops，代码自动测试，自动上线（cmdb：资产收集，定时任务，批量下发命令，代码自动发布，自动启动停止服务，自动：10.0.0.1上安装一个5.x版本的nginx，）https://github.com/jumpserver/jumpserver
    日志审计（nginx运行，产生日志，日志拿出来，分析一下4开头有多少，5开头有多少，echars画个图，展现出来）
  	-正在运行的docker容器，停掉某个容器，删除容器，启动容器，下载镜像
  ----运营人员负责维护数据
  -3 在幼儿园：需不需要一套管理系统，管理老师，管理家长信息，管理学生，在微信上直接看班级（摄像头），微信小程序
  
 # 11 个人总结：
	-抗压，兴趣爱好，。。。。例子，印证你抗压，优秀新人，连续多久奖学金，
  -自学能力强，docker，es，go自学的，公司全用django，flask，用flask，做了个xx项目放在git上，git地址是。。。
  -写了xxx博客，地址是，（千万不要拿你在学校的博客，隐藏，比较高端的，redis的，es相关，go相关），写一整套，给小白入门的
  -自己搭建博客：码云 +hexo ：日期是自己的（自己改）
 
# （非常重要）面试的时候用的：简历+1w字的独白：解释你简历上写的东西,项目
```



# 今日内容

## 1 es原理，倒排索引

### 1.1 产生背景

```python
#大规模数据的存储和检索
# 程序热更新，冷更新  冷备份，热备份
# es产生了，解决大规模数据的存储和检索
# Elasticsearch 是一个基于Lucene的分布式搜索  和  分析引擎
# 一个  开源   的  高扩展   的  分布式    全文检索引
# 近实时   --几百亿条数据，2，3秒
# Java开发, 在Apache许可条款下开放源码发布 : 组织，开源apache服务器，开源组织，协议，自己写的项目遵循apache许可的协议，放到网上开源：你开源的别人可以免费使用，你不能拿着我的东西赚钱，改改，闭源，收费。。
	-lamp搭建：Linux，apache，mysql，php（php服务器），
	-lnmp：linux，nginx，mysql，python
	-apache服务器，nginx 一种东西
  -tomcat，jboss，uwsgi，php
# 使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，使得全文检索变得简单
	-Lucene---java平台的搜索引擎，但是只能用java来操作，其他语言操作不了（python的集合），别的语言能用python集合么？基于集合封装一个服务，通过resful来调用服务，就可以使用python集合，任何语言都可以使用。。。
# es和lucene的关系。。。
#Elasticsearch vs solr（很少）  --就像mysql和sqlite的关系，都是搜索引擎，不同公司出的（底层都是基于lucene）


```

### 1.2 Elasticsearch核心概念

```python
# Cluster：集群,可以一台，可以多台
# 节点Node：形成集群的每个服务器称为节点。
# Shard：分片：存1t文件，把1t分成多片  200g一片，分成5片，每个分片放到不同的5台服务器上,一台服务器挂了，其他数据库的内容存在，导致数据不完整，整个架构不会崩溃。
#Replia：副本：再把5片每个复制一份或多份，再放在不同的机器上
	-通过分片和副本实现了高可用性，一个节点挂掉，数据还是完整的，（副本会作为主）
#全文检索:一篇文章进行索引
```

### 1.3 与关系型数据库Mysql对比

```python
mysql                          es
数据库                         -----》索引
表                            -------》类型
一列一列字段                    -----》field
每个字段有属性（int，varchar）   ---》mapping（映射）
一行一行数据                    ----》文档
索引                         -----》所有都是索引
sql语句                     ------》resful的接口（get，post）

```

### 1.4 文档-->类型-->索引

```python
一行一行数据-----》类型（表）------》索引（数据库）
```

### 1.5 什么是elk

elf 
Sdk 
EFK 

```python
https://blog.csdn.net/miss1181248983/article/details/89384990
# 日志分析系统：运维相关的
# e：es   l：Logstash  k:Kibana    做日志收集和日志分析，Logstash负责吧日志收集起来，丢到es中，Kibana负责过滤，展示
```

### 1.6 Elasticsearch特点和优势

```python
插件：ik分词

```

### 1.7 为什么使用es

```python
 # 2013年初，GitHub抛弃了Solr，采取ElasticSearch 来做PB级的搜索。 “GitHub使用ElasticSearch搜索20TB的数据，包括13亿文件和1300亿行代码”
 # 各大互联网公司都会用
```

### 1.8 我们在什么场景使用

```python
# 几乎每个系统都会有一个搜索的功能（只要涉及到搜索，就可以用它来做，mysql可以吗？对于大数据量，非常慢，所以用es）
```

### 1.9 到底能处理多大数据

```python
es一个分片是一个Lucene索引，不能处理多于21亿篇文档，或者多于2740亿的唯一词条，几遍单台机器，有5个分片，5*21亿篇文档，100多亿条数据
```



## 2 kibana，elasticsearch-head

```python
# kibana：相当于navcate   es客户端，官方提供的



#https://www.elastic.co/cn/downloads/past-releases/kibana-7-6-1
# 下载，解压，修改配置文件vim 安装目录/config/kibana.yml,在最后添加
server.port: 5601
server.host: "127.0.0.1"
server.name: chen
elasticsearch.hosts: ["http://localhost:9200/"]
  
# 启动./bin/kibana
# 在浏览器输入：127.0.0.1:5601

## 注意
1.版本和es要一致

```



```python
# elasticsearch-head： es客户端，第三方的(基于nodejs写的，需要安装nodejs)
# elasticsearch-head是elasticsearch的一款可视化工具，依赖于node.js ，所以需要先安装node.js

# 下载解压：https://github.com/mobz/elasticsearch-head
# 安装npm install grunt -g
# 切到解压路径下：npm install
# npm run start启动elasticsearch-head
# 浏览器打开：http://localhost:9100/

# 修改 Elasticsearch 安装目录中config 文件夹下 elasticsearch.yml 文件，加入下面两行：修改跨越
http.cors.enabled: true
http.cors.allow-origin: "*"
```

## 3 创建索引

```python
#设置
http://127.0.0.1:5601/app/kibana#/dev_tools/console?_g=()


# 创建lqz索引（数据，创建数据库）
# PUT 创建一个索引（相当于创建一个数据库）
get chen
# 创建一个索引（相当于创建一个数据库）
PUT chen

GET chen


DELETE chen
PUT chen

GET lqz

```

```python
GET lqz
{
  "lqz" : {
    "aliases" : { },
    "mappings" : { },
    "settings" : {
      "index" : {
        "creation_date" : "1585922717462",
        "number_of_shards" : "5",#默认分片5
        "number_of_replicas" : "1",#副本
        "uuid" : "Neiblw1jTReokvY5DnHCaA",
        "version" : {
          "created" : "7050099"
        },
        "provided_name" : "lqz"
      }
    }
  }
}
```

