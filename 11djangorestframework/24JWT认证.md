

# JWT认证

# drf-jwt

[drf-jwt：官网http://getblimp.github.io/django-rest-framework-jwt/](http://getblimp.github.io/django-rest-framework-jwt/)

安装子：虚拟环境

```
pip install djangorestframework-jwt
```

## jwt模块

```python
模块包：rest_framework_jwt
才有drf-jwt框架，后期任务只需要书写登录
	为什么要重写登录：drf-jwt只完成了账号密码登录，我们还需要手机登录，邮箱登录
	为什么不需要重写认证类：因为认证规则已经完成且固定不变，变得只有认证字符串的前缀，前缀可以在配置文件中配置
```



# 工作原理

```
"""
1) jwt = base64(头部).base(载荷).hash256(base64(头部).base(载荷).密钥)
2) base64是可逆的算法、hash256是不可逆的算法
3) 密钥是固定的字符串，保存在服务器
"""
```

# jwt认证规则

```python
"""
全称：json web token
解释：加密字符串的原始数据是json，后台产生，通过web传输给前台存储
格式：三段式 - 头.载荷.签名 - 头和载荷才有的是base64可逆加密，签名才有md5不可逆加密
内容：
	头(基础信息,也可以为空)：加密方式、公司信息、项目组信息、...
	载荷(核心信息)：用户信息、过期时间、...
	签名(安全保障)：头加密结果+载荷加密结果+服务器秘钥 的md5加密结果
	服务器签发（login）->web传送给前端存储 ->请求需要的登陆的结果在携带给后台 ->服务器校验认证组件=>权限管理
	
	
认证规则：
	后台一定要保障 服务器秘钥 的安全性(它是jwt的唯一安全保障)
	后台签发token -> 前台存储 -> 发送需要认证的请求带着token -> 后台校验得到合法的用户
"""
```

JWT含有三个部分：

* **头部（header）**
* **载荷（payload）**
* **签证（signature）**

**头部（header）**
 头部一般有两部分信息：`类型`、`加密的算法`（通常使用HMAC SHA256）
 头部一般使用base64加密：`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9`
 解密后：

```json
{
    "typ":"JWT",
    "alg":"HS256"
}
```

**载荷（payload）**
 该部分一般存放一些有效的信息。JWT的标准定义包含五个字段：

* **iss**：该JWT的签发者
* **sub**：该JWT所面向的用户
* **aud**：接收该JWT的一方
* **exp（expires）**：什么时候过期，这里是一个Unit的时间戳
* **iat（issued at**）：在什么时候签发的

**签证（signature）**
 JWT最后一个部分。该部分是使用了HS256加密后的数据；包含三个部分：

* **heade**r(base64后的）
* **payload**（base64后的）
* **secret** 私钥

`secret`是保存在`服务器端`的，JWT的签发生成也是在服务器端的，`secret`就是用来进行JWT的`签发`和JWT的`验证`，所以，它就是你服务端的`秘钥`，在任何场景都不应该流露出去。一旦客户端得知这个secret，那就意味着客户端可以自我签发JWT了。

### JWT特点

* **紧凑**：意味着这个字符串很小，甚至可以放在URL参数，POST Parameter中以Http Header的方式传输。
* **自包含**：传输的字符串包含很多信息，别人拿到以后就不需要多次访问数据库获取信息，而且通过其中的信息就可以知道加密类型和方式（当然解密需要公钥和密钥）。

### 如何使用JWT？

在身份鉴定的实现中，传统的方法是在服务端存储一个 `session`，给客户端返回一个 `cookie`，而使用JWT之后，当用户使用它的认证信息登录系统之后，会返回给用户一个`JWT`， 用户只需要本地保存该 `token`（通常使用localStorage，也可以使用cookie）即可。

当用户希望访问一个受保护的路由或者资源的时候，通常应该在 `Authorization` 头部使用 `Bearer` 模式添加JWT，其内容格式：

```xml
Authorization: Bearer <token>
```

因为用户的状态在`服务端内容中是不存储`的，所以这是一种`无状态`的认证机制。服务端的保护路由将会检查请求头 `Authorization` 中的JWT信息，如果合法，则允许用户的行为。由于JWT是 `自包含`的，因此，减少了需要查询数据库的需要。

JWT的这些特征使得我们可以完全依赖无状态的特性提供数据API服务。因为JWT并不使用Cookie的，所以你可以在任何域名提供你的API服务而不需要担心跨域资源共享问题（CORS）

下面的序列图展示了该过程：



![img](https:////upload-images.jianshu.io/upload_images/11464886-9fd1cd00741d5d8d.png?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

image

中文流程介绍：

1. 用户使用账号和密码发出POST登录请求；
2. 服务器使用私钥创建一个JWT；
3. 服务器返回这个JWT给浏览器；
4. 浏览器将该JWT串放在请求头中向服务器发送请求；
5. 服务器验证该JWT；
6. 返回响应的资源给浏览器。

说了这么多JWT到底如何应用到我们的项目中，下面我们就使用SpringBoot 结合 JWT完成用户的登录验证。

### 应用流程

* 初次登录生成JWT流程图

  

  ![img](https:////upload-images.jianshu.io/upload_images/11464886-e074e8f5891b6f3f.png?imageMogr2/auto-orient/strip|imageView2/2/w/729/format/webp)

  image

* 用户访问资源流程图

  

  ![img](https:////upload-images.jianshu.io/upload_images/11464886-83d07cc27a3f7237.png?imageMogr2/auto-orient/strip|imageView2/2/w/745/format/webp)

# 限制访问频率

**定义限制访问频率的中间件**

common/middleware.py

```
`import` `time` `from` `django.utils.deprecation ``import` `MiddlewareMixin` `MAX_REQUEST_PER_SECOND``=``2` `#每秒访问次数` `class` `RequestBlockingMiddleware(MiddlewareMixin):` `  ``def` `process_request(``self``,request):``    ``now``=``time.time()``    ``request_queue ``=` `request.session.get(``'request_queue'``,[])``    ``if` `len``(request_queue) < MAX_REQUEST_PER_SECOND:``      ``request_queue.append(now)``      ``request.session[``'request_queue'``]``=``request_queue``    ``else``:``      ``time0``=``request_queue[``0``]``      ``if` `(now``-``time0)<``1``:``        ``time.sleep(``5``)` `      ``request_queue.append(time.time())``      ``request.session[``'request_queue'``]``=``request_queue[``1``:]`
```

**二、将中间件加入配置文件**

setting.py

```
`MIDDLEWARE ``=` `[``  ``'django.middleware.security.SecurityMiddleware'``,``  ``'django.contrib.sessions.middleware.SessionMiddleware'``,``  ``'django.middleware.common.CommonMiddleware'``,``  ``'django.middleware.csrf.CsrfViewMiddleware'``,``  ``'common.middleware.RequestBlockingMiddleware'``, ``#在sessions之后，auth之前``  ``'django.contrib.auth.middleware.AuthenticationMiddleware'``,``  ``'django.contrib.messages.middleware.MessageMiddleware'``,``  ``'django.middleware.clickjacking.XFrameOptionsMiddleware'``,``]`
```

对使用 rest_framework 框架的项目来说，可以使用框架的设置来对api的访问频率进行限制

```
`REST_FRAMEWORK ``=` `{``'DEFAULT_PARSER_CLASSES'``: (``'rest_framework.parsers.JSONParser'``,``'rest_framework.parsers.FormParser'``,``'rest_framework.parsers.MultiPartParser'``,``),` `'DEFAULT_AUTHENTICATION_CLASSES'``: (``# 'lecare.core.rest_auth.CrossSiteSessionAuthentication',``),` `'DEFAULT_PERMISSION_CLASSES'``: [``# 'rest_framework.permissions.IsAuthenticated',``'rest_framework.permissions.AllowAny'``,``],` `'PAGE_SIZE'``: ``20``,``'UNICODE_JSON'``: ``False``,``# 'COERCE_DECIMAL_TO_STRING': False,``# 'EXCEPTION_HANDLER': 'lecare.core.custom_exception_handler.custom_exception_handler',``'JWT_EXPIRATION_DELTA'``: datetime.timedelta(hours ``=` `2``),``'JWT_REFRESH_EXPIRATION_DELTA'``: datetime.timedelta(days ``=` `360``),``'JWT_ALLOW_REFRESH'``: ``False``,``'JWT_AUTH_HEADER_PREFIX'``: ``'JWT'``,``'JWT_PAYLOAD_HANDLER'``: ``'consumer.jwt_conf.jwt_payload_handler'``,``'JWT_RESPONSE_PAYLOAD_HANDLER'``: ``'consumer.jwt_conf.jwt_response_payload_handler'``,``'JWT_GET_USER_SECRET_KEY'``: ``'consumer.jwt_conf.jwt_get_secret_key'``,``# 'DEFAULT_THROTTLE_CLASSES': (``# # 开启匿名用户接口请求频率限制``# 'rest_framework.throttling.AnonRateThrottle',``# # 开启授权用户接口请求频率限制``# 'rest_framework.throttling.UserRateThrottle'``# ),``# 'DEFAULT_THROTTLE_RATES': {``# # 频率限制有second, minute, hour, day``# # 匿名用户请求频率``# 'anon': '30/second',``# # 授权用户请求频率``# 'user': '30/second'``# }``}`
```





# 为什么要才有jwt认证(优点)

1) 后台不需要存储token，只需要存储签发与校验token的算法，效率远远大于后台存储和取出token完成校验
2) jwt算法认证，更适合服务器集群部署

服务器压力小，集群部署更加完善。



# drf-jwt开发

## 使用：user/urls.py

```python
from django.conf.urls import url

from . import views

from rest_framework_jwt.views import refresh_jwt_token
from rest_framework_jwt.views import obtain_jwt_token
from rest_framework_jwt.views import verify_jwt_token
from rest_framework_jwt.views import ObtainJSONWebToken
url(r'^obtain/$', obtain_jwt_token),  # 获取token
    url(r'^verify/$', verify_jwt_token),  # 验证token是否正确,正确原样返回
    url(r'^refresh/$', refresh_jwt_token),  # 提供一个token,刷新一个token，时间往后推迟
    # url(r'^login/$', ObtainJSONWebToken.as_view())  # 实时刷新token

    url(r'^login/$', views.LoginAPIView.as_view()),
```

## 配置信息：JWT_AUTH到settings.py中

```python
# drf-jwt配置

import datetime

JWT_AUTH = {
    'JWT_EXPIRATION_DELTA': datetime.timedelta(days=7),  # 过期时间

    # 'JWT_ALLOW_REFRESH': True,  # 对refresh_jwt_token 允许刷新
    # 'JWT_REFRESH_EXPIRATION_DELTA': datetime.timedelta(days=7),  # 刷新的过期时间

    'JWT_AUTH_HEADER_PREFIX': 'JWT',  # 前缀
}
```

## 序列化user：user/serializers.py(自己创建)

```python
# 只参与序列化
class UserModelSerializer(ModelSerializer):
    # 改了原数据库字段的序列化方式
    password = SerializerMethodField()
    def get_password(self, obj):
        return '########'

    class Meta:
        model = models.User
        fields = ('username', 'password', 'mobile', 'email', 'first_name', 'last_name')


```

## 自定义response：user/utils.py

```python
from rest_framework.response import Response


class APIResponse(Response):
    # 格式化data
    def __init__(self, status=0, msg='ok', results=None, http_status=None, headers=None, exception=False, **kwargs):
        data = {  # json的response基础有数据状态码和数据状态信息
            'status': status,
            'msg': msg
        }
        if results is not None:  # 后台有数据，响应数据
            data['results'] = results
        data.update(**kwargs)  # 后台的一切自定义响应数据直接放到响应数据data中
        super().__init__(data=data, status=http_status,
                         headers=headers, exception=exception)

```

## 基于drf-jwt的全局认证：user/authentications.py(自己创建)

```python
# 自定义认证类

"""
认证模块工作原理
1）继承BaseAuthentication类，重写authenticate方法
2）认证规则(authenticate方法实现体)：
    没有携带认证信息，直接返回None => 游客
    有认证信息，校验失败，抛异常 => 非法用户
    有认证信息，校验出User对象 => 合法用户
"""

from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed
class TokenAuthentication(BaseAuthentication):
    prefix = 'Token'
    def authenticate(self, request):
        # 拿到前台的token
        auth = request.META.get('HTTP_AUTHORIZATION')
        # 没有返回None，有进行校验
        if not auth:
            return None
        auth_list = auth.split()

        if not (len(auth_list) == 2 and auth_list[0].lower() == self.prefix.lower()):
            raise AuthenticationFailed('非法用户')

        token = auth_list[1]

        # 校验算法
        user = _get_obj(token)
        # 校验失败抛异常，成功返回（user, token）
        return (user, token)

# 校验算法（认证类）与签发算法配套
"""
拆封token：一段 二段 三段
用户名：b64decode(一段)
用户主键：b64decode(二段)
碰撞解密：md5(用户名+用户主键+服务器秘钥) == 三段
"""
import base64, json, hashlib
from django.conf import settings
from api.models import User
def _get_obj(token):
    token_list = token.split('.')
    if len(token_list) != 3:
        raise AuthenticationFailed('token异常')
    username = json.loads(base64.b64decode(token_list[0])).get('username')
    pk = json.loads(base64.b64decode(token_list[1])).get('pk')

    md5_dic = {
        'username': username,
        'pk': pk,
        'key': settings.SECRET_KEY
    }

    if token_list[2] != hashlib.md5(json.dumps(md5_dic).encode()).hexdigest():
        raise AuthenticationFailed('token内容异常')

    user_obj = User.objects.get(pk=pk, username=username)
    return user_obj


""" 认证类的认证核心规则
def authenticate(self, request):
    token = get_token(request)
    try:
        user = get_user(token)  # 校验算法
    except:
        raise AuthenticationFailed()
    return (user, token)
"""
```

## 全局启用：settings.py

```python
# drf-jwt配置

import datetime

JWT_AUTH = {
    'JWT_EXPIRATION_DELTA': datetime.timedelta(days=7),  # 过期时间

    # 'JWT_ALLOW_REFRESH': True,  # 对refresh_jwt_token 允许刷新
    # 'JWT_REFRESH_EXPIRATION_DELTA': datetime.timedelta(days=7),  # 刷新的过期时间

    'JWT_AUTH_HEADER_PREFIX': 'JWT',  # 前缀
}
```

## 局部启用禁用：任何一个cbv类首行

```
# 局部禁用
authentication_classes = []

# 局部启用
from user.authentications import JSONWebTokenAuthentication
authentication_classes = [JSONWebTokenAuthentication]
```

# 多方式登录：user/utils.py

```python
from rest_framework.serializers import ModelSerializer, CharField, ValidationError, SerializerMethodField
from . import models
from django.contrib.auth import authenticate
import re
from rest_framework_jwt.serializers import jwt_payload_handler, jwt_encode_handler
class LoginSerializer(ModelSerializer):
    username = CharField(write_only=True)
    password = CharField(write_only=True)
    class Meta:
        model = models.User
        fields = ('username', 'password')

    # 在全局钩子中签发token
    def validate(self, attrs):
        # user = authenticate(**attrs)
        # 账号密码登录 => 多方式登录
        user = self._many_method_login(**attrs)

        # 签发token，并将user和token存放到序列化对象中
        payload = jwt_payload_handler(user)
        token = jwt_encode_handler(payload)

        self.user = user
        self.token = token

        return attrs

   

```

```python
 # 多方式登录
    def _many_method_login(self, **attrs):
        username = attrs.get('username')
        password = attrs.get('password')
        if re.match(r'.*@.*', username):
            user = models.User.objects.filter(email=username).first()  # type: models.User

        elif re.match(r'^1[3-9][0-9]{9}$', username):
            user = models.User.objects.filter(mobile=username).first()
        else:
            user = models.User.objects.filter(username=username).first()

        if not user:
            raise ValidationError({'username': '账号有误'})

        if not user.check_password(password):
            raise ValidationError({'password': '密码有误'})

        return user

```



## 配置多方式登录：settings/dev.py

```
AUTHENTICATION_BACKENDS = ['user.utils.JWTModelBackend']
```

## 手动签发JWT：了解 - 可以拥有原生登录基于Model类user对象签发JWT

```python
from rest_framework_jwt.settings import api_settings

jwt_payload_handler = api_settings.JWT_PAYLOAD_HANDLER
jwt_encode_handler = api_settings.JWT_ENCODE_HANDLER

payload = jwt_payload_handler(user)
token = jwt_encode_handler(payload)
```







