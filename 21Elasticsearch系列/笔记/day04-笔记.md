



# 昨日回顾

```python
# 1 插件安装。ik  es7，目前用的不是特别多，最新的项目    -----》2，5，6
# 安装
    1 - download pre-build package from here: https://github.com/medcl/elasticsearch-analysis-ik/releases
    create plugin folder cd your-es-root/plugins/ && mkdir ik
    unzip plugin to folder your-es-root/plugins/ik
    2 - use elasticsearch-plugin to install ( supported from version v5.5.1 ):
    ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.3.0/elasticsearch-analysis-ik-6.3.0.zip
#2 中文分词器（es上中文分词基本上都用它），做中文分词

#3  映射（字段的属性）：如果不指定映射（也有映射，自动生成），手动指定：数字，字符串（分词，部分词），日期
	-创建：put index/_mapping
  			{映射}
  -查看：get index/_mapping
#4 文档的增删查改
	-增：put 索引/doc/id号    
  		{数据}           数据的字段可以随意（json），字段属性会自动创建
 -删：delete 索引/doc/id号   
 -查	：get 索引/doc/id号
 -改 ：put：修改，之前的没了
			post：局部修改，之前的不会动
    
#5 倒排索引
	-整理一下（100字左右）写到你1w字作文上，把倒排索引写到简历上


```

# 今日内容

## 0 如何提高网站并发量

```python
#1 静态文件（图片，js，css），不放在自己服务器，放到花钱买的服务器上（七牛云，阿里云的oss对象存储）使用cdn，单独自己搭建文件服务器（fastfds）
#2 精灵图（一个大图，只加载一次，定位）
#3 前端缓存：中间件：response[Cache-Control]="dasd"   Cache-Control: max-age=600

# nginx
	-nginx能不能挡住这么高的并发量
  	-挡不住：1 dns解析，不通的ip地址，访问不通的服务器
					  2 在nginx之前再当个负载均衡硬件（f5）---分发给10个nginx
      			3 限流：丢掉一些请求，访问不到了（服务器不会崩）
    -挡住了：
    	      1 uwsgi(部署)+django  用集群，增大机器性能
      			2 选用异步web框架：sanic、tornado  （django，3.0以前，flask ，都是同步框架）
# pythonweb
		-进项目了：
  				1 用缓存----》单机，redis集群，主从，不走mysql
    、		 2 用异步----》celery，rabbitmq  请求来了，直接丢到消息队列，直接返回，异步的去处理请求
    			3 es
    		  3 优化sql，优化代码（逻辑）
      		4 mysq主从做读写分离
        	5 项目控制，限流，降级
        
# 语言层面（解释性和编译型）
	-go，性能高
  
# 架构层面
	-微服务：限流，降级，熔断（了解限流，降级，熔断，写到简历上）
```

数据

```

PUT lqz/doc/1
{
  "name":"顾老二",
  "age":30,
  "from": "gu",
  "desc": "皮肤黑、武器长、性格直",
  "tags": ["黑", "长", "直"]
}



PUT lqz/doc/2
{
  "name":"大娘子",
  "age":18,
  "from":"sheng",
  "desc":"肤白貌美，娇憨可爱",
  "tags":["白", "富","美"]
}




PUT lqz/doc/3
{
  "name":"龙套偏房",
  "age":22,
  "from":"gu",
  "desc":"mmp，没怎么看，不知道怎么形容",
  "tags":["造数据", "真","难"]
}
PUT lqz/doc/4
{
  "name":"石头",
  "age":29,
  "from":"gu",
  "desc":"粗中有细，狐假虎威",
  "tags":["粗", "大","猛"]
}



PUT lqz/doc/5
{
  "name":"魏行首",
  "age":25,
  "from":"广云台",
  "desc":"仿佛兮若轻云之蔽月,飘飘兮若流风之回雪,mmp，最后竟然没有嫁给顾老二！",
  "tags":["闭月","羞花"]
}
```



## 1 文档基本查询

```python
# 增 删  查 改  curd
# 更新：两种

#查
	-字符串查询：GET lqz/doc/_search?q=from:gu
	-结构化查询
		GET lqz/doc/_search
			{
 				 "query": {
   			 "match": {
     		 	"desc": "粗"
    		}
  		}
		}
      
      
      
# 排序：升序，降序
# 排序(并不是所有字段都能排序)
#match_all 表示查询所有
GET lqz/doc/_search
{
  "query": {
    "match_all": {
    }
  },
  "sort": [
    {
      "age": {
        "order": "asc"
      }
    }
  ]
}

# 分页
# 分页，从第二条开始，取一条
 # "from": 1,从第几条开始
 #"size": 1   取的大小（多少）
GET lqz/doc/_search
{
  "query": {
    "match_all": {
    }
  },
  "sort": [
    {
      "age": {
        "order": "asc"
      }
    }
  ],
 "from": 1,
  "size": 1
}


# 查询结果过滤
# 只查询name和age
GET lqz/doc/_search
{
  "query": {
    "match_all": {
    }
  },

  "_source": ["name", "age"]
}


# 高亮：默认，自定义
# 默认查询结果高亮显示，用mach时会有
GET lqz/doc/_search
{
  "query": {
    "match": {
      "name":"石头"
    }
  },

   "highlight": {
    "fields": {
      "name": {}
    }
  }
}

# 自定义高亮
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from":"广云台"
    }
  },

  "highlight": {
    "pre_tags": "<b style='color:red'>",
    "post_tags": "</b>",
    "fields": {
      "from": {}
    }
  }
}

GET lqz/doc/_search
{
  "query": {
    "match": {
      "desc":"狐假虎威"
    }
  },

  "highlight": {
    "pre_tags": "<b style='color:red'>",
    "post_tags": "</b>",
    "fields": {
      "desc": {}
    }
  }
}



# 聚合查询
#聚合函数（min，max，avg sum）
GET lqz/doc/_search
{
  "query": {
    "match": {
      "from": "gu"
    }
  },
  "aggs": {
    "my_min": {
      "min": {
        "field": "age"
      }
    }
  },
  "_source": ["name", "age","from"]
}

# 分组查询，（用的较少，基本会全文检索）
#  "size": 0,不取数据，只要结果
#{
#    "from": 15,
#    "to": 20
#}, 从15到20分成一组
GET lqz/doc/_search
{
  "size": 0, 
  "query": {
    "match_all": {}
  },
  "aggs": {
    "age_group": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 15,
            "to": 20
          },
          {
            "from": 20,
            "to": 25
          },
          {
            "from": 25,
            "to": 30
          }
        ]
      }
    }
  }
}



```

## 2 文档组合查询（布尔查询）

```python
- must（and）
- should（or）
- must_not（not）
- filter




# 组合查询之must

GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "gu"
          }
        },
        {
          "match": {
            "age": 30
          }
        }
      ]
    }
  }
}
# 组合查询之should，或者

GET lqz/doc/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "from": "gu"
          }
        },
        {
          "match": {
            "tags": "闭月"
          }
        }
      ]
    }
  }
}

# 组合查询之must_not  都不是
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "from": "gu"
          }
        },
        {
          "match": {
            "tags": "可爱"
          }
        },
        {
          "match": {
            "age": 18
          }
        }
      ]
    }
  }
}

# 组合查询之filter  gt lt  gte  lte  
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "from": "gu"
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "lt": 25
          }
        }
      }
    }
  }
}
# 查询年龄小于25的
GET lqz/doc/_search
{
  "query": {
    "bool": {
      "filter": {
        "range": {
          "age": {
            "lt": 25
          }
        }
      }
    }
  }
}

总结：
- `must`：与关系，相当于关系型数据库中的`and`。
- `should`：或关系，相当于关系型数据库中的`or`。
- `must_not`：非关系，相当于关系型数据库中的`not`。
- `filter`：过滤条件。
- `range`：条件筛选范围。
- `gt`：大于，相当于关系型数据库中的`>`。
- `gte`：大于等于，相当于关系型数据库中的`>=`。
- `lt`：小于，相当于关系型数据库中的`<`。
- `lte`：小于等于，相当于关系型数据库中的`<=`。
```



## 3 集群搭建

```python
# 广播
# 搭一个4个es的集群
#1 把es文件夹复制四份
#2 启动四次即可
（一般不用）



#单播（用的最多）
配置文件
D:\elasticsearch750\elasticsearch-7.5.0\config\elasticsearch.yml
#1 elasticsearch1节点，,集群名称是my_es1,集群端口是9300；节点名称是node1，监听本地9200端口，可以有权限成为主节点和读写磁盘（不写就是默认的）。
#node.master: true可以有权限成为主节#node.data: true点和读写磁盘。
cluster.name: my_es1
node.name: node1
network.host: 127.0.0.1
http.port: 9200
transport.tcp.port: 9300
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

# 2 elasticsearch2节点,集群名称是my_es1,集群端口是9302；节点名称是node2，监听本地9202端口，node.master: true可以有权限成为主节node.data: true点和读写磁盘。

cluster.name: my_es1
node.name: node2
network.host: 127.0.0.1
http.port: 9202
transport.tcp.port: 9302
node.master: true
node.data: true
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

# 3 elasticsearch3节点，集群名称是my_es1,集群端口是9303；节点名称是node3，监听本地9203端口，可以有权限成为主节点和读写磁盘。

cluster.name: my_es1
node.name: node3
network.host: 127.0.0.1
http.port: 9203
transport.tcp.port: 9303
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

# 4 elasticsearch4节点，集群名称是my_es1,集群端口是9304；节点名称是node4，监听本地9204端口，仅能读写磁盘而不能被选举为主节点。

cluster.name: my_es1
node.name: node4
network.host: 127.0.0.1
http.port: 9204
transport.tcp.port: 9304
node.master: false
node.data: true
discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300", "127.0.0.1:9302", "127.0.0.1:9303", "127.0.0.1:9304"]

由上例的配置可以看到，各节点有一个共同的名字my_es1,但由于是本地环境，所以各节点的名字不能一致，我们分别启动它们，它们通过单播列表相互介绍，发现彼此，然后组成一个my_es1集群。谁是老大则是要看谁先启动了！

```



## 4 补充（mapping，ik分词）

```python
#分词 查看   上海自来水来自海上   被ik分成什么样

# 搜   自海   搜不到
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "上海自来水来自海上"
}


GET _analyze
{
  "analyzer": "ik_smart",
  "text": "上海自来水来自海上"
}
```

```python
# 创建映射时，指定字段用哪个分词器
PUT books
{
  "mappings": {
    "properties":{
      "title":{
        "type":"text",
        "analyzer": "ik_max_word"
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
```









