# 方法一

![](https://img-blog.csdnimg.cn/20190606134146148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pqd196eWZ4,size_16,color_FFFFFF,t_70)

```
class SoloveCrossDomainMiddleware(object):
    def process_response(self, request, response):
        """
        处理跨域的中间件，将所有的响应都能实现跨域
        """
        response['Access-Control-Allow-Origin'] = "*"
        response['Access-Control-Allow-Headers'] = "Content-Type"
        return response

Django版本:Django (1.11.8)

如果是之前的版本SoloveCrossDomainMiddleware类需要继承MiddlewareMixin即
```



```
from django.utils.deprecation import MiddlewareMixin
class SoloveCrossDomainMiddleware(MiddlewareMixin):
```

## 方法二

```
class MiddlewareMixin(object):
    def __init__(self, get_response=None):
        self.get_response = get_response
        super(MiddlewareMixin, self).__init__()

    def __call__(self, request):
        response = None
        if hasattr(self, 'process_request'):
            response = self.process_request(request)
        if not response:
            response = self.get_response(request)
        if hasattr(self, 'process_response'):
            response = self.process_response(request, response)
        return response



class CORSMiddleware(MiddlewareMixin):
    def process_response(self,request,response):
        # 添加响应头
        # 允许你的域名来获取我的数据
        # response['Access-Control-Allow-Origin'] = "*"
        # 允许你携带Content-Type请求头
        # response['Access-Control-Allow-Headers'] = "Content-Type"
        # 允许你发送DELETE,PUT
        # response['Access-Control-Allow-Methods'] = "DELETE,PUT"
        response['Access-Control-Allow-Origin'] = "*"
        if request.method == "OPTIONS":
            response['Access-Control-Allow-Headers'] = "Content-Type"
            response['Access-Control-Allow-Methods'] = "PUT,DELETE"
        return response
```

