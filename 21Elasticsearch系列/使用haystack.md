## Haystack的介绍和使用

## 一，什么是Haystack

　　搜索是一个日益重要的话题。用户越来越依赖于搜索从噪声信息中分离和快速找到有用信息。此外，搜索搜索可以洞察那些东西是受欢迎的，改善网站上难以查找的东西。

　　为此，Haystack试图整合自定义搜索，使其尽可能简单的灵活和强大到足以处理更高级的用例。haystack支持多种搜索引擎，不仅仅是whoosh，使用

solr、elastic search等搜索，也可通过haystack，而且直接切换引擎即可，甚至无需修改搜索代码。

## 二，安装相关的包

```
pip install django-haystack
pip install whoosh
pip install jieba
```

## 三，配置

　　1：将Haystack添加到settings.py中的INSTALLED_APPS中：



```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    # 添加
    'haystack',
    # 你的app
    'blog',
]
```



　　2：在你的settings.py中添加一个设置来指示站点配置文件正在使用的后端，以及其他的后端设置。

　　　　`HAYSTACK——CONNECTIONS`是必需的设置，并且应该至少是以下的一种：

　　　　**Solr：**



```
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.solr_backend.SolrEngine',
        'URL': 'http://127.0.0.1:8983/solr'
        # ...or for multicore...
        # 'URL': 'http://127.0.0.1:8983/solr/mysite',
    },
}
```



　　　　**Elasticsearch:**



```
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.elasticsearch_backend.ElasticsearchSearchEngine',
        'URL': 'http://127.0.0.1:9200/',
        'INDEX_NAME': 'haystack',
    },
}
```



　　　　**Whoosh：**



```
#需要设置PATH到你的Whoosh索引的文件系统位置
import os
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.whoosh_backend.WhooshEngine',
        'PATH': os.path.join(os.path.dirname(__file__), 'whoosh_index'),
    },
}

# 自动更新索引
HAYSTACK_SIGNAL_PROCESSOR = 'haystack.signals.RealtimeSignalProcessor'
```



　　　　**Xapian:**



```
#首先安装Xapian后端（http://github.com/notanumber/xapian-haystack/tree/master）
#需要设置PATH到你的Xapian索引的文件系统位置。
import os
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'xapian_backend.XapianEngine',
        'PATH': os.path.join(os.path.dirname(__file__), 'xapian_index'),
    },
}
```



 　　下面我们以whoosh为例进行操作。

## 四：配置路由

　　在整个项目的urls.py中，配置搜索功能的url路径

```
urlpatterns = [
    ...
    url(r'^search/', include('haystack.urls')),
]
```

## 五，创建索引

　　在你的应用目录下面新建一个search_indexes.py文件，文件名不能修改！



```
from haystack import indexes
from app01.models import Article

class ArticleIndex(indexes.SearchIndex, indexes.Indexable):
    #类名必须为需要检索的Model_name+Index，这里需要检索Article，所以创建ArticleIndex
    text = indexes.CharField(document=True, use_template=True)#创建一个text字段 
    #其它字段
    desc = indexes.CharField(model_attr='desc')
    content = indexes.CharField(model_attr='content')

    def get_model(self):#重载get_model方法，必须要有！
        return Article

    def index_queryset(self, using=None):
        return self.get_model().objects.all()
```



　　ps：为什么要创建索引呢，索引就像一本书的目录，可以为读者提供更快速的导航与查找。在这里也是同样的道理，当数据量非常大的时候，若要从

　　　　这些数据里找出所有满足搜索条件的几乎是不太可能的事情，将会给服务器带来极大的负担，所以我们需要为指定的数据添加一个索引。

　　　　索引实现的细节并不是我们需要关心的事情，但是它为哪些字段创建索引，怎么指定，下面来说明：

　　　　每个索引里面必须有且只能有一个字段为 document=Ture，这代表着haystack和搜索引擎将使用此字段的内容作为索引进行检索（primary field）

　　　　其他的字段只是附属的属性，方便调用，并不做检索的依据。

　　　　注意：如果一个字段设置了document=True,则一般约定此字段名为text，这是ArticleIndex类里面一贯的写法。

　　　　另外，我们在text字段上提供了use_template=Ture。这允许我们使用一个数据模板，来构建文档搜索引擎索引。你应该在模板目录下建立,也就是在

　　　　templates文件夹中建立一个新的模板，search/indexes/项目名/模型名_text.txt，并且将以下的内容放入txt文件中：

```
#在目录“templates/search/indexes/应用名称/”下创建“模型类名称_text.txt”文件
{{ object.title }}
{{ object.desc }}
{{ object.content }}
```

　　　　这个数据模板的作用就是对`Note.title`, `Note.user.get_full_name`,`Note.body`这三个字段建立索引，当检索的时候会对这三个字段做全文检索匹配。

## 六：编辑搜索模板

　　　　搜索模板默认在search/search.html中，下面的代码足以让你搜索运行：



```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style>
        span.highlighted {
            color: red;
        }
    </style>
</head>
<body>
{% load highlight %}
{% if query %}
    <h3>搜索结果如下：</h3>
    {% for result in page.object_list %}
{#        <a href="/{{ result.object.id }}/">{{ result.object.title }}</a><br/>#}
        <a href="/{{ result.object.id }}/">{%   highlight result.object.title with query max_length 2%}</a><br/>
        <p>{{ result.object.content|safe }}</p>
        <p>{% highlight result.content with query %}</p>
    {% empty %}
        <p>啥也没找到</p>
    {% endfor %}

    {% if page.has_previous or page.has_next %}
        <div>
            {% if page.has_previous %}
                <a href="?q={{ query }}&amp;page={{ page.previous_page_number }}">{% endif %}&laquo; 上一页
            {% if page.has_previous %}</a>{% endif %}
            |
            {% if page.has_next %}<a href="?q={{ query }}&amp;page={{ page.next_page_number }}">{% endif %}下一页 &raquo;
            {% if page.has_next %}</a>{% endif %}
        </div>
    {% endif %}
{% endif %}
</body>
</html>
```



 

　　注意：page.object_list实际上是SearchResult对象的列表。这些对象返回索引的所有数据。他们可以通过{{ result.object }}来访问，

　　　　　　所以`{{ result.object.title}}`实际使用的是数据库中Article对象来访问`title`字段的。

 

 

## 七，重建索引

　　配置完成之后，接下应该把数据库中的数据放入索引。Haystack中自带了一个命令工具：

```
python manage.py rebuild_index
```

## 八，使用jieba分词

　　新建一个ChineseAnalyzer.py文件:



```
import jieba
from whoosh.analysis import Tokenizer, Token

class ChineseTokenizer(Tokenizer):
    def __call__(self, value, positions=False, chars=False,
                 keeporiginal=False, removestops=True,
                 start_pos=0, start_char=0, mode='', **kwargs):
        t = Token(positions, chars, removestops=removestops, mode=mode,
                  **kwargs)
        seglist = jieba.cut(value, cut_all=True)
        for w in seglist:
            t.original = t.text = w
            t.boost = 1.0
            if positions:
                t.pos = start_pos + value.find(w)
            if chars:
                t.startchar = start_char + value.find(w)
                t.endchar = start_char + value.find(w) + len(w)
            yield t


def ChineseAnalyzer():
    return ChineseTokenizer()
```



　　保存在python安装路径的backends文件夹中（例如：D:\python3\Lib\site-packages\haystack\backends）

　　然后在该文件夹中找到一个whoosh_backend.py文件，改名为whoosh_cn_backend.py

　　在内部添加：

```
from .ChineseAnalyzer import ChineseAnalyzer 
```

　　然后查找到这行代码：

```
analyzer=StemmingAnalyzer()
```

　　修改为：

```
analyzer=ChineseAnalyzer()
```

## 九，在模板找中创建搜索栏：

```
<form method='get' action="/search/" target="_blank">
    <input type="text" name="q">
    <input type="submit" value="查询">
</form>
```

# docker中使用

 django 对接elasticsearch实现全文检索

**本文demo代码请加群获取**

## 第一步：安装elasticsearch环境（docker安装）

拉取镜像

​    

```
docker image pull delron/elasticsearch-ik:2.4.6-1.0
```

运行容器

​    

```
docker run -d -p 9200:9200 -p 9300:9300 --name search delron/elasticsearch-ik:2.4.6-1.0
```

## 第二步：首先安装相关的依赖包

​    

```
pip3 install drf-haystack 
pip3 install elasticsearch
pip3 install djangorestframework
```

## 第三步：在django项目配置文件settings.py中注册应用

​    

```
INSTALLED_APPS = [
    ...
    'app01.apps.App01Config',        
    'haystack',
    'rest_framework'
]
```

## 第四步：在django项目配置文件settings.py中指定搜索的后端

​    

```
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.elasticsearch_backend.ElasticsearchSearchEngine',
        'URL': 'http://172.16.209.100:9200/',  # 此处为elasticsearch运行的服务器ip地址，端口号固定为9200
        'INDEX_NAME': 'test',  # 指定elasticsearch建立的索引库的名称
    },
}

# 当添加、修改、删除数据时，自动生成索引
HAYSTACK_SIGNAL_PROCESSOR = 'haystack.signals.RealtimeSignalProcessor'
 # 指定搜索结果每页的条数
 # HAYSTACK_SEARCH_RESULTS_PER_PAGE = 1
```

## 第五步：创建索引类

在此之前要先创建model类，并插入数据

​    

```
from django.db import models
class Book(models.Model):
    nid=models.AutoField(primary_key=True)
    name=models.CharField(max_length=32)
    publish=models.CharField(max_length=32)
    price=models.DecimalField(max_digits=5,decimal_places=2)
#插入多条数据
```

在需要进行索引的应用的目录下创建文件search_indexes.py, 在该文件内创建该索引类

我在app01应用下创建：search_indexes.py

​    

```
# 索引模型类的名称必须是 模型类名称 + Index
from haystack import indexes
from .models import Book
class BookIndex(indexes.SearchIndex, indexes.Indexable):
    text = indexes.CharField(document=True, use_template=True)
    def get_model(self):
        """返回建立索引的模型类"""
        return Book
    def index_queryset(self, using=None):
        """返回要建立索引的数据查询集"""
        return self.get_model().objects.all()
"""
说明: 
1.在BookIndex建立的字段，都可以借助haystack由elasticsearch搜索引擎查询。
2.其中text字段声明为document=True，表名该字段是主要进行关键字查询的字段， 该字段的索引值可以由多个数据库模型类字段组成(是多个字段,不是多个数据库模型类,转者注)，具体由哪些模型类字段组成，我们用use_template=True表示后续通过模板来指明。
3.在 REST framework中，索引类的字段会作为查询结果返回数据的来源, 
"""
```

## 第六步：在templates目录中创建text字段使用的模板文件

创建文件templates/search/indexes/app01/book_text.txt文件中定义

​    

```
{{ object.name }}
{{ object.publish }}
```

## 第七步：手动更新索引

​    

```
python manage.py rebuild_index   #数据库有多少条数据，全部会被同步到es中
```

## 第八步：创建haystack序列化器

​    

```
from drf_haystack.serializers import HaystackSerializer
from rest_framework.serializers import ModelSerializer

from app01 import models

from app01.search_indexes import BookIndex
class BookSerializer(ModelSerializer):
    class Meta:
        model=models.Book
        fields='__all__'
class BookIndexSerializer(HaystackSerializer):
    object = BookSerializer(read_only=True) # 只读,不可以进行反序列化

    class Meta:
        index_classes = [BookIndex]# 索引类的名称
        fields = ('text', 'object')# text 由索引类进行返回, object 由序列化类进行返回,第一个参数必须是text
```

## 第九步：创建视图类

​    

```
from drf_haystack.viewsets import HaystackViewSet
from app01.models import Book
from app01.serializers import BookIndexSerializer
class BookSearchView(HaystackViewSet):
    index_models = [Book]

    serializer_class = BookIndexSerializer
#该视图会返回搜索结果的列表数据，所以如果可以为视图增加REST framework的分页功能。
#我们在配置文件已经定义了分页配置，所以此搜索视图会进行分页
```

## 第十步：添加路由

​    

```
from django.conf.urls import url
from django.contrib import admin
from rest_framework import routers

from app01.views import BookSearchView
router = routers.DefaultRouter()
router.register("book/search", BookSearchView, base_name="book-search")
urlpatterns = [
    url(r'^admin/', admin.site.urls),
]

urlpatterns += router.urls
```

## 第十一步：测试

​    

```
http://127.0.0.1:8000/?text=北  #查询出名字中和出版社中有北的数据
```

![image-20191105223450063](https://tva1.sinaimg.cn/large/006y8mN6gy1g8njevn11gj31oj0u0wju.jpg)





 