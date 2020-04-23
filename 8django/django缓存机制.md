# [Django的缓存机制](https://www.cnblogs.com/zuoshoushizi/p/7850281.html)



由于Django是动态网站，所有每次请求均会去数据进行相应的操作，当程序访问量大时，耗时必然会更加明显，最简单解决方式是使用：缓存，缓存将一个某个views的返回值保存至内存或者memcache中，5分钟内再有人来访问时，则不再去执行view中的操作，而是直接从内存或者Redis中之前缓存的内容拿到，并返回。

Django中提供了6种缓存方式：

* 开发调试
* 内存
* 文件
* 数据库
* Memcache缓存（python-memcached模块）
* Memcache缓存（pylibmc模块）

通用配置



```
'TIMEOUT': 300,                                               # 缓存超时时间（默认300，None表示永不过期，0表示立即过期）
                'OPTIONS':{
                    'MAX_ENTRIES': 300,                                       # 最大缓存个数（默认300）
                    'CULL_FREQUENCY': 3,                                      # 缓存到达最大个数之后，剔除缓存个数的比例，即：1/CULL_FREQUENCY（默认3）
                },
                'KEY_PREFIX': '',                                             # 缓存key的前缀（默认空）
                'VERSION': 1,                                                 # 缓存key的版本（默认1）
                'KEY_FUNCTION' 函数名                                          # 生成key的函数（默认函数会生成为：【前缀:版本:key】）
```



以上六中模式都可以使用

自定义key



```
 def default_key_func(key, key_prefix, version):
        """
        Default function to generate keys.

        Constructs the key used by all other methods. By default it prepends
        the `key_prefix'. KEY_FUNCTION can be used to specify an alternate
        function with custom key making behavior.
        """
        return '%s:%s:%s' % (key_prefix, version, key)

    def get_key_func(key_func):
        """
        Function to decide which key function to use.

        Defaults to ``default_key_func``.
        """
        if key_func is not None:
            if callable(key_func):
                return key_func
            else:
                return import_string(key_func)
        return default_key_func
```



### 开发调试





```
    # 此为开始调试用，实际内部不做任何操作
    # 配置：
        CACHES = {
            'default': {
                'BACKEND': 'django.core.cache.backends.dummy.DummyCache',     # 引擎
              通用配置
            }
        }
```





### 内存





```
    # 此缓存将内容保存至内存的变量中
    # 配置：
        CACHES = {
            'default': {
                'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
                'LOCATION': 'unique-snowflake',
              通用配置
            }
        }

    # 注：其他配置同开发调试版本
```





### 文件





```
    # 此缓存将内容保存至文件
    # 配置：

        CACHES = {
            'default': {
                'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache',
                'LOCATION': '/var/tmp/django_cache',
                 通用配置
            }
        }
    # 注：其他配置同开发调试版本
```





### 数据库





```
 # 此缓存将内容保存至数据库

    # 配置：
        CACHES = {
            'default': {
                'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
                'LOCATION': 'my_cache_table', # 数据库表
              通用配置
            }
        }

    # 注：执行创建表命令 python manage.py createcachetable
```





### Memcache缓存（python-memcached模块）





```
# 此缓存使用python-memcached模块连接memcache

    CACHES = {
        'default': {
            'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
            'LOCATION': '127.0.0.1:11211',
        }
    }

    CACHES = {
        'default': {
            'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
            'LOCATION': 'unix:/tmp/memcached.sock',
        }
    }   

    CACHES = {
        'default': {
            'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
            'LOCATION': [
                '172.19.26.240:11211',
                '172.19.26.242:11211',
            ]
        }
    }
```





### Memcache缓存（pylibmc模块）





```
 # 此缓存使用pylibmc模块连接memcache
    
    CACHES = {
        'default': {
            'BACKEND': 'django.core.cache.backends.memcached.PyLibMCCache',
            'LOCATION': '127.0.0.1:11211',
        }
    }

    CACHES = {
        'default': {
            'BACKEND': 'django.core.cache.backends.memcached.PyLibMCCache',
            'LOCATION': '/tmp/memcached.sock',
        }
    }   

    CACHES = {
        'default': {
            'BACKEND': 'django.core.cache.backends.memcached.PyLibMCCache',
            'LOCATION': [
                '172.19.26.240:11211',
                '172.19.26.242:11211',
            ]
        }
    }
```





 

 

### 缓存的应用

#### 单独视图缓存

```
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)
def my_view(request):
            ...
```

即通过装饰器的方式实现，导入模块之后，在需要缓存的函数前加@cache_page(60 * 15) 60*15表示缓存时间是15分钟

例子如下：





```
from django.views.decorators.cache import cache_page
@cache_page(10)
def cache(request):
    import time
    ctime = time.time()
    return  render(request,"cache.html",{"ctime":ctime})
```





前端页面如下：





```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>{{ ctime }}</h1>
    <h1>{{ ctime }}</h1>
    <h1>{{ ctime }}</h1>

</body>
</html>
```





这样在前端页面在获取的ctime的时候就会被缓存10秒钟，10秒钟之后才会变化，但是这样的话就相当月所有的调用ctime的地方都被缓存了

#### 局部缓存





```
引入TemplateTag

{% load cache %}

使用缓存

{% cache 5000 缓存key %}
缓存内容
{% endcache %}
```





更改前端代码如下：





```
{% load cache %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>{{ ctime }}</h1>
    <h1>{{ ctime }}</h1>
    {% cache 10 c1 %}
    <h1>{{ ctime }}</h1>
    {% endcache %}
</body>
</html>
```





这样就实现了最后一个ctime缓存，其他两个不缓存

#### 全站缓存

全站缓存的时候，需要在中间件的最上面添加：

'django.middleware.cache.UpdateCacheMiddleware',

在中间件的最下面添加：

'django.middleware.cache.FetchFromCacheMiddleware',

 

其中'django.middleware.cache.UpdateCacheMiddleware'里面只有process_response方法，在'django.middleware.cache.FetchFromCacheMiddleware'中只有process_request方法，所以最开始是直接跳过UpdateCacheMiddleware，然后从第一个到最后一个中间件的resquest,第一次没有缓存座椅匹配urls路由关系依次进过中间件的process_view,到达views函数，再经过process_exception最后经过response,到达FetchFromCacheMiddleware