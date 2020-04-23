

[TOC]



# Cookie和Session

## 为什么会有cookie和session?

由于http协议是无状态的 无法记住用户是谁，cookie主要是在浏览器上记录客户的状态，session主要是用来在服务端记录客户的状态。

# cookie

## Cookie的由来

大家都知道HTTP协议是无状态的。

无状态的意思是每次请求都是独立的，它的执行情况和结果与前面的请求和之后的请求都无直接关系，它不会受前面的请求响应情况直接影响，也不会直接影响后面的请求响应情况。

一句有意思的话来描述就是人生只如初见，对服务器来说，每次的请求都是全新的。

状态可以理解为客户端和服务器在某次会话中产生的数据，那无状态的就以为这些数据不会被保留。会话中产生的数据又是我们需要保存的，也就是说要“保持状态”。因此Cookie就是在这样一个场景下诞生。

## 什么是Cookie

**cookie是保存在客户端浏览器上的键值对**，是服务端设置在客户端浏览器上的键值对，也就意味着浏览器其实可以拒绝服务端的"命令"，浏览器有权限禁止服务端写入cookie,默认情况下 浏览器都是直接让服务端设置键值对,

Cookie具体指的是一段小信息，下次访问服务器时浏览器会自动携带这些键值对，以便服务器提取有用信息。

## Cookie的原理

cookie的工作原理是：由服务器产生内容，浏览器收到请求后保存在本地；当浏览器再次访问时，浏览器会自动带上Cookie，这样服务器就能通过Cookie的内容来判断这个是“谁”了。

## 查看Cookie

我们使用Chrome浏览器，打开开发者工具。

![img](https://images2018.cnblogs.com/blog/867021/201804/867021-20180403225926558-498750585.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191030201641238-129730214.png)

# Django中操作Cookie

由于视图函数返回的都是HttpResponse对象（render,rendict,HttpResponse），所以可以操作该对象来操作,利用该对象来操作cookie。

`return HttpResponse()`

## 操作

```
obj = HttpResponse()
obj.set_cookie(key,value)#告诉浏览器在本地保存一个键值对
obj.set_cookie(key,value,max_age=5)#设置超时时间
return obj
```





## 基于cookie做用户的登录认证

1. request.COOKIES.get(key)  # 检验用户是否登录
2. 如何保存用户在没有登录之前想访问的那个页面的url，然后当用户用户名和密码输入正确点击登录之后再跳转会去
3. **request.path_info  # 只拿后缀**
4. **request.get_full_path()  # 后缀加get请求携带的参数**
5. 利用上面的方法在装饰器中获取用户想要访问的url
6. 在跳转到登录页面的url的时候 以get请求携带参数的方式将用户想要访问的url携带过去
7. 在登录的视图函数中 对url进行判断 
   1. 用户没有登录的情况下访问了一个必须登录的页面
   2. 用户直接访问的就是登录页面

```python
def login(request):
    # print(request.path_info)  # 只拿url 不拿get请求携带的额参数
    # print(request.get_full_path())  # 都拿

    if request.method == "POST":
        username = request.POST.get('username')
        password = request.POST.get('password')
        if username == 'jason' and password == '123':
            old_path = request.GET.get('next')
            if old_path:
                # 保存用户登录状态
                obj = redirect(old_path)
            else:
                obj = redirect('/home/')
            obj.set_cookie('name','jason')  # 让客户端浏览器 记录一个键值对
            # obj.set_cookie('name','jason',max_age=5)  # 让客户端浏览器 记录一个键值对
            return obj
    return render(request,'login.html')
```

装饰器，对要访问的页面进行修饰

```
from functools import wraps
def login_auth(func):
    @wraps(func)  # 装饰器修复技术，让其更加完美，如果不加这个的话，函数内部会打印装饰器内的注释
    # 加上之后，打印的是被装饰的函数
    def inner(request, *args, **kwargs):
        if request.COOKIES.get('name'):
            res = func(request, *args, **kwargs)
            print(res, 6)
            return res
        else:
            target_url = request.path_info
            print(target_url, 5)
            return redirect('/login/?next=%s' % target_url)
    return inner
```

```
@login_auth
def home(request):
    obj = HttpResponse('我是主页只能登陆后才能进来')
    return obj
@login_auth
def index(request):
    return HttpResponse("ddd")
```

### 获取Cookie

```python
if request.COOKIES.get('name'):#获取cookies
    res = func(request, *args, **kwargs)
    return res
```



```
request.COOKIES['key']
request.get_signed_cookie(key, default=RAISE_ERROR, salt='', max_age=None)
```

参数：

- default: 默认值
- salt: 加密盐
- max_age: 后台控制过期时间

### 设置Cookie

```python
	obj = redirect('/home/')
obj.set_cookie('name', 'json')
return obj
```



```
rep = HttpResponse(...)
rep ＝ render(request, ...)

rep.set_cookie(key,value,...)
rep.set_signed_cookie(key,value,salt='加密盐', max_age=None, ...)
```

设置时间

```
obj = redirect('/home/')
obj.set_cookie('name', 'json',max_age=5)
return obj
max_age=None, 超时时间,秒
```



参数：

- key, 键
- value='', 值
- max_age=None, 超时时间
- expires=None, 超时时间(IE requires expires, so set it if hasn't been already.)
- path='/', Cookie生效的路径，/ 表示根路径，特殊的：根路径的cookie可以被任何url的页面访问
- domain=None, Cookie生效的域名
- secure=False, https传输
- httponly=False 只能http协议传输，无法被JavaScript获取（不是绝对，底层抓包可以获取到也可以被覆盖）

### 删除Cookie

```
@login_auth
def logout(request):
    obj = redirect('/login/')
    obj.delete_cookie('name')# 删除用户浏览器上之前设置的usercookie值
    return obj  

```



# Session

**session是保存在服务器上的键值对**,由于cookie是将所有的关键性信息保存在客户端浏览器上的 数据不是很安全

所以有了session:session就是将数据保存在服务端 只给客户端浏览器一个随机字符串

服务端记录了 随机字符串与真实数据的对应关系

## Session的由来

Cookie虽然在一定程度上解决了“保持状态”的需求，但是由于Cookie本身最大支持4096字节，以及Cookie本身保存在客户端，可能被拦截或窃取，因此就需要有一种新的东西，它能支持更多的字节，并且他保存在服务器，有较高的安全性。这就是Session。

问题来了，基于HTTP协议的无状态特征，服务器根本就不知道访问者是“谁”。那么上述的Cookie就起到桥接的作用。

我们可以给每个客户端的Cookie分配一个唯一的id，这样用户在访问时，通过Cookie，服务器就知道来的人是“谁”。然后我们再根据不同的Cookie的id，在服务器上保存一段时间的私密资料，如“账号密码”等等。

总结而言：Cookie弥补了HTTP无状态的不足，让服务器知道来的人是“谁”；但是Cookie以文本的形式保存在本地，自身安全性较差；所以我们就通过Cookie识别不同的用户，对应的在Session里保存私密的信息以及超过4096字节的文本。

另外，上述所说的Cookie和Session其实是共通性的东西，不限于语言和框架。

# Django中Session相关方法

![1572438988352](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/1572438988352.png)

**django session表是针对浏览器的，不同的浏览器来 才会有不同的记录**

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191030205441417-1651013698.png)

### 前期准备

首先要进行数据库迁移命令`python manage.py makemigrations`

`python manage.py migrate`

,迁移完成之后会**有django_session表，这个表用来存储session**

## 设置session

```python
def set_session(request):
    request.session['username']="sky" ## 仅仅只会在内存产生一个缓存
    return HttpResponse("设置session")
```

**设置session的时候发生了什么？**

```python
1.django内部自动生成了随机的字符串
2.在django_session表中存入数据,在服务端默认情况下需要django_session表来存储session信息(没有执行数据库迁移命令会报错)django session默认的过期时间是14天
session_key          session_data         date
随机字符串1              数据1           	 ...
随机字符串2              数据2            	...
随机字符串3              数据3            	...
3.将产生的随机字符串发送给浏览器 让浏览器保存到cookie中sessionid:随机字符串
```

## 设置多个session

```
def set_session(request):
    request.session['username']="sky" ## 仅仅只会在内存产生一个缓存
    request.session['passion']="123"
    return HttpResponse("设置session")
```

在数据表中还是一条数据，随机字符串会发生改变





## 获取session

```python
def get_session(request):
    print(request.session.get('username'))
    return HttpResponse('获取session')
```

**获取session发生了什么？**

```python
1.浏览器发送cookie到django后端之后 django会自动获取到cookie值
2.拿着随机字符串去django_session表中比对 是否有对应的数据
3.无论有没有数据 你都可以通过request.session.get()
1.没有返回的结果就是None
2.有则返回随机字符串所对应的真实数据 
```

## 删除session

```python
删除session
	request.session.delete()  # 只删除服务端的session
                
	request.session.flush()  # 浏览器和服务端全部删除

```

## session设置超时时间

     request.session.set_expiry(value多种配置)
                    数字
                    0 
                    不写
                    时间格式
    request.session.set_expiry(value)
        * 如果value是个整数，session会在些秒数后失效。
        * 如果value是个datatime或timedelta，session就会在这个时间后失效。
        * 如果value是0,用户关闭浏览器session就会失效。
        * 如果value是None,session会依赖全局session失效策略。
# 其他的session设置

```
# 获取、设置、删除Session中数据
request.session['k1']#不推荐使用
request.session.get('k1',None)
request.session['k1'] = 123
request.session.setdefault('k1',123) # 存在则不设置
del request.session['k1']


# 所有 键、值、键值对
request.session.keys()
request.session.values()
request.session.items()
request.session.iterkeys()
request.session.itervalues()
request.session.iteritems()

# 会话session的key
request.session.session_key

# 将所有Session失效日期小于当前日期的数据删除
request.session.clear_expired()

# 检查会话session的key在数据库中是否存在
request.session.exists("session_key")

# 删除当前会话的所有Session数据
request.session.delete()
　　
# 删除当前的会话数据并删除会话的Cookie。
request.session.flush() 
    这用于确保前面的会话数据不可以再次被用户的浏览器访问
    例如，django.contrib.auth.logout() 函数中就会调用它。

# 设置会话Session和Cookie的超时时间
request.session.set_expiry(value)
    * 如果value是个整数，session会在些秒数后失效。
    * 如果value是个datatime或timedelta，session就会在这个时间后失效。
    * 如果value是0,用户关闭浏览器session就会失效。
    * 如果value是None,session会依赖全局session失效策略。
```





# Session流程解析

![img](https://images2018.cnblogs.com/blog/867021/201805/867021-20180514143525989-365124875.png)

# Session版登陆验证





```
from functools import wraps


def check_login(func):
    @wraps(func)
    def inner(request, *args, **kwargs):
        next_url = request.get_full_path()
        if request.session.get("user"):
            return func(request, *args, **kwargs)
        else:
            return redirect("/login/?next={}".format(next_url))
    return inner


def login(request):
    if request.method == "POST":
        user = request.POST.get("user")
        pwd = request.POST.get("pwd")

        if user == "alex" and pwd == "alex1234":
            # 设置session
            request.session["user"] = user
            # 获取跳到登陆页面之前的URL
            next_url = request.GET.get("next")
            # 如果有，就跳转回登陆之前的URL
            if next_url:
                return redirect(next_url)
            # 否则默认跳转到index页面
            else:
                return redirect("/index/")
    return render(request, "login.html")


@check_login
def logout(request):
    # 删除所有当前请求相关的session
    request.session.delete()
    return redirect("/login/")


@check_login
def index(request):
    current_user = request.session.get("user", None)
    return render(request, "index.html", {"user": current_user})
```



# Django中的Session配置

Django中默认支持Session，其内部提供了5种类型的Session供开发者使用。

直接设置在sessings中就行。

## session能保存在那些地方？

session是保存在服务端的加键值对

可以保存在：

1. 数据库
2. 文件
3. 缓存数据库
4. 后端有很多方法来保存session信息，并不是只能保存在数据库中。

```
1. 数据库Session
SESSION_ENGINE = 'django.contrib.sessions.backends.db'   # 引擎（默认）

2. 缓存Session
SESSION_ENGINE = 'django.contrib.sessions.backends.cache'  # 引擎
SESSION_CACHE_ALIAS = 'default'                            # 使用的缓存别名（默认内存缓存，也可以是memcache），此处别名依赖缓存的设置

3. 文件Session
SESSION_ENGINE = 'django.contrib.sessions.backends.file'    # 引擎
SESSION_FILE_PATH = None                                    # 缓存文件路径，如果为None，则使用tempfile模块获取一个临时地址tempfile.gettempdir() 

4. 缓存+数据库
SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'        # 引擎

5. 加密Cookie Session
SESSION_ENGINE = 'django.contrib.sessions.backends.signed_cookies'   # 引擎

其他公用设置项：
SESSION_COOKIE_NAME ＝ "sessionid"                       # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串（默认）
SESSION_COOKIE_PATH ＝ "/"                               # Session的cookie保存的路径（默认）
SESSION_COOKIE_DOMAIN = None                             # Session的cookie保存的域名（默认）
SESSION_COOKIE_SECURE = False                            # 是否Https传输cookie（默认）
SESSION_COOKIE_HTTPONLY = True                           # 是否Session的cookie只支持http传输（默认）
SESSION_COOKIE_AGE = 1209600                             # Session的cookie失效日期（2周）（默认）
SESSION_EXPIRE_AT_BROWSER_CLOSE = False                  # 是否关闭浏览器使得Session过期（默认）
SESSION_SAVE_EVERY_REQUEST = False                       # 是否每次请求都保存Session，默认修改之后才保存（默认）
```











# CBV中加装饰器相关(装饰器修复)

CBV实现的登录视图





```
class LoginView(View):

    def get(self, request):
        """
        处理GET请求
        """
        return render(request, 'login.html')

    def post(self, request):
        """
        处理POST请求 
        """
        user = request.POST.get('user')
        pwd = request.POST.get('pwd')
        if user == 'alex' and pwd == "alex1234":
            next_url = request.GET.get("next")
            # 生成随机字符串
            # 写浏览器cookie -> session_id: 随机字符串
            # 写到服务端session：
            # {
            #     "随机字符串": {'user':'alex'}
            # }
            request.session['user'] = user
            if next_url:
                return redirect(next_url)
            else:
                return redirect('/index/')
        return render(request, 'login.html')
```





要在CBV视图中使用我们上面的check_login装饰器，有以下三种方式：

from django.utils.decorators import method_decorator

## **1. 加在CBV视图的get或post方法上**





```
from django.utils.decorators import method_decorator


class HomeView(View):

    def dispatch(self, request, *args, **kwargs):
        return super(HomeView, self).dispatch(request, *args, **kwargs)

    def get(self, request):
        return render(request, "home.html")
    
    @method_decorator(check_login)
    def post(self, request):
        print("Home View POST method...")
        return redirect("/index/")
```





## **2. 加在dispatch方法上**





```
from django.utils.decorators import method_decorator


class HomeView(View):

    @method_decorator(check_login)
    def dispatch(self, request, *args, **kwargs):
        return super(HomeView, self).dispatch(request, *args, **kwargs)

    def get(self, request):
        return render(request, "home.html")

    def post(self, request):
        print("Home View POST method...")
        return redirect("/index/")
```





因为CBV中首先执行的就是dispatch方法，所以这么写相当于给get和post方法都加上了登录校验。

## **3. 直接加在视图类上，但method_decorator必须传 name 关键字参数**

如果get方法和post方法都需要登录校验的话就写两个装饰器。





```
from django.utils.decorators import method_decorator

@method_decorator(check_login, name="get")
@method_decorator(check_login, name="post")
class HomeView(View):

    def dispatch(self, request, *args, **kwargs):
        return super(HomeView, self).dispatch(request, *args, **kwargs)

    def get(self, request):
        return render(request, "home.html")

    def post(self, request):
        print("Home View POST method...")
        return redirect("/index/")
```

## 补充

CSRF Token相关装饰器在CBV只能加到dispatch方法上，或者加在视图类上然后name参数指定为dispatch方法。

备注：

- csrf_protect，为当前函数强制设置防跨站请求伪造功能，即便settings中没有设置全局中间件。
- csrf_exempt，取消当前函数防跨站请求伪造功能，即便settings中设置了全局中间件。

```
from django.views.decorators.csrf import csrf_exempt, csrf_protect
from django.utils.decorators import method_decorator


class HomeView(View):

    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return super(HomeView, self).dispatch(request, *args, **kwargs)

    def get(self, request):
        return render(request, "home.html")

    def post(self, request):
        print("Home View POST method...")
        return redirect("/index/")
```

或者

```
from django.views.decorators.csrf import csrf_exempt, csrf_protect
from django.utils.decorators import method_decorator


@method_decorator(csrf_exempt, name='dispatch')
class HomeView(View):
   
    def dispatch(self, request, *args, **kwargs):
        return super(HomeView, self).dispatch(request, *args, **kwargs)

    def get(self, request):
        return render(request, "home.html")

    def post(self, request):
        print("Home View POST method...")
        return redirect("/index/")
```

























