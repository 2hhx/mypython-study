

[TOC]



# Restful 接口规范

与django相比的话，不会出现csrf问题

接口规范：

```
https/api/v1/books/get、post/？limit=3/http_status/{status,msg,results}/data:https://delete
成功不做数据返回
```





# 简介

2000年Roy Fielding博士在其博士论文中提出REST（Representational State Transfer）风格的软件架构模式后，REST就基本上迅速取代了复杂而笨重的SOAP，成为Web API的标准了。

RESTful作为目前最流行的 API 设计规范，一定有着它独有的魅力：强大、简易、易上手。

# URL设计

## django内数据分析

views.py

```
from . import models

class Book(View):
    def get(self, request, *args, **kwargs):
        pk = kwargs.get('pk', None)
        if pk:  # 单查
            book_dic = models.Book.objects.filter(pk=pk).values('name', 'price').first()
            results = book_dic
        else:  # 群查
            book_query = models.Book.objects.values('name', 'price')
            results = list(book_query)

        if not results:
            return JsonResponse({
                'status': 1,
                'msg': 'data error',
            })
        return JsonResponse({
            'status': 0,
            'msg': 'ok',
            'results': results
        })

    # 单增：{}
    # 群增：[{},{},{}]
    def post(self, request, *args, **kwargs):
        return JsonResponse({'res': 'post ok'})

    # 单改：pk，{}
    # 群改：[{pk:1,...},{},{}]
    def put(self, request, *args, **kwargs):

        return JsonResponse({'res': 'put ok'})

    # 单改：pk，{}
    # 群改：[{pk:1,...},{},{}]
    def patch(self, request, *args, **kwargs):
        return JsonResponse({'res': 'patch ok'})

    # 单删：pk
    # 群删：pks
    def delete(self, request, *args, **kwargs):
        return JsonResponse({'res': 'delete ok'})
```



urls.py

```
from django.conf.urls import url, include
from django.contrib import admin

from api import views
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^test/', views.Test.as_view()),

    # 路由分发
    url(r'^api/', include('api.urls')),
]
```

app内的urls.py

```
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^books/$', views.Book.as_view()),
    url(r'^books/(?P<pk>\d+)/$', views.Book.as_view()),
]

```



##  数据的安全保障

- url链接一般都采用https协议进行传输

  注：采用https协议，可以提高数据交互过程中的安全性

##  接口特征表现

- 用api关键字标识接口url：

  - [https://api.baidu.com](https://api.baidu.com/)
  - https://www.baidu.com/api

  注：看到api字眼，就代表该请求url链接是完成前后台数据交互的

## 多数据版本共存

- 在url链接中标识数据版本

  - https://api.baidu.com/v1
  - https://api.baidu.com/v2

  注：url链接中的v1、v2就是不同数据版本的体现（只有在一种数据资源有多版本情况下）

## 数据即是资源

- 接口一般都是完成前后台数据的交互，交互的数据我们称之为资源

  - https://api.baidu.com/users
  - https://api.baidu.com/books
  - https://api.baidu.com/book

  注：一般提倡用资源的复数形式，在url链接中奖励不要出现操作资源的动词，错误示范：https://api.baidu.com/delete-user

- 特殊的接口可以出现动词，因为这些接口一般没有一个明确的资源，或是动词就是接口的核心含义

  - https://api.baidu.com/place/search
  - https://api.baidu.com/login

## 资源操作由请求方式决定

- 操作资源一般都会涉及到增删改查，我们提供请求方式来标识增删改查动作
  - https://api.baidu.com/books - get请求：获取所有书
  - https://api.baidu.com/books/1 - get请求：获取主键为1的书
  - https://api.baidu.com/books - post请求：新增一本书书
  - https://api.baidu.com/books/1 - put请求：整体修改主键为1的书
  - https://api.baidu.com/books/1 - patch请求：局部修改主键为1的书
  - https://api.baidu.com/books/1 - delete请求：删除主键为1的书

# 响应状态码

## 正常响应

- 响应状态码2xx
  - 200：常规请求
  - 201：创建成功

## 重定向响应

- 响应状态码3xx
  - 301：永久重定向
  - 302：暂时重定向

## 客户端异常

- 响应状态码4xx
  - 403：请求无权限
  - 404：请求路径不存在
  - 405：请求方法不存在

## 服务器异常

- 响应状态码5xx
  - 500：服务器异常

# 响应结果

## 响应数据要有状态码、状态信息以及数据本身

```
{
    "status": 0,
    "msg": "ok",
    "results":[
        {
            "name":"肯德基(罗餐厅)",
            "location":{
                "lat":31.415354,
                "lng":121.357339
            },
            "address":"月罗路2380号",
            "province":"上海市",
            "city":"上海市",
            "area":"宝山区",
            "street_id":"339ed41ae1d6dc320a5cb37c",
            "telephone":"(021)56761006",
            "detail":1,
            "uid":"339ed41ae1d6dc320a5cb37c"
        }
        ...
        ]
}
```

## 需要url请求的资源需要访问资源的请求链接

```
{
    "status": 0,
    "msg": "ok",
    "results":[
        {
            "name":"肯德基(罗餐厅)",
            "img": "https://image.baidu.com/kfc/001.png"
        }
        ...
        ]
}
```



![](https://img2018.cnblogs.com/blog/1739658/201911/1739658-20191118213422964-499834075.png)