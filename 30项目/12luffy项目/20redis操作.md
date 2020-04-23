##### redis VS mysql

```python
"""
redis: 内存数据库(读写快)、非关系型(操作数据方便)
mysql: 硬盘数据库(数据持久化)、关系型(操作数据间关系)

大量访问的临时数据，才有redis数据库更优
"""
```

##### redis VS memcache

```python
"""
redis: 操作字符串、列表、字典、无序集合、有序集合 | 支持数据持久化(数据丢失可以找回、可以将数据同步给mysql) | 高并发支持
memcache: 操作字符串 | 不支持数据持久化 | 并发量小
"""
```



##### Redis操作

```python
"""
基础操作：
	启动服务：redis-server &
	连接数据库：redis-cli
    连接指定数据库：redis-cli -h 127.0.0.1 -p 6379 -n 1  #数据库1
    切换数据库：select 1  #数据库1

数据操作：字符串、列表、字典、无序集合、有序(排序)集合
	有序集合：游戏排行榜
	
"""
```







## redis数据库

```python
# 1.安装redis与可视化操作工具

# 2.在服务中管理redis服务器的开启关闭

# 3.命令行简单使用redis：
	-- redis-cli  # 启动客户端
    -- set key value  # 设置值
    -- get key  # 取出值
    
# 4.redis支持：字符串、字典、列表、集合、有序集合
# https://www.runoob.com/redis/redis-tutorial.html

# 5.特点：可持久化、单线程单进程并发
```

## python使用redis

##### 依赖

```
>: pip3 install redis
```

##### 直接使用

```python
import redis
r = redis.Redis(host='127.0.0.1', port=6379, db=1)
```

##### 连接池使用

```python
import redis
pool = redis.ConnectionPool(host='127.0.0.1', port=6379, db=10, max_connections=100)
r = redis.Redis(connection_pool=pool)
```

##### 缓存使用：要额外安装 django-redis

```python
# 1.将缓存存储位置配置到redis中：settings.py
# 缓存数据库的配置

CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        # 分配redis缓存数据库，选择第11个
        "LOCATION": "redis://127.0.0.1:6379/11",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
            # 连接池最大连接100个进程
            "CONNECTION_POOL_KWARGS": {"max_connections": 100}
        }
    }
}

# 2.操作cache模块直接操作缓存：views.py
from django.core.cache import cache  # 结合配置文件实现插拔式
# 存放token，可以直接设置过期时间
cache.set('token', 'header.payload.signature', 10)
# 取出token
token = cache.get('token')
```

