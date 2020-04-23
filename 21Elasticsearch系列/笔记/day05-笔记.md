

# 今日内容

## 1 python操作es

```python
# 两个模块：
# elasticsearch：官方给的 Official Python low-level client for Elasticsearch   :https://github.com/elastic/elasticsearch-py
学习文档：https://elasticsearch-py.readthedocs.io/en/master/
# elasticsearch-dsl：基于elasticsearch封装的:High level Python client for Elasticsearch 
https://github.com/elastic/elasticsearch-dsl-py
    https://elasticsearch-dsl.readthedocs.io/en/latest/
```

```python


# elasticserarch

#pymysql  和mysql
#redis    和redis


from elasticsearch import Elasticsearch
es = Elasticsearch(hosts=[{"host": "localhost", "port": 9200},{"host": "localhost", "port": 9200}])

# 创建索引
# es.indices.create(index='test-index')
#
# result = es.indices.delete(index='user', ignore=[400, 404])

# ignore 404 and 400
# es.indices.delete(index='test-index', ignore=[400, 404])

# 插入文档
#
# data = {'userid': '1', 'username': 'lqz','password':'123'}
# result = es.create(index='news', doc_type='doc', id=1, body=data)
# # 删除数据
# result = es.delete(index='news', doc_type='doc', id=1)



# 查询
# 查找所有文档
# 所有查询方式完全一样，只需要，会写字典就可以了（分页，高亮，过滤）
query = {'query': {'match_all': {}}}
#  查找名字叫做jack的所有文档
# query = {'query': {'term': {'username': 'lqz'}}}

# 查找年龄大于11的所有文档
# query = {'query': {'range': {'age': {'gt': 11}}}}

allDoc = es.search(index='news', doc_type='doc', body=query)
print(allDoc['hits']['hits'][0]['_source'])
```

```python
# elasticsearch-dsl
from datetime import datetime
from elasticsearch_dsl import Document, Date, Keyword, Text,Integer

from elasticsearch_dsl.connections import connections

# 创建连接（要在项目的某个位置执行）
connections.create_connection(hosts=["localhost"])


class Article(Document):
    title = Text(analyzer='ik_max_word',  fields={'title': Keyword()})  #mapping：对着官方文档看
    author = Text()
    def __str__(self):
        return self.title+self.author
    class Index:
        name = 'myindex'
    # def save(self, ** kwargs):
    #     return super(Article, self).save(** kwargs)


if __name__ == '__main__':
    # Article.init()  # 创建映射
    # 保存数据
    # article = Article()
    # article.title = "我这篇文章很牛逼啊"
    # article.author = "lqz"
    # article.save()  # 数据就保存了

    #查询数据
    # s=Article.search().filter('match', title="牛逼")
    # # s = s.filter('match', title="牛逼")
    #
    # results = s.execute()
    # print(results[0])

    #特别复杂查询怎么写？low-level的查
    # django orm实现不了的，用原生sql

    #删除数据
    # s = Article.search()
    # s = s.filter('match', title="牛逼").delete()

    #修改数据（跟orm一毛一样）
    # s = Article().search()
    # s = s.filter('match', title="测试")
    # results = s.execute()
    # print(results[0])
    # results[0].title="xxx"
    # results[0].save()
```



## 2 分布式id生成方案

```python
#之前写项目，id如何生成的？mysql自增   两台机器，两个数据，跑同样的代码，生成的id号可能会重复
#取当前时间+随机字符串   在不同的机器上，也可能会出现id重复

#生成不重复的id号
#全局唯一，趋势自增，信息安全， 低延迟，高可用  
#所有机器访问这一个mysql生成，延迟高，可用性低

QPS：Queries Per Second意思是“每秒查询率”，是一台服务器每秒能够相应的查询次数，是对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准。
TPS：是TransactionsPerSecond的缩写，也就是事务数/秒。它是软件测试结果的测量单位。一个事务是指一个客户机向服务器发送请求然后服务器做出反应的过程。客户机在发送请时开始计时，收到服务器响应后结束计时，以此来计算使用的时间和完成的事务个数


1 uuid (没有趋势递增)  :https://www.cnblogs.com/liuqingzheng/articles/9872350.html
2 mysql：有趋势自增，效率很低
3 redis：10w 
4 大公司在用的方案           -------了解（美团自己写了一个：美团Leaf）
	-雪花算法（跟语言没关系，任何语言都有方案）理论上snowflake方案的QPS约为409.6w/s
  -git找的别人开源的
  
 #你的简历，雪花算法，分布式id生成      项目描述中，公司一开始用的uuid，趋势自增，可能会出现重复----》我写的雪花算法
```

## 3 redis实现分布式锁

```python
秒杀场景：简历上；熟悉redis分布式锁的使用   （熟悉内部原理）
	-10商品    5台机器上  一台机器在改10的时候，其他不能改 -----》线程锁锁不住
	-mysql：乐观锁，悲观锁 -----》  分布式锁  ，性能低   mysql死锁---》表锁住了
  -redis实现分布式锁：https://www.cnblogs.com/liuqingzheng/p/11080501.html#_label1
  -在公司怎么用：redis分布式锁的实现--官方提供的python版本
  -https://github.com/SPSCommerce/redlock-py
    分布在不同机器上，用的都是同一把锁，
    -在干某个事之前，获取锁m：y_lock = dlm.lock("my_resource_name",1000)
    -干事   你的代码，干事
    -事干完了，释放锁：dlm.unlock(my_lock)
  	
```





## 4 haystack的使用

```python
#简历：通过haystack+es 实现全文检索 两年前用的，先在不用了自己封装的

# 自己通过es创建个索引，插入数据，取查询 标题，内容     大哥关键字   标题或者内容中----》json

https://www.cnblogs.com/xiaoyuanqujing/articles/11803376.html
 # django 框架的一个第三方模块：快速实现全文检索   haystack相当于django 的orm 做了一个封装，让我们更方便的使用es，solor，whoosh（不用它，完全没问题），用了，编码更简单

#docker image pull delron/elasticsearch-ik:2.4.6-1.0   2版本es，继承了ik    学的7 版本
#haystack最高支持到5，6，不支持7版本

pip3 install drf-haystack 
pip3 install elasticsearch
pip3 install djangorestframework




如果嫌麻烦，就用咱之前教的es操作，不需要借助haystack

```



# 补充

## 技术类博客，多去看：开源中国，csdn，cbblogs，开发者头条，掘金

## [Redis与Mysql双写一致性方案解析](https://www.cnblogs.com/liuqingzheng/p/11080680.html)

## [什么是分布式锁？实现分布式锁的三种方式 ](https://www.cnblogs.com/liuqingzheng/p/11080501.html)

           ##  [分布式系统全局唯一ID生成 ](https://www.cnblogs.com/liuqingzheng/p/11074623.html)         

  ##      [反向代理和正向代理 ](https://www.cnblogs.com/liuqingzheng/p/10521675.html)         

## [分库分表之终极设计方案 ](https://www.cnblogs.com/liuqingzheng/p/10755148.html)

  ## [对称加密和非对称加密 ](https://www.cnblogs.com/liuqingzheng/p/9760765.html)     

## 字符编码（https://www.cnblogs.com/xiaoyuanqujing/articles/12023669.html）

 ## [23种计模式之Python实现（史上最全最通俗易懂） ](https://www.cnblogs.com/liuqingzheng/p/10038958.html)         

## es数据和mysql同步

会自动做么？自己写代码实现，他们之间的数据同步

django框架中（不是所有表都放到es中），只检索文章 (user,tag)，es的表结构一般跟mysql表结构不一样，tag[]，分类，title，content

配置文件配置：

用代码实现，插入一篇文章，在es中写一条数据，视图函数中，Article.objects.create( )  立马调用es写入

重写Article     create方法  高端一些 同步操作（）

改成异步（celery），性能高了   又高端一些

最高端的-----用django的信号（异步的）跟flask一毛一样https://www.cnblogs.com/liuqingzheng/articles/9803403.html

干某个事会触发：在表中插入数据时（django的内置信号），写es

任意一个表create数据，都会走它

```
from django.db.models.signals import pre_save
import logging
def callBack(sender, **kwargs):
    print(sender)
    print(kwargs)
    # 创建对象写日志
    logging.basicConfig(level=logging.DEBUG)
    # logging.error('%s创建了一个%s对象'%(sender._meta.db_table,kwargs.get('instance').title))
    logging.debug('%s创建了一个%s对象'%(sender._meta.model_name,kwargs.get('instance').title))

pre_save.connect(callBack)
```

falsk信号：注册，调用（内置，自定义）

定时任务：celery（消息队列），APScheduler，每天晚上12点，同步数据







## 要反复润色你的简历

个人技能，项目经验，redis做了缓存，做了计数器，set去重（分布式的），持久化方案，为什么要用，因为一开始用**方案，es 整理一下，项目里

## 面试过程中怎么说

对你是有想法的，对你抱有期望值，（简历上写的东西你都准备了，降级，熔断：30个字，至少一万字）

1 个人介绍：

面试官你好，我叫xxx，我毕业于xx大学，省重点，，党员，（是就体现出来，不是就不要说），我大学的专业是 软件工程，我从几几年几月开始在XX公司做python后端开发，在公司做了多长时间，做了什么项目，有什么收获，你取得的成绩

后来又去了xx公司，做python爬虫，做了xxx项目，xx东西

说一，两个最具代表性的即可

说一个最具代表性的即可

项目升级，添加新功能（原来app，没有附近的好店的推荐），找解决方案，两种方案，redis的地理位置信息做，es地理微信信息来做，redis，两天的时间，实现了这个功能

爬取好大夫的  药品信息，医生信息，医疗名词解释  爬下来，搭一个服务，给分析部门用，web做的比较多，flask，搭了一个服务，页面是简单展示（vue，bootstrop+jq）主要是提供接口，scrapy爬的，scarpy-redis 分布式爬虫，xpath，css选择，pipline把数据持久化到mysql，es，mongodb  ，代理池（网上xx开源），cookie（我怎么搭建的），集成了selenium ，请求头里必须带什么，js加密，css加密，fiddler抓包， 10几万条数据mysql，比较慢（全文检索），es  ik，xxx，哪一年获得了公司的优秀新人，优秀员工，组里，涨工资最高的，

10分钟后

把你准备的东西说出来

drf：不管人家问什么，这小子都能把它扯到drf上

drf源码分析部分整理好了，    在开发中的收获？   遇到了什么问题？    我利用工作之余读了drf源码

django 中间了  session，common： 带斜杠和不带斜杆都行  127.0.0.1  重定向到了 127.0.0.1 /

Json 模块 3.6以后改了，json.dumps(字符串/bytes都支持)   ，git上拉下来，3.6上写的  ，python版本不一样，3.6  

vscode也开发过python

2   拿着你的简历上面的个人技能/项目里面的东西问（你简历上肯定有很多它不会的东西，但有很多它比你掌握的深的东西），只挑它会的，来问你，而且他可能比你懂得深，   你要让他觉得你好牛逼，什么都懂，可能有的地方掌握的不如他深，但是你比他知道的多

你的优势，一定要在面试过程中体现出来   一旦说出他不太懂的东西，你一定要不停的说，一旦看到他比你懂这个，扯到别的上面取

问你的问题，你真的答不上来，太正常了，（问你的问题，好多都是他从网上现搜的，他都不一定会）

在上家公司没有用过这个技术，东西，知道这个名词，掌握的不是特别好，我就知道他是干XX用的，它有XX东西

一点都不知道的，你反问他，这个东西干啥用的，（这是个数据库，做全文检索的），用过类似的，讲你用过的那个东西

听完，你知道是干啥的，但你没用过类似的，这玩意是不是跟redis的列表挺像的

3 问一些古怪的东西（它看到一个东西，会问一问你会不会，工作中碰到的问题，直接问你）

多半是回答不上来，它也没有期望你给一个明确的答案，它就是想看你知不知道，看看你如果不知道的反应是什么，

这东西我没有用过，我可以学，最多给我3天时间，我就可以搞定

算法问题，主要想考你逻辑思维能力---》现场发挥，你能想到什么样，就给他的答案，不管对错，都给他答案，这是你的想法，如果错了，它告诉你不对，之前我做东西，都是一边搜，一边解决，现在你问我的这个问题，我目前能够想到的答案就是这样，如果你给我点时间，我上网查一查，会有更好的解决方案

4 问项目（项目架构，开发周期，协同开发，跟前端如何交流，联调，怎么测试的，发布频率。。。。）

让你的前端项目app，小程序，直接连到你机器    前端的人把配置的地址改成你的，点击一下出bug的地方，断点，日志，打印，

5 生活相关，之前薪资。。。公积金基数是多少，公司几个人，你组几个人，怎么去公司

跟面试官认了老乡，爱好  。。。。。基本就成了

6 你有什么想问我们的吗？问公司情况，问公司项目，问公司用什么技术点，问出东西来，润色你的简历

面之前，应不应该搜一搜这家公司干啥的，天眼查  （面试过程中说出来，谁出来，咱公司是不是做数据分析，对的客户群体是什么呀，项目前景，盈利怎么样）
