# 昨日回顾

```python
# es是什么？分布式的全文检索引擎（存数据的地方，数据库）
# es用来干什么？你的项目里哪个地方用了它。只要涉及到搜索，就会用
# es原理：基于lucence，它是java的一个做全文检索的模块，只能java用，比较难，于是有人封装了一下，做成服务，基于resful规范
# es跟solr比较：都是全文检索引擎，只是不同公司出的，es可扩展性高，solr实时性es高，    mysql和oracle的区别
# es概念：集群，节点，分片，副本，全文检索  保证了es高可用
# es跟mysql比较：
	索引----》数据库
  类型----》表
  文档----》一行数据
  mapping---》列的类型
  
# 谁再用？github。。。

# haystack：django框架上的一个搜索模块，底层的库，使用下面几种，它相当于django的orm
https://www.cnblogs.com/ay742936292/p/11165876.html
    
	-目前只支持es 2.几的版本（7.多的不支持）
	-whoosh：简单一些，python写的全文检索引擎（sqlite）
  -solr（oracle）
  -elasticsearch（mysql）
  
# celery：小组织写的，作者说不支持windows平台（测试如果有问题，就不要再windows上使用了）


# 安装kibana和elasticsearch-head
	-kibana---es官方提供的（es客户端）
  	-去官网（下载对应版本，跟es对应起来），解压
    -执行bin/bibana   启动起来，在浏览器访问
  -elasticsearch-head--第三方用nodjs写的（es客户端）
  	-安装node.js                （类似于安装python解释器）
  	-去git上下载（开源的），解压
    -安装依赖模块 npm install   （类似于安装模块，pip3 install -r requestsments.txt）
    -num run start             (django写了个服务，run server)
    -在浏览器访问
    -注意配置跨越问题
  -postman也可以（只要能做resful交互）
  
 # 越大的公司用的技术越老 ssh      ----否则你的路一定会越走越窄    ----太正常了
#  业务   大部分公司都是这样，  你出去找工作的时候，一定要有信心  你的技术能力一点都不比他们差
#  公司用go，给你多上时间，你能上手  1个周  （干点活）


#润色你的简历（看到一个技术，忽然想起来，融到你的简历里，个人技能，项目）
跨域问题，同源策略，简单请求非简答请求，如何处理
session token

详细：curd，分组查询，组合查询，子查询


# psutil：用来做监控---devops项目（cmdb资产收集，代码发布，批量命令，日志处理，服务器监控：前端用echars展示cpu的占用，进程占用内存情况，从高到底排序，点X，关闭）
https://www.liaoxuefeng.com/wiki/1016959663602400/1183565811281984
```



# 今日内容



## 1 倒排索引

详见md文件

elk：自己去学，报linux周末班

## 2 索引操作

```python
# 创建lqz索引（数据，创建数据库）
PUT lqz
# 查看索引(所有信息)
GET lqz
# 查看索引配置信息
GET lqz/_settings

# 更新索引（修改配置信息）
PUT lqz/_settings
{
  "number_of_replicas": 2
}

DELETE lqz
```



## 3 映射管理（字段，属性）

```python
#在Elasticsearch 6.0.0或更高版本中创建的索引只包含一个mapping type。 在5.x中使用multiple mapping types创建的索引将继续像以前一样在Elasticsearch 6.x中运行。 Mapping types将在Elasticsearch 7.0.0中完全删除
# 一个数据库下只能创建一个表

# 映射就是字段的类型和属性（刚刚插入文档时，没有指定类型和属性，会默认给你指定）


# 映射
# 字段类型，属性



# 1 查看索引映射
GET lqz2/_mapping
# long  类型，数字类型



#2  创建一个books索引
# 创建类型：
#title：text类型
#price：integer类型
#addr：keyword类型
# company：{name:text,company_addr:text,employee_count:数字}
#publish_date：日期类型，格式：yyy-MM-dd
PUT books
{
  "mappings": {
    "properties":{
      "title":{
        "type":"text"
      },
      "price":{
        "type":"integer"
      },
      "addr":{
        "type":"keyword"
      },
      "company":{
        "properties":{
          "name":{"type":"text"},
          "company_addr":{"type":"text"},
          "employee_count":{"type":"integer"}
        }
      },
      "publish_date":{"type":"date","format":"yyy-MM-dd"}
      
    }
    
  }
}

#查看books的映射（不需要记，copy修改即可）
GET books/_mapping



# 3 测试数据
PUT books/_doc/1
{
  "title":"大头儿子小偷爸爸",
  "price":100,  
  "addr":"北京天安门",
  "company":{
    "name":"我爱北京天安门",
    "company_addr":"我的家在东北松花江傻姑娘",
    "employee_count":10
  },
  "publish_date":"2019-08-19"
}


# 查询文档
GET books/_doc/1

```



## 4 文档操作（一行行数据）

```python
# 不需要提前创建索引和类型，直接新增文档（自动创建出索引和类型），跟mongodb 是一样的


# 对应到mysql--一行行的数据

# 增加文档
#文档新增(索引没有，类型没有，也可以直接新增文档（跟mongo很像）)
'''格式
PUT 索引(数据库)/类型（表）/id号
{
  "name":"lqz",
  "age":18
}
'''

##注意事项：
# 在es7.0以后，一个索引只能创建一个类型（一个数据库，只能创建一个表）
# 在es6.0以前，是可以创建多个，6.0 正好支持多个，和一个，如果创建多个，他会给你一个警告

# 熟悉es6.0以前版本和7.0以上版本的区别

# 新增一条文档（一条数据）
PUT lqz2/doc/1
{
  "name":"lqz",
  "age":18
}
# 每个文档的字段，可以不一致（非关系型：mongodb）
# 字段没有就是没有，（不是有，值为空）
PUT lqz2/doc/2
{
  "name":"egon",
  "wife":"凤姐",
  "address":"山东烟台"
}


# 查询
GET lqz2/doc/1
{
  "_index" : "lqz2",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "lqz",
    "age" : 18
  }
}

GET lqz2/doc/3
{
  "_index" : "lqz2",
  "_type" : "doc",
  "_id" : "3",
  "found" : false
}

#修改（两种方式）
#id为1的文档名字，这种修改，原来的字段没有了
PUT lqz2/doc/1
{
  "name":"dsb"
}
GET lqz2/doc/1

# 通过post  带上_update   文档中带doc
# 局部更新
POST lqz2/_update/2
{
  "doc":{
    "name":"xxx"
  }
}


#删除
DELETE lqz2/doc/1
```



## 5 插件安装

```python
#es可扩展性很高，支持很多插件，ik就是其中一个插件
#es如何安装插件
	-三种方案
  	-1 命令行方式：bin/elasticsearch-plugin install  插件名字
    -2 url安装：  bin/elasticsearch-plugin install [url]
    -3 离线安装：
    	#点击下载analysis-smartcn离线包
			#将离线包解压到ElasticSearch 安装目录下的 plugins 目录下
			#重启es。新装插件必须要重启es   
```



## 6 中文分词器 ik

三个方法

1.可以自己写

2.jieba库

2.ik （是es作者公布写的）

```python
# 可扩展性高，咱可以使用第三方的分词器
# 输入一篇文章要保存，涉及到分词，然后建立倒排索引，分词的好坏，涉及到es是否好用
#默认情况下，es，分词都是分英文 按空格分词
# 涉及到中文，比较复杂，ik（es作者写的）


# IK Analyzer是一个开源的，基于java语言开发的轻量级的中文分词工具包
# https://github.com/medcl/elasticsearch-analysis-ik

# 一定要注意对应版本：https://github.com/medcl/elasticsearch-analysis-ik/releases
#点击下载https://github.com/medcl/elasticsearch-analysis-ik离线包
#将离线包解压到ElasticSearch 安装目录下的 plugins 目录下
#重启es。新装插件必须要重启es   
```









# 补充

## 1 伪静态：

```python
# 咱么做的网站利于搜索引擎抓取,
# 百度这个大爬虫，一刻不停的取网上抓取网页，存到它数据库中
# 等级：静态页面被抓取的概率高（静态页面不会改变），/user  被抓取的概率低，   /user.htm
# 把地址存到它库中的概率高
# 百度快照，就是当时百度抓取页面时，页面的内容，
# 你去搜，输入关键字，能搜到页面，打开页面，发现，关键字没有，

```











