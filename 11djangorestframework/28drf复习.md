# 了解drf复习

```python
"""
1、html常用标签：link、meta、div、span、b、i、a、img、ul、table、form

2、css选择器，css三种布局
	div .div #div
	盒模型：margin
	浮动布局：float
	定位布局：position
	
3、js四种变量，js字符串、数组、对象的操作方法，js可变长参数
	let var 没有关键字 const

4、接口的四个核心部分：请求方式，请求地址，请求参数，响应结果
	get 取 post 增
	长得像返回数据的URL
	拼接参数还是数据包参数：key-value
	响应状态码，状态信息，数据

5、接口工具：写接口文档的YApi平台，访问接口的Postman工具
	接口文档：将接口的四个核心描述成文档
	Postman工具：测试接口的请求响应

6、restful接口规范：如何设计url，请求方式代表操作方式，网络状态码及其含义，响应结果
	https://api.oldboy.com/v2/users/?limit=3&search=张
	get post put(patch) delete
	网络状态码：2xx 4xx 5xx
	数据：{status，msg，results}


7、基于原生django书写满足restful规范的接口：两个url 对应 一个视图类 完成 十大接口
	/api/users/
	/api/users/(?P<pk>\d+)/
	class User(View):
		get|post|put|patch|delete方法

8、CBV请求生命周期：as_view()完成路由匹配 => url请求会调用as_view()的返回值视图函数view => 调用dispatch()完成请求分发 => 视图类的具体视图方法处理请求 => 返回给前台

9.
	安装drf：pip install djangorestframework
	视图类继承drf的APIView: from rest_framework.views import APIView
	读懂drf的as_view()方法：返回视图函数view是，局部禁用了csrf认证 - csrf_exempt(view)
	请求分发的dispatch()，
		在分发执行视图方法前，完成了
			二次封装request：self.initialize_request(request, *args, **kwargs)
			三大认证：self.initial(request, *args, **kwargs)
		在视图方法处理完请求后：
			出现异常的处理：self.handle_exception(exc)
			二次封装response：self.finalize_response(request, response, *args, **kwargs)
			
10、自己看drf源码入口：直接查看 rest_framework.view的APIView的dispatch方法
"""
```

# 解读源码复习

```python
"""
1、请求模块：drf的request是对wsgi的request二次封装，且完全兼容，拓展query_params和data两个属性
	request._request
	request._request = request
	request.query_params = request._request.GET
	request.data(form-data,urlencoded,json)
	
	
2、渲染模块：可以全局和局部配置渲染方式
	renderer_classes = []
	DEFAULT_RENDERER_CLASSES: []

3、解析模块：可以全局和局部配置能解析的数据包
	parser_classes = []
	DEFAULT_PAARSER_CLASSES: []

4、异常模块：自定义异常模块，可以自定义异常返回以及提供记录异常的接口(这里接口的意思就是该需求后期可以在这里添加)
	'EXCEPTION_HANDLER': 'api.utils.exception_handler',
	
	from rest_framework.views import exception_handler as drf_exception_handler
	def exception_handler(exc, context):
		response = drf_exception_handler(exc, context)
		if respose is None:
			return Response(自己处理) 	# 服务器错误 500
		return response  # drf处理的，客户端错误 4xx
		
5、响应模块：知道response对象产生可以传那些信息，response对象又是如何访问这些信息的
	Response(data={}, status=status.HTTP_200_OK, headers={})
"""
```

# 序列化组件复习

```python
"""
1、新建django项目、注册drf、设置国际化、配置数据库（修改数据库操作模块）、自定义Model类（设置文件类型字段和选项字段）、配置media及开放media资源接口
2、继承Serializer类的基础序列化类，可以将对象序列化成前台所需数据
3、继承Serializer类的基础反序列化类，可以指定一系列前台提供的数据的校验规则，确保数据安全
	class BookSerializer(serializers.Serializer):
		name = serializers.CharField(min_length=3, max_length=10)
		
		# 自定义序列化字段
		my_name = serializers.SerializerMethodField()
		def get_my_name(self, obj):
			return 'my_name的值，一般与obj有关系'
			
		# 自定义反序列化字段
		re_name = serializers.CharField(min_length=3, max_length=10)
		
		# required | write_only | read_only
		# 局部钩子
		def validate_name(self, value):
			if 'g' in name:
				raise serializers.ValidationError('名字异常')
			return value
		# 全局钩子
		def validate_name(self, attrs):
			attrs.pop('re_name')
			# 失败抛异常，成功返回attrs
		# 需求有增和改还需要重写create和update
		
		
		
	
4、重点掌握整合序列化与反序列为一体的继承ModelSerializer类的序列化类
	设置序列化与反序列化字段，并进行区分
	提供自定义序列化字段以及自定义反序列化字段
	设置系统校验规则、局部钩子校验规则与全局钩子校验规则
	class Book(models.Model):
		# 自定义序列化字段
		@property
		def my_name(self):
			return 'my_name的值，与Book类对象self有关'
		
	class BookModelSerializer(ModelSerializer):
		# 自定义反序列化字段，系统校验规则必须在声明中属性，在全局钩子移除
		re_name = serializers.CharField(min_length=3, max_length=10, write_only=True)
		class Meta:
			model = Book
			fields = ('name', 'price', 'my_name', 're_name')
			extra_kwargs = {
				# 系统校验
				'name': {
					'required': True
				}
			}
		# 局部全局钩子，和Serializer方式一样，且不用重写create和update方法
"""
```





## 知识点总结

```python
"""
1、(重点)二次封装Response：自定义APIResponse继承Response，重写__init__方法
{'status':0,'msg'ok','results':{}}
class APIResponse(Response):
	def __init__(self, status, msg, ..., **kwargs):
		# status+msg+kwargs => data
		super().__init__(data, ...)

2、(正常)设置了abstract为True的模型类，称之为基表，这样的模型类是专门作为基类来提供公有属性的
A(name)、B(title)-Meta-abstract=True、C(B)、D(B)

3、(重点)ORM多表关联操作：
	外键所放位置
        一对多：外键放在多的一方
		多对多：外键放在常用的一方
		一对一：外键放在不常用的一方
		外键字段为正向查询字段，related_name是反向查询字段
	外键如何断关联
		设置外键字段db_constraint=False
	外键间的级联关系
		一对一：作者没了，详情也没：on_delete=models.CASCADE
		一对多：出版社没了，书还是那个出版社出版：on_delete=models.DO_NOTHING
		一对多：部门没了，员工没有部门(空不能)：null=True, on_delete=models.SET_NULL
		一对多：部门没了，员工进入默认部门(默认值)：default=0, on_delete=models.SET_DEFAULT
		多对多：不能设置on_delete
4、(重点)连表序列化，在Model类中定义插拔序列化方法属性，完成连表查询
5、(正常)子序列化可以辅助快速实现自定义外键深度的序列化，但是不能进行反序化
6、(重点)单查群查接口，序列化类提供序列化对象，many参数控制着操作的数据是一条还是多条
ModelSerializer(obj) | ModelSerializer(obj, many=True)

7、(正常)单删群删接口，后台操作删除字段即可，前台提供pk就是单删，提供pks就是群删
pk,pks => pks => filter(pk__in=pks,is_delete=False).update(is_delete=True)

try:
	# 可能有异常的代码（数据库操作）
except:
	raise Exception('某某数据库操作异常了')
	
8、(重点)单增群增接口，根据数据判断是单增还是群增，对应序列化类要设置many，而序列化类只需要通过data即可
ModelSerializer(data=request_data) | ModelSerializer(data=request_data, many=True)

9、(正常)单局部改，序列化类参数instance=修改的对象, data=修改的数据, partial=是否能局部修改，单整体修改就是partial=False(默认就是False)
ModelSerializer(instance=obj,data=data,partial=True)

10、(了解)群改，前台提供的数据，后台要转化成要修改的对象们和用来更新的数据们，ModelSerializer设置list_serializer_class关联自己的ListSerializer，重写update方法，完成群改
ModelSerializer(instance=objs,data=datas,many=True,partial=True)
"""
```



# 多表序列化组件复习

```python
""" 视图家族
1、View：将请求方式与视图类的同名方法建立映射，完成请求响应
2、APView：
	1）View的所有功能；
  	2）重写as_view禁用csrf认证；
  	3）重写dispatch：请求、响应、渲染、异常、解析、三大认证
	4）多了一堆类属性，可以完成视图类的局部配置

3、GenericAPIView：
	1）APView的所有功能
	2）三个方法：get_object()、get_queryset()、get_serializer()
    3）三个属性：queryset、serializer_class、lookup_url_kwarg
    
4、mixins包：
	1）五大工具类：RetrieveModelMixin, ListModelMixin, CreateModelMixin, UpdateModelMixin, DestroyModelMixin
	2）六大工具方法：retrieve、list、create、update、partial_update、destroy
   
5、generics包：
	1）一堆mixins工具类与GenericAPIView视图基类组合
	
6、ViewSetMixin
	1
"""
```



# 视图家族复习

```python
""" 视图家族
1、View：将请求方式与视图类的同名方法建立映射，完成请求响应
2、APView：
	1）View的所有功能；
  	2）重写as_view禁用csrf认证；
  	3）重写dispatch：请求、响应、渲染、异常、解析、三大认证
	4）多了一堆类属性，可以完成视图类的局部配置

3、GenericAPIView：
	1）APView的所有功能
	2）三个方法：get_object()、get_queryset()、get_serializer()
    3）三个属性：queryset、serializer_class、lookup_url_kwarg
    
4、mixins包：
	1）五大工具类：RetrieveModelMixin(retrieve), ListModelMixin(list), CreateModelMixin(create), UpdateModelMixin(updata,partial_update), DestroyModelMixin(destroy)
	2）六大工具方法：retrieve、list、create、update、partial_update、destroy
   
5、generics包：
	1）一堆mixins工具类与GenericAPIView视图基类组合
	
6、ViewSetMixin
	1）重写as_view()，完成请求方式与视图方法的自定义映射
		as_view({'get': 'my_get'})
		
7、视图集基类：
	ViewSet(ViewSetMixin, APIView)：可以自定义映射关系的APIView
	GenericViewSet(ViewSetMixin, GenericAPIView)：可以自定义映射关系的GenericAPIView
	
8、常用Model视图集
	1）ModelViewSet(mixins.CreateModelMixin,
                   mixins.RetrieveModelMixin,
                   mixins.UpdateModelMixin,
                   mixins.DestroyModelMixin,
                   mixins.ListModelMixin,
                   GenericViewSet)
    	某一资源的六大操作视图集
    	
    2）ReadOnlyModelViewSet(mixins.RetrieveModelMixin,
                           mixins.ListModelMixin,
                           GenericViewSet)
        某一资源的只读操作视图集
"""
```



Content_type

```python
# 给Django中的所有模块中的所有表进行编号存储到content_type表中
# 应用一：权限表的权限是操作表的，所有在权限表中有一个content_type表的外键，标识该权限具体操作的是哪张表
# 应用二：价格策略

"""
Course：
name、type、days、price、vip_type
基础	免费课  7		0
中级	学位课	 180	69
究极	会员课	 360    	 至尊会员


Course：
name、type、days、content_type_id
基础	免费课  7	  null
中级	学位课	 180   1
究极	会员课	 360   2

app01_course_1
id、price

app01_course_2
id vip_type

content_type表（Django提供）
id、app_label、model
1	app01	 course_1
2	app01	 course_2
"""
```



# 三大认证复习

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





# 前后台分离模式下信息交互规则

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



## jwt知识点总结

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


