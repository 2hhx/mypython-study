## 复习

```python
"""
1、认证、权限、频率的工作原理
基础哪个类、重写哪个方法、方法的实现体要完成什么事

系统的认证类：都很少使用，常用的前后台分类认证 - jwt(json web token)

系统的权限类：
AllowAny：不限制
IsAuthenticated：必须是登录用户
IsAdminUser：必须是后台用户
IsAuthenticatedOrReadOnly：读操作无限制，其他操作需要登录

2、自定义User表
继承AbstractUser、配置AUTH_USER_MODEL、配置admin（UserAdmin密文操作密码）

3、一个需要登录后的群查接口UserList、一个获取LoginAPIView成功的Token

4、LoginAPIView要根据请求的usr、pwd交给序列化类，全局校验得到 user、token
	签发token的算法
	
5、自定义认证类，校验token
	校验token的算法
	
6、自定义权限类
	指定权限规则

7、登录接口必须完成所有认证权限局部禁用，权限接口在权限类中局部配置自定义认证权限类(或在全局配置)
"""
```



## jwt认证规则

```python
"""
全称：json web token
解释：加密字符串的原始数据是json，后台产生，通过web传输给前台存储
格式：三段式 - 头.载荷.签名 - 头和载荷才有的是base64可逆加密，签名才有md5不可逆加密
内容：
	头(基础信息,也可以为空)：加密方式、公司信息、项目组信息、...
	载荷(核心信息)：用户信息、过期时间、...
	签名(安全保障)：头加密结果+载荷加密结果+服务器秘钥 的md5加密结果
	
	
认证规则：
	后台一定要保障 服务器秘钥 的安全性(它是jwt的唯一安全保障)
	后台签发token -> 前台存储 -> 发送需要认证的请求带着token -> 后台校验得到合法的用户
	
	
为什么要才有jwt认证：
	1) 后台不需要存储token，只需要存储签发与校验token的算法，效率远远大于后台存储和取出token完成校验
	2) jwt算法认证，更适合服务器集群部署
"""
```



## jwt模块

```python
"""
安装：pip install djangorestframework-jwt
模块包：rest_framework_jwt

才有drf-jwt框架，后期任务只需要书写登录
	为什么要重写登录：drf-jwt只完成了账号密码登录，我们还需要手机登录，邮箱登录
	为什么不需要重写认证类：因为认证规则已经完成且固定不变，变得只有认证字符串的前缀，前缀可以在配置文件中配置
"""
```





## 前后台分离模式下信息交互规则

```python
"""
1）任何人都能直接访问的接口
	请求不是是get、还是post等，不需要做任何校验

2）必须登录后才能访问的接口
	任何请求方式都可能做该方式的限制，请求必须在请求头中携带认证信息 - authorization
	
3）前台的认证信息获取只能通过登录接口
	前台提供账号密码等信息，去后台换认证信息token
	
4）前台如何完成登录注销
	前台登录成功一般在cookie中保存认证信息token，分离注销就是前台主动清除保存的token信息
"""
```



## 知识点总结

```python
"""
1、jwt认证：三段式的格式、每一段的内容、由后台签发到前台存储再到传给后台校验的认证流水线

2、drf-jwt插件：
	三个接口：签发token、校验token、刷新token
	自定义jwt插件的配置
	
3、使用jwt插件完成多方式登录
	视图类：将请求数据交给序列化类完成校验，然后返回用户信息和token(从序列化对象中拿到)
	序列化类：自定义反序列化字段，全局钩子校验数据得到user和token，并保存在序列化类对象中
		token可以用jwt插件的rest_framework_jwt.serializers中
			jwt_payload_handler，jwt_encode_handler
		完成签发

4、自定义频率类完成视图类的频率限制
	1）定义类继承SimpleRateThrottle,重写get_cache_key方法，设置scope类属性
	2）scope就是一个认证字符串，在配置文件中配置scope字符串对应的频率设置
	3）get_cache_key的返回值是字符串，该字符串是缓存访问次数的缓存key	
"""
```





## A作业（必做）

```python
"""
1、整理今天所学知识点

2、新建项目，完成有可视化前台的登录，注册，注销

3、登录成功，可视化访问个人中心
"""
```



## B作业（选做）

```python
"""
1、预习路飞项目
"""
```



























