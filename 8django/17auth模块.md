[TOC]

# Auth模块是什么

使用auth模块 必须用全套 不是自己写一部分 用别人一部分

**Auth模块是Django自带的用户认证模块：**

我们在开发一个网站的时候，无可避免的需要设计实现网站的用户系统。此时我们需要实现包括用户注册、用户登录、用户认证、注销、修改密码等功能，这还真是个麻烦的事情呢。

Django作为一个完美主义者的终极框架，当然也会想到用户的这些痛点。它内置了强大的用户认证系统--auth，它默认使用 auth_user 表来存储用户数据。

# 如何使用auto模块

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031174124280-829984029.png)

1.创建超级管理员，用来登陆用于登录django admin的后台管理

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031171935427-1288711529.png)



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031173201280-1526372995.png)



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031172951342-775049707.png)

接下来创建创建超级管理员

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031172158115-585647836.png)

**注意事项：设置密码的时候密码不能单一，并且需要8个字符才能够进行创建，并且密码是加密的，无法直接查看**



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031173640948-1227476955.png)

创建成功后可以看表内的内容，下面是所有的字段。

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031173916531-970122499.png)

登陆后

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031183532554-443360669.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031184529210-1366828095.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031184550424-1407443712.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031184625061-446902695.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031184658270-673414490.png)

# auth模块常用方法

```
from django.contrib import auth
```

## authenticate()（示例：登陆）

作用：校验用户是否存在

该方法会有一个返回值  当条件存在的情况下 返回就是数据对象本身,条件不满足 直接返回None

提供了用户认证功能，即验证用户名以及密码是否正确，一般需要username 、password两个关键字参数。

如果认证成功（用户名和密码正确有效），便会返回一个 User 对象。

authenticate()会在该 User 对象上设置一个属性来标识后端已经认证了该用户，且该信息在后续的登录过程中是需要的。

用法：

```
user = authenticate(username='username',password='password')
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031190105981-204960971.png)

```python
<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>password:<input type="text" name="password"></p>
    <p><input type="submit" value="提交"></p>
</form>
```



## login(HttpRequest, user)

作用：保存用户的登陆状态

```
auth.login(request,user_obj)  # 执行完这一句之后 只要是能够拿到request的地方 
        # 都可以通过request.user获取到当前登录用户对象
```



该函数接受一个HttpRequest对象，以及一个经过认证的User对象。

该函数实现一个用户登录的功能。它本质上会在后端为该用户生成相关session数据。

用法：**执行这个方法的时候，用户是需要存在，才行**

```
from django.contrib import auth
def login(request):
    if request.method=="POST":
        username = request.POST.get("username")
        password = request.POST.get("password")
        user_obj =auth.authenticate(username=username,password=password)
        print(user_obj)
        print(user_obj.username)
        print(user_obj.password)
        """该方法会主动帮你操作session表 并且只要执行了该方法
            你就可以在任何位置通过request.user获取到当前登录的用户对象
        """
        auth.login(request,user_obj)
        #一定要记录用户状态 才算真正的用户登录
    return render(request,'login.html')

def home(request):
    print(request.user)
    return HttpResponse('哈喽共享你进入home')
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031192145215-2119662981.png)

## is_authenticated()

作用：判断当前用户是否登录`request.user.is_authenticated()`

true是代表已经登陆，flase代表没有登陆

用来判断当前请求是否通过了认证。

用法：

```
def index(request):
    print(request.user)  # 直接拿到登录用户的用户对象
    print(request.user.is_authenticated,1)  # 简单快捷的判断用户是否登录
    print(request.user.is_authenticated(),2)
    return HttpResponse('index')
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031192753388-642618561.png)

## **login_requierd装饰器**

auth 给我们提供的一个装饰器工具，用来快捷的给某个视图添加登录校验。

必须先导入`from django.contrib.auth.decorators import login_requierd`

用法：

```
from django.contrib.auth.decorators import login_required
      
@login_required#给下面的视图添加装饰器，没有登陆的话，访问不了
def set_password(request):
	pass
```

若用户没有登录，则会跳转到django默认的 登录URL '/accounts/login/ ' 并传递当前访问url的绝对路径 (登陆成功后，会重定向到该路径)。

### 局部配置

如果需要自定义登录的URL,后面添加参数

```
from django.contrib.auth.decorators import login_required
      
@login_required(login_url="/login/")#给下面的视图添加装饰器，没有登陆的话，访问不了
def set_password(request):
	pass
```

### 全局配置

如果需要所有的都跳到自己写的指定URL，则需要在settings.py文件中通过LOGIN_URL进行修改。

示例：

```
auto校验全局是否登陆的配置
LOGIN_URL = '/login/'  # 这里配置成你项目登录页面的路由
```

```
from django.contrib.auth.decorators import login_required
      
@login_required#这个时候不需要添加参数了
def set_password(request):
	pass
```

## check_password(password)

auth 提供的一个检查密码是否正确的方法，需要提供当前请求用户的密码。

密码正确返回True，否则返回False。

用法：

```
ok = user.check_password('密码')
```

## set_password(password)

auth 提供的一个修改密码的方法，接收 要设置的新密码 作为参数。

注意：设置完一定要调用用户对象的save方法！！！

用法：

```
user.set_password(password='')
user.save()
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191031201612184-1977241061.png)

```
from django.contrib.auth.decorators import login_required
@login_required(login_url='/login')
def set_password(request):
    if request.method == 'POST':
        old_password = request.POST.get('old_password')
        new_password = request.POST.get('new_password')
        # 校验原密码对不对
        is_right = request.user.check_password(old_password)
        #判断密码是否正确，正确返回True
        print(is_right)
        if is_right:
            # 修改密码
            request.user.set_password(new_password)
            # 这个方法，仅仅只会在内存中产生一个缓存
            # 并不会直接修改数据库
            request.user.save()  # 一定要点save方法保存 才能真正的操作数据库
            return redirect('/login/')
    return render(request, 'set_password.html', locals())
```

```html
<form action="" method="post">
    {% csrf_token %}
    <p>username<input type="text" disabled value="{{ request.user.username }}"></p>{#disabled名字不能修改#}
    <p><input type="text" name="old_password"></p>
    <p><input type="text" name="new_password"></p>
    <input type="submit">
</form>
```



## create_user()

auth 提供的一个创建新用户的方法，需要提供必要参数（username、password）等。

用法：

```
from django.contrib.auth.models import User
user = User.objects.create_user（username='用户名',password='密码',email='邮箱',...）
```

## create_superuser()

auth 提供的一个创建新的超级用户的方法，需要提供必要参数（username、password）等。

用法：

```
from django.contrib.auth.models import User
user = User.objects.create_superuser（username='用户名',password='密码',email='邮箱',...）
```

![1572525572189](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/1572525572189.png)

## **logout(request)**

该函数接受一个HttpRequest对象，无返回值。

当调用该函数时，当前请求的session信息会全部清除。该用户即使没有登录，使用该函数也不会报错。

用法：

```
from django.contrib.auth import logout
   
def logout_view(request):
  request.logout(request)
  # Redirect to a success page.
```



## User对象的属性

User对象属性：username， password

is_staff ： 用户是否拥有网站的管理权限.

is_active ： 是否允许用户登录, 设置为 False，可以在不删除用户的前提下禁止用户登录。

 

# 扩展默认的auth_user表（扩展用户表）

这内置的认证系统这么好用，但是auth_user表字段都是固定的那几个，我在项目中没法拿来直接使用啊！

比如，我想要加一个存储用户手机号的字段，怎么办？

聪明的你可能会想到新建另外一张表然后通过一对一和内置的auth_user表关联，这样虽然能满足要求但是有没有更好的实现方式呢？

答案是当然有了。

我们可以通过继承内置的 AbstractUser 类，来定义一个自己的Model类。

这样既能根据项目需求灵活的设计用户表，又能使用Django强大的认证系统了。



**强调 你继承了AbstractUser之后 你自定义的表中 字段不能跟原有的冲突**

```
from django.db import models
from django.contrib.auth.models import AbstractUser
# Create your models here.

class Userinfo(AbstractUser):
    """
    强调 你继承了AbstractUser之后 你自定义的表中 字段不能跟原有的冲突
    """
    phone = models.BigIntegerField()
    avatar = models.FileField()
    register_time = models.DateField(auto_now_add=True)

    
    def __str__(self):
        return self.username
```

**注意**

按上面的方式扩展了内置的auth_user表之后，一定要在settings.py中告诉Django，我现在使用我新定义的UserInfo表代替auth_user表做用户认证。写法如下：

```
# 引用Django自带的User表，继承使用时需要设置
AUTH_USER_MODEL = "app名.UserInfo"
```

再次注意：

一旦我们指定了新的认证系统所使用的表，我们就需要重新在数据库中创建该表，而不能继续使用原来默认的auth_user表了。



#### 另外一种

​            利用一对一表关系 扩展字段