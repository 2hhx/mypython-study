[TOC]



# 一般操作

在进行一般操作时先配置一下参数，使得我们可以直接在Django页面中运行我们的测试脚本

# 在Python脚本中调用Django环境

## 模型转为mysql数据库中的表settings配置

需要在settings中配置：



```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'tabledatabase',
        'USER':'root',
        'PASSWORD':'root',
        'HOST':'127.0.0.1',
        'PORT':3306,
        'CHARSET':'utf8',
        'ATOMIC_REQUEST': True,
        'OPTIONS': {
            "init_command": "SET storage_engine=MyISAM",
        }
    }
}
'''
'NAME':要连接的数据库，连接前需要创建好
'USER':连接数据库的用户名
'PASSWORD':连接数据库的密码
'HOST':连接主机，默认本机
'PORT':端口 默认3306
'CHARSET':'utf8',字符编码默认utf8
'ATOMIC_REQUEST': True,
设置为True统一个http请求对应的所有sql都放在一个事务中执行（要么所有都成功，要么所有都失败）。
是全局性的配置， 如果要对某个http请求放水（然后自定义事务），可以用non_atomic_requests修饰器 
'OPTIONS': {
             "init_command": "SET storage_engine=MyISAM",
            }
设置创建表的存储引擎为MyISAM，INNODB
'''
```



**注意1：**NAME即数据库的名字，在mysql连接前该数据库必须已经创建，而上面的sqlite数据库下的db.sqlite3则是项目自动创建 USER和PASSWORD分别是数据库的用户名和密码。设置完后，再启动我们的Django项目前，我们需要激活我们的mysql。然后，启动项目，会报错：no module named MySQLdb 。这是因为django默认你导入的驱动是MySQLdb，可是MySQLdb 对于py3有很大问题，所以我们需要的驱动是PyMySQL 所以，我们只需要找到项目名文件下的__init__,在里面写入：

```
import pymysql
pymysql.install_as_MySQLdb()
```

最后通过两条数据库迁移命令即可在指定的数据库中创建表 ：

```
python manage.py makemigrations
python manage.py migrate
```

**注意2:**确保配置文件中的INSTALLED_APPS中写入我们创建的app名称



```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    "app01"
]
```



**注意3:**如果报错如下：

```
django.core.exceptions.ImproperlyConfigured: mysqlclient 1.3.3 or newer is required; you have 0.7.11.None
```

MySQLclient目前只支持到python3.4，因此如果使用的更高版本的python，需要修改如下：

通过查找路径C:\Programs\Python\Python36-32\Lib\site-packages\Django-2.0-py3.6.egg\django\db\backends\mysql
这个路径里的文件把

```
if version < (1, 3, 3):
     raise ImproperlyConfigured("mysqlclient 1.3.3 or newer is required; you have %s" % Database.__version__)
```

注释掉就可以了

## 测试文件tests.py的使用和配置

![img](https://images2018.cnblogs.com/blog/1342004/201806/1342004-20180620150259388-1532214705.png)

```
import os
if __name__ == '__main__':
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "untitled15.settings")
    import django
    django.setup()

    from app01 import models

    books = models.Book.objects.all()
    print(books)
```



这样就可以直接运行你的test.py文件来运行测试

## Django终端打印SQL语句settings中配置

如果你想知道你对数据库进行操作时，Django内部到底是怎么执行它的sql语句时可以加下面的配置来查看

在Django项目的settings.py文件中，在最后复制粘贴如下代码：



```
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}
```



配置好之后，再执行任何对数据库进行操作的语句时，会自动将Django执行的sql语句打印到pycharm终端上

**补充：**

除了配置外，还可以通过一点.query即可查看查询语句，具体操作如下：

![img](https://images2018.cnblogs.com/blog/1342004/201807/1342004-20180707191146266-2051634567.png)

# 单表的数据准备

操作下面的操作之前，我们实现创建好了数据表，这里主要演示下面的操作，不再细讲创建准备过程

```python
python manage.py makemigrations
python manage.py migrate
```



```python
.models.py
from django.db import models

# Create your models here.
from django.db import models


class Book(models.Model):
    title = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=8, decimal_places=2)
    publish_data = models.DateField()

    def __str__(self):
        return self.title
   
```

```python
tests.py
import os

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "onetable.settings")
    import django

    django.setup()
```



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191024193332760-1830955373.png)

## 单表操作增删改

```python
from oneapp import models
import datetime

增加
res = models.Book.objects.create(title='python',price=121,publish_data=datetime.datetime.now())
print(res)
res1 = models.Book.objects.create(title='python',price=121,publish_data='2019-5-4')
print(res1)
book_obj = models.Book(title='math',price=120,publish_data='2019-4-7')

book_obj.save()
删除
res = models.Book.objects.filter(title='python').delete()
print(res)
修改
res = models.Book.objects.filter(pk=3).update(title='linux')
print(res)
book_obj= models.Book.objects.filter(pk=5).first()
book_obj.title = 'Go'
book_obj.save()

```



## 必知必会单表查询13条



**<1> all(): 查询所有结果**

```
 	1
    res = models.Book.objects.all()
    print(res)
```

**<2> filter(\**kwargs): 它包含了与所给筛选条件相匹配的对象**



```
 2
 res = models.Book.objects.filter()
    print(res)
    res = models.Book.objects.filter(title='python')
    print(res.first().price)
```



**<3> get(\**kwargs): 返回与所给筛选条件相匹配的对象，返回结果有且只有一个，如果符合筛选条件的对象超过一个或者没有都会抛出错误。**



```
 3
    res = models.Book.objects.get(title='python')
    print(res)
```



**<4> exclude(\**kwargs): 它包含了与所给筛选条件不匹配的对象**



```
  4
    res = models.Book.objects.exclude(id=4)
    print(res)
```



**<5> values(\*field): 返回一个ValueQuerySet——一个特殊的**

**QuerySet，运行后得到的并不是一系列model的实例化对象，而是一个可迭代的字典序列**



```
5
    res = models.Book.objects.values('title')
    print(res)

```



**<6> values_list(\*field): 它与values()非常相似，它返回的是一个元组序列，values返回的是一个字典序列**



```
6
    res = models.Book.objects.values_list('title')
    print(res,type(res.first()))
```



**<7> order_by(\*field): 对查询结果排序**



```
7
    res = models.Book.objects.order_by('price')#默认是升序
    print(res)
    res = models.Book.objects.order_by('-price')
    print(res)
```



**<8> reverse(): 对查询结果反向排序，请注意reverse()通常只能在具有已定义顺序的QuerySet上调用(在model类的Meta中指定ordering或调用order_by()方法)。**不能传值



```
8
    print(models.Book.objects.order_by('price'))
    res = models.Book.objects.order_by('price').reverse()
    print(res)

```



**<9> distinct(): 从返回结果中剔除重复纪录(如果你查询跨越多个表，可能在计算QuerySet时得到重复的结果。此时可以使用distinct()，注意只有在PostgreSQL中支持按字段去重。)**不能传值



```
9
    res = models.Book.objects.all().distinct()
    print(res)
```



**<10> count(): 返回数据库中匹配查询(QuerySet)的对象数量。**不能传值



```
10
    res = models.Book.objects.all().count()
    print(res)

```



**<11> first(): 返回第一条记录**



```
11
    res = models.Book.objects.all().first()
    print(res)
```



**<12> last(): 返回最后一条记录**



```
12
    res = models.Book.objects.all().last()
    print(res)
```



**<13> exists(): 如果QuerySet包含数据，就返回True，否则返回False**，不能传值



```
13
    res = models.Book.objects.all().exists()
    print(res)

```





## 13个必会单表操作方法总结

**返回QuerySet对象的方法有**

all()查询所有结果

filter()它包含了与所给筛选条件相匹配的对象，不加条件的时候返回所有。和all()相同。

exclude()它包含了与所给筛选条件不匹配的对象

order_by()对查询结果排序('-id')

reverse()对查询结果反向排序，对查询结果反向排序，请注意reverse()通常只能在具有已定义顺序的QuerySet上调用(在model类的Meta中指定ordering或调用order_by()方法)。

distinct()从返回结果中剔除重复纪录

**特殊的QuerySet**

values()       返回一个可迭代的字典序列，返回一个ValueQuerySet——一个特殊的QuerySet，运行后得到的并不是一系列model的实例化对象，而是一个可迭代的字典序列，获取所有的值，以字典的形式打印

values_list() 返回一个可迭代的元祖序列，它与values()非常相似，它返回的是一个元组序列。

**返回具体对象的**

get() 返回与所给筛选条件相匹配的对象，返回结果有且只有一个，如果符合筛选条件的对象超过一个或者没有都会抛出错误。

first() 返回第一条记录

last()返回最后一条记录

**返回数字的方法有**

count()返回数据库中匹配查询(QuerySet)的对象数量。

**返回布尔值的方法有：**

exists()如果QuerySet包含数据，就返回True，否则返回False

**不能传值的有**

count(),first(),last(),exists(),distinct(),all()

## 单表查询之神奇的双下划线　

```
查询价格大于200的书籍
res = models.Book.objects.filter(price__gt=200)
查询价格小于200的书籍
res = models.Book.objects.filter(price__lt=200)
查询价格大于或者等于200的书籍
res = models.Book.objects.filter(price__gte=200)
res1 = models.Book.objects.filter(price__lte=200)

价格是200 或者是123.23 或者666.66
res = models.Book.objects.filter(price__in=[200,123.23,666.66])
价格在200 到700之间的书籍
res = models.Book.objects.filter(price__range=(200,666.66))# 顾头不顾尾
res = models.Book.objects.filter(price__range=[10, 200])
```

### 模糊匹配

```
# 模糊匹配
"""
like
    %
    _
"""
查询书籍名称中包含p的
res = models.Book.objects.filter(title__contains='p')  # 区分大小写
忽略大小写
res = models.Book.objects.filter(title__icontains='p')  # 忽略大小写


查询书籍名称是以三开头的书籍
res = models.Book.objects.filter(title__startswith='p')
res1 = models.Book.objects.filter(title__endswith='h')
print(res1)
查询出版日期是2019年的书籍
res = models.Book.objects.filter(publish_date__year='2019')
查询出版日期是10月的书籍
res = models.Book.objects.filter(publish_date__month='10')
```

### date时间操作

```python
Book.objects.filter(pub_date__year=2012)
date字段还可以：
models.Book.objects.filter(first_day__year=2017)
date字段可以通过在其后加__year,__month,__day等来获取date的特点部分数据
# date
    # Book.objects.filter(pub_date__date=datetime.date(2005, 1, 1))

    # Book.objects.filter(pub_date__date__gt=datetime.date(2005, 1, 1))
    # year
    #
    # Book.objects.filter(pub_date__year=2005)
    # Book.objects.filter(pub_date__year__gte=2005)

    # month
    #
    # Book.objects.filter(pub_date__month=12)
    # Book.objects.filter(pub_date__month__gte=6)

    # day
    #
    # Book.objects.filter(pub_date__day=3)
    # Book.objects.filter(pub_date__day__gte=3)

    # week_day
    #
    # Book.objects.filter(pub_date__week_day=2)
    # Book.objects.filter(pub_date__week_day__gte=2)
需要注意的是在表示一年的时间的时候，我们通常用52周来表示，因为天数是不确定的，老外就是按周来计算薪资的哦~
```

# ForeignKey操作

## 正向查找(两种方式)

### 1.对象查找（跨表）

语法：

**对象****.****关联字段****.****字段**

要点:先拿到对象，再通过对象去查对应的外键字段，分两步

示例：

```
book_obj = models.Book.objects.first()  # 第一本书对象(第一步)
print(book_obj.publisher)  # 得到这本书关联的出版社对象
print(book_obj.publisher.name)  # 得到出版社对象的名称
```

### 2.字段查找（跨表）

语法：

**关联字段****__****字段**

要点:利用Django给我们提供的神奇的双下划线查找方式

示例：

```
models.Book.objects.all().values("publisher__name")
#拿到所有数据对应的出版社的名字，神奇的下划线帮我们夸表查询
```

## 反向操作(两种方式)

### 1.对象查找

语法：

**obj.表名_set**

要点:先拿到外键关联多对一，一中的某个对象，由于外键字段设置在多的一方，所以这里还是借用Django提供的双下划线来查找

示例：

```
publisher_obj = models.Publisher.objects.first()  # 找到第一个出版社对象
books = publisher_obj.book_set.all()  # 找到第一个出版社出版的所有书
titles = books.values_list("title")  # 找到第一个出版社出版的所有书的书名
```

结论:如果想通过一的那一方去查找多的一方，由于外键字段不在一这一方，所以用__set来查找即可

### 2.字段查找

语法：

**表名__字段**

要点:直接利用双下滑线完成夸表操作

```
titles = models.Publisher.objects.values("book__title")
```

# ManyToManyField

## class RelatedManager

"关联管理器"是在一对多或者多对多的关联上下文中使用的管理器。

它存在于下面两种情况：

1. 外键关系的反向查询
2. 多对多关联关系

简单来说就是在多对多表关系并且这一张多对多的关系表是有Django自动帮你建的情况下，下面的方法才可使用。

### 方法，add(),set(),remove(),clear()

**create()**

创建一个关联对象，并自动写入数据库，且在第三张双方的关联表中自动新建上双方对应关系。

```
models.Author.objects.first().book_set.create(title="偷塔秘籍")
上面这一句干了哪些事儿:
1.由作者表中的一个对象跨到书籍比表
2.新增名为偷塔秘籍的书籍并保存
3.到作者与书籍的第三张表中新增两者之间的多对多关系并保存
```

**add()**

add() 括号内既可以传数字也可以传数据对象,并且都支持传多个

把指定的model对象添加到第三张关联表中。

添加对象,添加id

```python
book_obj = models.Book.objects.filter(pk=3).first()
print(book_obj.authors)  # 就相当于 已经在书籍和作者的关系表了
book_obj.authors.add(1)
book_obj.authors.add(2,3)
author_obj = models.Author.objects.filter(pk=1).first()
author_obj1 = models.Author.objects.filter(pk=2).first()
book_obj.authors.add(author_obj)
book_obj.authors.add(author_obj,author_obj1)
```



**set()**

set() 括号内 既可以传数字也传对象 
    并且也是支持传多个的
    但是需要注意 括号内必须是一个可迭代对象

更新某个对象在第三张表中的关联对象。不同于上面的add是添加,set相当于重置，可以添加对象和id

```
book_obj = models.Book.objects.filter(pk=3).first()
book_obj.authors.set([3,])
book_obj.authors.set([1,3])
author_obj = models.Author.objects.filter(pk=1).first()
author_obj1 = models.Author.objects.filter(pk=2).first()
book_obj.authors.set((author_obj,))
book_obj.authors.set((author_obj,author_obj1))
```

**remove()**

remove() 括号内 既可以传数字也传对象 并且也是支持传多个的

从关联对象集中移除执行的model对象(移除对象在第三张表中与某个关联对象的关系)

```
book_obj = models.Book.objects.filter(pk=3).first()
book_obj.authors.remove(2)
book_obj.authors.remove(1,2)

author_obj = models.Author.objects.filter(pk=1).first()
author_obj1 = models.Author.objects.filter(pk=2).first()

book_obj.authors.remove(author_obj)

book_obj.authors.remove(author_obj,author_obj1)
```

**clear()**

clear()括号内不需要传任何参数 直接清空当前书籍对象所有的记录

从关联对象集中移除一切对象。(移除所有与对象相关的关系信息)

```
book_obj = models.Book.objects.filter(pk=3).first()
book_obj.authors.clear()
```

注意：

对于ForeignKey对象，clear()和remove()方法仅在null=True时存在。

**举个例子：**

ForeignKey字段没设置null=True时。

```
class Book(models.Model):
    title = models.CharField(max_length=32)
    publisher = models.ForeignKey(to=Publisher)
```

没有clear()和remove()方法：

```
>>> models.Publisher.objects.first().book_set.clear()
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AttributeError: 'RelatedManager' object has no attribute 'clear'
```

当ForeignKey字段设置null=True时，

```
class Book(models.Model):
    name = models.CharField(max_length=32)
    publisher = models.ForeignKey(to=Class, null=True)
```

此时就有clear()和remove()方法：

```
>>> models.Publisher.objects.first().book_set.clear()
```

再次强调：

1. 对于所有类型的关联字段，add()、create()、remove()和clear(),set()都会马上更新数据库。换句话说，在关联的任何一端，都不需要再调用save()方法。

实例：我们来假定下面这些概念，字段和关系

作者模型：一个作者有姓名和年龄。

作者详细模型：把作者的详情放到详情表，包含生日，手机号，家庭住址等信息。作者详情模型和作者模型之间是一对一的关系（one-to-one）

出版商模型：出版商有名称，所在城市以及email。

书籍模型： 书籍有书名和出版日期，一本书可能会有多个作者，一个作者也可以写多本书，所以作者和书籍的关系就是多对多的关联关系(many-to-many);一本书只应该由一个出版商出版，所以出版商和书籍是一对多关联关系(one-to-many)。

**注意：关联字段与外键约束没有必然的联系（建管理字段是为了进行查询，建约束是为了不出现脏数据）**

在Models创建如下模型



```
class Book(models.Model):
    nid = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    publish_date = models.DateField()
    # 阅读数
    # reat_num=models.IntegerField(default=0)
    # 评论数
    # commit_num=models.IntegerField(default=0)

    publish = models.ForeignKey(to='Publish',to_field='nid',on_delete=models.CASCADE)
    authors=models.ManyToManyField(to='Author')
    def __str__(self):
        return self.name


class Author(models.Model):
    nid = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    age = models.IntegerField()
    author_detail = models.OneToOneField(to='AuthorDatail',to_field='nid',unique=True,on_delete=models.CASCADE)


class AuthorDatail(models.Model):
    nid = models.AutoField(primary_key=True)
    telephone = models.BigIntegerField()
    birthday = models.DateField()
    addr = models.CharField(max_length=64)


class Publish(models.Model):
    nid = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    city = models.CharField(max_length=32)
    email = models.EmailField()
```

注意事项：

- 表的名称`myapp_modelName`，是根据 模型中的元数据自动生成的，也可以覆写为别的名称　　
- ` id` 字段是自动添加的
- 对于外键字段，Django 会在字段名上添加`"_id"` 来创建数据库中的列名
- 这个例子中的CREATE TABLE SQL 语句使用PostgreSQL 语法格式，要注意的是Django 会根据settings 中指定的数据库类型来使用相应的SQL 语句。
- 定义好模型之后，你需要告诉Django _使用_这些模型。你要做的就是修改配置文件中的INSTALL_APPSZ中设置，在其中添加`models.py`所在应用的名称。
- 外键字段 ForeignKey 有一个 null=True 的设置(它允许外键接受空值 NULL)，你可以赋给它空值 None 。



## 二 添加表记录

### 一对多的

```
方式1:
   publish_obj=Publish.objects.get(nid=1)
   book_obj=Book.objects.create(title="",publishDate="2012-12-12",price=100,publish=publish_obj)
  
方式2:
   book_obj=Book.objects.create(title="",publishDate="2012-12-12",price=100,publish_id=1)
```

核心：book_obj.publish与book_obj.publish_id是什么？ 

 



```
   关键点:

    一 book_obj.publish=Publish.objects.filter(id=book_obj.publish_id).first()

    二 book_obj.authors.all()
       关键点:book.authors.all()  # 与这本书关联的作者集合

        1 book.id=3
        2 book_authors
            id  book_id  author_ID
            3      3             1
            4      3             2

        3  author
           id   name
           1   alex
           2   egon

    book_obj.authors.all()    ------->   [alex,egon]
```



```
    # -----一对多添加
    pub=Publish.objects.create(name='egon出版社',email='445676@qq.com',city='山东')
    print(pub)

    # 为book表绑定和publish的关系
    import datetime,time
    now=datetime.datetime.now().__str__()
    now = datetime.datetime.now().strftime('%Y-%m-%d')
    print(type(now))
    print(now)
    # 日期类型必须是日期对象或者字符串形式的2018-09-12（2018-9-12），其它形式不行
    Book.objects.create(name='海燕3',price=333.123,publish_date=now,publish_id=2)
    Book.objects.create(name='海3燕3',price=35.123,publish_date='2018/02/28',publish=pub)
    pub=Publish.objects.filter(nid=1).first()
    book=Book.objects.create(name='测试书籍',price=33,publish_date='2018-7-28',publish=pub)
    print(book.publish.name)
    # 查询出版了红楼梦这本书出版社的邮箱
    book=Book.objects.filter(name='红楼梦').first()
    print(book.publish.email)
```



### 多对多



```
  # 当前生成的书籍对象
    book_obj=Book.objects.create(title="追风筝的人",price=200,publishDate="2012-11-12",publish_id=1)
    # 为书籍绑定的做作者对象
    yuan=Author.objects.filter(name="yuan").first() # 在Author表中主键为2的纪录
    egon=Author.objects.filter(name="alex").first() # 在Author表中主键为1的纪录

    # 绑定多对多关系,即向关系表book_authors中添加纪录
    book_obj.authors.add(yuan,egon)    #  将某些特定的 model 对象添加到被关联对象集合中。   =======    book_obj.authors.add(*[])
```







```
    book = Book.objects.filter(name='红楼梦').first()
    egon=Author.objects.filter(name='egon').first()
    lqz=Author.objects.filter(name='lqz').first()
    # 1 没有返回值，直接传对象
    book.authors.add(lqz,egon)
    # 2 直接传作者id
    book.authors.add(1,3)
    # 3 直接传列表,会打散
    book.authors.add(*[1,2])
    # 解除多对多关系
    book = Book.objects.filter(name='红楼梦').first()
    # 1 传作者id
    book.authors.remove(1)
    # 2 传作者对象
    egon = Author.objects.filter(name='egon').first()
    book.authors.remove(egon)
    #3 传*列表
    book.authors.remove(*[1,2])
    #4 删除所有
    book.authors.clear()
    # 5 拿到与 这本书关联的所有作者，结果是queryset对象，作者列表
    ret=book.authors.all()
    # print(ret)
    # 6 queryset对象，又可以继续点（查询红楼梦这本书所有作者的名字）
    ret=book.authors.all().values('name')
    print(ret)
    # 以上总结：
    # （1）
    # book=Book.objects.filter(name='红楼梦').first()
    # print(book)
    # 在点publish的时候，其实就是拿着publish_id又去app01_publish这个表里查数据了
    # print(book.publish)
    # （2）book.authors.all()
```



核心:book_obj.authors.all()是什么？

**多对多关系其它常用API：**

```
book_obj.authors.remove()      # 将某个特定的对象从被关联对象集合中去除。    ======   book_obj.authors.remove(*[])
book_obj.authors.clear()       #清空被关联对象集合
book_obj.authors.set()         #先清空再设置　
```



## 三 基于对象的跨表查询

```python
ORM跨表查询
        1.子查询
        2.连表查询
        
    正反向的概念
        外键字段在谁那儿 由谁查谁就是正向
        
        谁手里有外键字段 谁就是正向查
        没有外键字段的就是反向
        书籍对象 查  出版社    外键字段在书籍       正向查询
        出版社 查 书籍         外键字段在书籍       反向查询
        
        
        正向查询按字段
        反向查询按表名小写 ...
```



```python
# 1.基于对象的跨表查询    子查询
    # 1.查询书籍是python入门的出版社名称
    # book_obj = models.Book.objects.filter(title='python入门').first()
    # # 正向查询按字段
    # print(book_obj.publish.name)
    # print(book_obj.publish.addr)
    # 2.查询书籍主键是6的作者姓名
    # book_obj = models.Book.objects.filter(pk=6).first()
    # # print(book_obj.authors)  # app01.Author.None
    # print(book_obj.authors.all())
    # 3.查询作者是jason的手机号
    # author_obj = models.Author.objects.filter(name='jason').first()
    # print(author_obj.author_detail.phone)
    # print(author_obj.author_detail.addr)
    """
    正向查询 按字段 
    当该字段所对应的数据有多个的时候 需要加.all()
    否者点外键字段直接就能够拿到数据对象
    """
    # 4.查询出版社是东方出版社出版过的书籍
    # publish_obj = models.Publish.objects.filter(name='东方出版社').first()
    # # print(publish_obj.book_set)  # app01.Book.None
    # print(publish_obj.book_set.all())
    # 5.查询作者是jason写过的所有的书
    # author_obj = models.Author.objects.filter(name='jason').first()
    # # print(author_obj.book_set)  # app01.Book.None
    # print(author_obj.book_set.all())
    # 6.查询手机号是110的作者
    # author_detail_obj = models.AuthorDetail.objects.filter(phone=110).first()
    # print(author_detail_obj.author)
    # print(author_detail_obj.author.name)
    # print(author_detail_obj.author.age)
    """
    反向查询按表名小写 
        什么时候需要加_set
            当查询的结果可以是多个的情况下 需要加_set.all()
        什么时候不需要加_set
            当查询的结果有且只有一个的情况下 不需要加任何东西 直接表名小写即可
    """
    # 7.查询书籍是python入门的作者的手机号
    # book_obj = models.Book.objects.filter(title='python入门').first()
    # print(book_obj.authors.all())

    # 2.基于双下划綫的跨表查询   连表查询
    """
    MySQL
        left join
        inner join
        right join
        union
    """
    # 1.查询书籍是python入门的出版社名称
    # 正向
    # res = models.Book.objects.filter(title='python入门').values('publish__name')
    # print(res)
    # 反向
    # res = models.Publish.objects.filter(book__title='python入门').values('name')
    # print(res)


    # 2.查询作者是jason的手机号码
    # 正向
    # res1 = models.Author.objects.filter(name='jason').values('author_detail__phone')
    # print(res1)
    # 反向
    # res = models.AuthorDetail.objects.filter(author__name='jason').values('phone','author__age')
    # print(res)



    # 3.查询手机号是120的作者姓名

    # res2 = models.AuthorDetail.objects.filter(phone=120).values('author__name')
    # print(res2)
    # res = models.Author.objects.filter(author_detail__phone=120).values('name','author_detail__addr')
    # print(res)


    # 4.查询出版社是东方出版社出版的书籍名称
    # res = models.Publish.objects.filter(name='东方出版社').values('book__title','addr')
    # print(res)
    # 5.查询作者是jason的写过的书的名字和价格
    # res = models.Author.objects.filter(name='jason').values('book__title','book__price')
    # print(res)

    # 7.查询书籍是python入门的作者的手机号
    # res = models.Book.objects.filter(title='python入门').values('authors__author_detail__phone')
    # print(res)
```



### 一对多查询（publish与book）

正向查询（按字段：publish）

```
# 查询主键为1的书籍的出版社所在的城市
book_obj=Book.objects.filter(pk=1).first()
# book_obj.publish 是主键为1的书籍对象关联的出版社对象
print(book_obj.publish.city)
```

反向查询（按表名：book_set）

```
publish=Publish.objects.get(name="苹果出版社")
#publish.book_set.all() : 与苹果出版社关联的所有书籍对象集合
book_list=publish.book_set.all()    
for book_obj in book_list:
       print(book_obj.title)
```





```
   # 一对多正向查询
    book=Book.objects.filter(name='红楼梦').first()
    print(book.publish)#与这本书关联的出版社对象
    print(book.publish.name)
    # 一对多反向查询
    # 人民出版社出版过的书籍名称
    pub=Publish.objects.filter(name='人民出版社').first()
    ret=pub.book_set.all()
    print(ret)
```



### 一对一查询（Author 与 AuthorDetail）

正向查询(按字段：authorDetail)：

```
egon=Author.objects.filter(name="egon").first()
print(egon.authorDetail.telephone)
```

反向查询(按表名：author)：

```
# 查询所有住址在北京的作者的姓名
 
authorDetail_list=AuthorDetail.objects.filter(addr="beijing")
for obj in authorDetail_list:
     print(obj.author.name)
```





```
    # 一对一正向查询
    # lqz的手机号
    lqz=Author.objects.filter(name='lqz').first()
    tel=lqz.author_detail.telephone
    print(tel)
    # 一对一反向查询
    # 地址在北京的作者姓名
    author_detail=AuthorDatail.objects.filter(addr='北京').first()
    name=author_detail.author.name
    print(name)
```



### 多对多查询 (Author 与 Book)

正向查询(按字段：authors)：

```
# 眉所有作者的名字以及手机号
 
book_obj=Book.objects.filter(title="眉").first()
authors=book_obj.authors.all()
for author_obj in authors:
     print(author_obj.name,author_obj.authorDetail.telephone)
```

反向查询(按表名：book_set)：

```
# 查询egon出过的所有书籍的名字
 
    author_obj=Author.objects.get(name="egon")
    book_list=author_obj.book_set.all()        #与egon作者相关的所有书籍
    for book_obj in book_list:
        print(book_obj.title)
```





```
    # 正向查询----查询红楼梦所有作者名称
    book=Book.objects.filter(name='红楼梦').first()
    ret=book.authors.all()
    print(ret)
    for auth in ret:
        print(auth.name)
    # 反向查询 查询lqz这个作者写的所有书
    author=Author.objects.filter(name='lqz').first()
    ret=author.book_set.all()
    print(ret)
```



 

**注意：**

你可以通过在 ForeignKey() 和ManyToManyField的定义中设置 related_name 的值来覆写 FOO_set 的名称。例如，如果 Article model 中做一下更改：

```
publish = ForeignKey(Book, related_name='bookList')
```

 

那么接下来就会如我们看到这般:

```
# 查询 人民出版社出版过的所有书籍
 
publish=Publish.objects.get(name="人民出版社")
book_list=publish.bookList.all()  # 与人民出版社关联的所有书籍对象集合
```

 



## 基于双下划线的跨表查询

Django 还提供了一种直观而高效的方式在查询(lookups)中表示关联关系，它能自动确认 SQL JOIN 联系。要做跨关系查询，就使用两个下划线来链接模型(model)间关联字段的名称，直到最终链接到你想要的model 为止。

```
'''
    正向查询按字段,反向查询按表名小写用来告诉ORM引擎join哪张表
'''
```

### 一对多查询



```
# 练习:  查询苹果出版社出版过的所有书籍的名字与价格(一对多)

    # 正向查询 按字段:publish
queryResult=Book.objects.filter(publish__name="苹果出版社").values_list("title","price")

    # 反向查询 按表名:book

queryResult=Publish.objects.filter(name="苹果出版社").values_list("book__title","book__price")查询的本质一样，就是select from的表不一样
```







```
    # 正向查询按字段，反向查询按表名小写
    # 查询红楼梦这本书出版社的名字
    # select * from app01_book inner join app01_publish
    # on app01_book.publish_id=app01_publish.nid
    ret=Book.objects.filter(name='红楼梦').values('publish__name')
    print(ret)
    ret=Publish.objects.filter(book__name='红楼梦').values('name')
    print(ret)
```



### 多对多查询　



```
# 练习: 查询alex出过的所有书籍的名字(多对多)

    # 正向查询 按字段:authors:
    queryResult=Book.objects.filter(authors__name="yuan").values_list("title")

    # 反向查询 按表名:book
    queryResult=Author.objects.filter(name="yuan").values_list("book__title","book__price")
```









```
    # 正向查询按字段，反向查询按表名小写
    # 查询红楼梦这本书出版社的名字
    # select * from app01_book inner join app01_publish
    # on app01_book.publish_id=app01_publish.nid
    ret=Book.objects.filter(name='红楼梦').values('publish__name')
    print(ret)
    ret=Publish.objects.filter(book__name='红楼梦').values('name')
    print(ret)
    # sql 语句就是from的表不一样
    # -------多对多正向查询
    # 查询红楼梦所有的作者
    ret=Book.objects.filter(name='红楼梦').values('authors__name')
    print(ret)
    # ---多对多反向查询
    ret=Author.objects.filter(book__name='红楼梦').values('name')
    ret=Author.objects.filter(book__name='红楼梦').values('name','author_detail__addr')
    print(ret)
```



 

### 一对一查询



```
# 查询alex的手机号
    
    # 正向查询
    ret=Author.objects.filter(name="alex").values("authordetail__telephone")

    # 反向查询
    ret=AuthorDetail.objects.filter(author__name="alex").values("telephone")
```







```
    # 查询lqz的手机号
    # 正向查
    ret=Author.objects.filter(name='lqz').values('author_detail__telephone')
    print(ret)
    # 反向查
    ret= AuthorDatail.objects.filter(author__name='lqz').values('telephone')
    print(ret)
```



### 进阶练习(连续跨表)



```
# 练习: 查询人民出版社出版过的所有书籍的名字以及作者的姓名


    # 正向查询
    queryResult=Book.objects.filter(publish__name="人民出版社")values_list("title","authors__name")
    # 反向查询
    queryResult=Publish.objects.filter(name="人民出版社").values_list("book__title","book__authors__age","book__authors__name")


# 练习: 手机号以151开头的作者出版过的所有书籍名称以及出版社名称


    # 方式1:
    queryResult=Book.objects.filter(authors__authorDetail__telephone__regex="151")
　　　　　　　　　　　　.values_list("title","publish__name")
    # 方式2:    
    ret=Author.objects.filter(authordetail__telephone__startswith="151").values("book__title","book__publish__name")
```







```
  # ----进阶练习，连续跨表
    # 查询手机号以33开头的作者出版过的书籍名称以及书籍出版社名称
    # author_datail author book publish
    # 基于authorDatail表
    ret=AuthorDatail.objects.filter(telephone__startswith='33').values('author__book__name','author__book__publish__name')
    print(ret)
    # 基于Author表
    ret=Author.objects.filter(author_detail__telephone__startswith=33).values('book__name','book__publish__name')
    print(ret)
    # 基于Book表
    ret=Book.objects.filter(authors__author_detail__telephone__startswith='33').values('name','publish__name')
    print(ret)
    # 基于Publish表
    ret=Publish.objects.filter(book__authors__author_detail__telephone__startswith='33').values('book__name','name')
    print(ret)
```



### related_name

```
publish = ForeignKey(Blog, related_name='bookList')
```



```
# 练习: 查询人民出版社出版过的所有书籍的名字与价格(一对多)

# 反向查询 不再按表名:book,而是related_name:bookList


    queryResult=Publish.objects
　　　　　　　　　　　　　　.filter(name="人民出版社")
　　　　　　　　　　　　　　.values_list("bookList__title","bookList__price") 
```



 

反向查询时，如果定义了related_name ，则用related_name替换表名，例如：

