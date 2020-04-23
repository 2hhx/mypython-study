# showdoc在线接口文档编辑

# https://www.showdoc.cc/

https://www.showdoc.cc/demo?page_id=7

# DRF自动生成API方法--coreapi



## DRF 自动生成API

[coreapi:https://pypi.org/project/coreapi/](https://pypi.org/project/coreapi/)

[coreapi:使用https://www.bbsmax.com/A/amd0N1ZXJg/](https://www.bbsmax.com/A/amd0N1ZXJg/)

    # pip install coreapi
    
    from django.urls import path
    from rest_framework.documentation import include_docs_urls
    
    urlpatterns = [
        # 如果存在权限的问题，加上 authentication_classes=[], permission_classes=[] 约束
        # 例如: include_docs_urls(title='API', authentication_classes=[], 
        # permission_classes=[])
        path("api-docs/", include_docs_urls("API文档")),
    ]
API自动生成无法显示传递参数

```python
from rest_framework import generics, views
class OwnerView(generics.GenericAPIView):
    """
    继承generics中的视图，API中显示参数
    """
class OwnerView(views.APIView):
    """
    继承views中的视图，API中不显示参数
    """

# 原因-- 可以查看源代码，其中views中是没有序列化的，而generics封装了序列化 
```

补充

* 如果和上面的基本一致, 还是报错403; 请查看 app 的 url 是否有添加
* 自己遇到的: 如果任何 APP 的 url 都不添加, 也是会报403 forbidden的错误

# Django使用swagger生成接口文档

ixuer 2019-11-08 [原文](https://www.bbsmax.com/link/cVZkZTFvR25kUA==)

参考博客：[Django接入Swagger，生成Swagger接口文档-操作解析](https://www.jianshu.com/p/c53de96f3ff1)

`Swagger`是一个规范和完整的框架，用于生成、描述、调用和可视化`RESTful`风格的`Web`服务。总体目标是使客户端和文件系统源代码作为服务器以同样的速度来更新。当接口有变动时，对应的接口文档也会自动更新。

Swagger优势：

1. Swagger可生成一个具有互动性的API控制台，开发者可快速学习和尝试API；
2. Swagger可生成客户端SDK代码，用于不同平台上（Java、Python...）的实现；
3. Swagger文件可在许多不同的平台上从代码注释中自动生成；
4. Swagger有一个强大的社区，里面有许多强悍的贡献者。

------

django使用swagger主要步骤：

1. 安装swagger；
2. 添加到swagger到配置文件；
3. 在主路由中配置路由；
4. 启动服务，测试效果；

------

一、 安装swagger

```
pip install django-rest-swagger
```

二、 将swagger添加到settings.py配置文件的INSTALLEDAPP中，如：

```
INSTALLED_APPS = [   
'django.contrib.admin', 
'django.contrib.auth',   
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'rest_framework', 
'rest_framework_swagger',  # swagger自动生成接口文档  
]
```

三、 在主路由中配置接口文档的路由：
在项目同名目录下的urls.py中，填写如下代码：

```
from django.urls import path
# 导入restframework的辅助函数get_schema_viewfrom rest_framework.schemas 
import get_schema_view# 导入swagger的两个Render类
from rest_framework_swagger.renderers import SwaggerUIRenderer,OpenAPIRenderer
# 利用get_schema_view()方法，传入两个Render类得到一个schema viewschema_view = get_schema_view(title='API',renderer_classes=[SwaggerUIRenderer,OpenAPIRenderer])
# 配置接口文档的访问路径
urlpatterns = [    
# 访问localhost:8000/docs/即可    
path('docs/', schema_view, name="swagger接口文档")]
```

四、 在接口类视图里面写上注释，可以被当成接口文档说明显示。启动服务，访问`localhost:8000/docs/`即可。