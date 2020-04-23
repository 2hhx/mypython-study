

# drf安装与使用

[restframework官网](https://www.django-rest-framework.org/)

## 安装drf：

`pip3 install djangorestframework`



## drf的配置分析

```
# 1）安装drf：pip3 install djangorestframework
# 2）settings.py注册app：INSTALLED_APPS = [..., 'rest_framework']
# 3）基于cbv完成满足RSSTful规范的接口
# 视图层
from rest_framework.views import APIView
from rest_framework.response import Response
user_list = [{'id': 1, 'name': 'Bob'}, {'id': 2, 'name': 'Tom'}]
class Users(APIView):
    def get(self, request, *args, **kwargs):
        return Response({
            'status': 0,
            'msg': 'ok',
            'results': user_list
        })
    def post(self, request, *args, **kwargs):
        # request对formdata，urlencoded，json三个格式参数均能解析
        name = request.data.get('name')
        id = len(user_list) + 1
        user = {'id': id, 'name': name}
        user_list.append(user)
        return Response({
            'status': '0',
            'msg': 'ok',
            'results': user
        })
# 路由层
from app import views
urlpatterns = [
    url(r'^users/', views.Users.as_view()),
]
```

## drf的封装风格

1. 设计风格，某一个功能就在工具包下面,所有的模块都在功能名字内

```
from rest_framework.views import APIView  # 视图
from rest_framework.request import Request  # 请求
from rest_framework.response import Response  # 响应
from rest_framework.filters import SearchFilter  # 过滤器
from rest_framework.pagination import PageNumberPagination  # 分页
from rest_framework.exceptions import APIException  # 异常
from rest_framework.authentication import BaseAuthentication  # 认证
from rest_framework.permissions import IsAuthenticated  # 校验认证
from rest_framework.throttling import SimpleRateThrottle  # 频率认证
from rest_framework.settings import APISettings  # 设置
from rest_framework import status  # 状态码  点进去之后名字是对状态码的描述
```



# request源码分析

```
# as_view()
    # 核心走了父类as_view
    view = super(APIView, cls).as_view(**initkwargs)
    # 返回的是局部禁用csrf认证的view视图函数
    return csrf_exempt(view)
    
# dispatch(self, request, *args, **kwargs)
    # 二次封装request对象
    request = self.initialize_request(request, *args, **kwargs)
    # 自定义request规则
    self.initial(request, *args, **kwargs)
    
# initialize_request(self, request, *args, **kwargs)
    # 原生request封装在request._request
    
# initial(self, request, *args, **kwargs)
    # 认证
    self.perform_authentication(request)
    # 权限
    self.check_permissions(request)
    # 频率
    self.check_throttles(request)
```

1. 