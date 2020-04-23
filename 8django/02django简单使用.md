[TOC]

## 用户访问内容

用户能够访问的所有的资源，都是程序猿提前暴露的，如果没有暴露，用户是不能进行访问的。

## diango重启的问题

，当我们更改django中的代码的时候，django内部会检测到我们更改，所以会重启。django是可以自动重启的 但是有时候反应速度比较慢,也有可能在你代码没写完的时候重启了 会报错 但是不用管，修改完毕后，可以自己进行启动。

## diango中的三板斧，主要用于前后端的交互

1. **必须要有返回值**
2. 

```python
from django.shortcuts import render,HttpResponse,redirect
    	HttpResponse  # 返回字符串
        
        render  # 返回html页面 并且可以给该html传值
        
        redirect  # 重定向
            # 既可以是我们自己的路径也可以是网上的路径
        
        django返回的都是HttpResponse对象
```

```python
def player(request):
    res = HttpResponse('dfdf')
    print(res)
    return res





from django.template import Template,Context
def test(request):
#可以直接解析html
    return HttpResponse("<h1>哈喽</h1>")
    # res = Template("<h1>{{user}}</h1>")
    # con = Context({"user":"json",'pwd':123})
    # ret = res.render(con)
    # print(ret)
    # return HttpResponse(ret)
```

```python
def sing(request):
    res = redirect('/login')
    print(res)
    return res
```

```python
def dance(request):
    res = render(request, 'cuang.html')
    print(res)
    return res

可以加入参数：参数会返回cuang.html页面中，进行渲染页面
def reg(request):
    user_dict = {'name':'jason','pwd':123}
    # return render(request,'reg.html')
    # 给模板传值的方式1
    # return render(request, 'reg.html',{'xxx':user_dict})  # 将user_dict传递给reg.html页面 页面上通过xxx就能够获取到该字典
    # 给模板传值的方式2
    return render(request,'reg.html',locals())  # 会将当前名称空间中所有的变量名全部传递给reg.html页面
    # locals()  会出现效率问题
```

## 静态文件的配置

1. 用户能够在浏览器中输入网址访问到相应的资源。
2. 前提是后端暴露了该资源的接口。
3. 在django中如果你想让用户访问到对应的资源，我们只需要在urls.py中设置对应的关系。
4. 反过来如果我没有urls.py中开设资源，用户就永远访问不到对应的资源。
5. 返回给浏览器的html页面上的所有的静态资源，也需要请求后端加载获取。
6. 通常我们将网址所用到的html文件全部放在templates文件夹下，网站用到的静态资源全部存放到static文件夹下。

```python
2.静态文件配置
    静态文件
        网站所用到的
            自己写好js
            自己写好css
            第三方的框架 bootstrap fontwesome sweetalert,elementui,layui
        
    通常情况下 网站所用到的静态文件资源 统一都放在static文件夹下
    STATIC_URL = '/static/'  # 是访问静态资源的接口前缀
    """只要你想访问静态资源 你就必须以static开头"""
    # 手动配置静态文件访问资源
    STATICFILES_DIRS = [
        os.path.join(BASE_DIR,'static'),
        os.path.join(BASE_DIR,'static1'),
        # os.path.join(BASE_DIR,'static2'),
    ]
    
    html中   接口前缀 动态解析，这样可以修改STATIC_URL，在html中依旧可以检测到css,js
    {% load static %}
    <link rel="stylesheet" href="{% static 'bootstrap/css/bootstrap.min.css' %}">
    <script src="{% static 'bootstrap/js/bootstrap.min.js' %}"></script>
```





## diango orm简介

```python
orm对象关系映射
        
        类                   数据库的表
        
        对象                  表的记录
        
        对象获取属性          记录的某个字段对应的值
```

<h3>优点：</h3>能够让一个不会数据库操作的人 也能够简单快捷去使用数据库
<h3>缺点:</h3>由于封装程度太高 可能会导致程序的执行效率偏低
有时候 结合项目需求 可能需要你手写sql语句



<h2>注意事项：</h2>
1.django的orm不会自动帮你创建库，库需要你自己手动创建
            表会自动帮你创建  你只需要书写符合django orm语法的代码即可

```python
去应用下所在的models.py中书写类
    
    from django.db import models

    # Create your models here.
    class Userinfo(models.Model):
        # 设置id字段为userinfo表的主键  id int primary key auto_increment
        id = models.AutoField(primary_key=True)  # 在django中 你可以不指定主键字段 django orm会自动给你当前表新建一个名为id的主键字段
        # 设置username字段  username varchar(64)  CharField必须要指定max_length参数
        username = models.CharField(max_length=64)  # 在django orm中 没有char字段  但是django 暴露给用户 可以自定义char字段
        # 设置password字段  password int
        password = models.IntegerField()
    
```

### orm中最重要的俩条命令（数据库迁移(同步)命令）

1. 当你第一次执行上面两条命令的时候 django会自动创建很多张表  这些表都是django默认需要用到的表
2. 你自己写的模型类所对应的表 表名有固定格式：应用名_表名

<h4>方式1：命令行输入</h4>
```python
******************************数据库迁移(同步)命令***********************************   

    
    python manage.py makemigrations  # 不会创建表 仅仅是生成一个记录  将你当前的操作记录到一个小本本上（migrations文件夹）
    python manage.py migrate  # 将你的orm语句真正的迁移到(同步)到数据库中
    只要你在models.py中修改了跟数据库相关的代码  你就必须重新开始执行上面两条命令
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191022135649942-122185915.png)

<h4>方式二：快捷输入</h4>
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191022134057676-1703095037.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191022135352312-1462791032.png)

## form表单的应用

1. form表单 action参数可以写的形式
       1.不写 默认往当前地址提交
       2.写后缀 /index  朝着本网站的index路径提交数据
       3.写全路径  http://www.xiaohuar.com
2. form表单默认朝后端提交的方式 默认是get请求，你可以通过method参数修改提交方式，前端获取用户输入的信息 会被存放在input/option/...标签的value属性中
3. get请求携带参数的方式 是在url后面？
       如：url?username=admin&password=213213213213213
       缺点
           1.不安全
           2.get请求携带的参数有大小限制(最大不能超过4KB左右)
4. 前期你如果要提交post请求 你就去settings.py文件注释掉一个中间件

```python
前期你如果要提交post请求 你就去settings.py文件注释掉一个中间件
    MIDDLEWARE = [
            'django.middleware.security.SecurityMiddleware',
            'django.contrib.sessions.middleware.SessionMiddleware',
            'django.middleware.common.CommonMiddleware',
            # 'django.middleware.csrf.CsrfViewMiddleware',
            'django.contrib.auth.middleware.AuthenticationMiddleware',
            'django.contrib.messages.middleware.MessageMiddleware',
            'django.middleware.clickjacking.XFrameOptionsMiddleware',
        ]
```



## request对象以及方法

1. request.method:获取前端的请求方式  并且是大写的字符串形式
2. 请求方式
       get
       post
       两者都能够携带数据，但是get请求携带的数据是直接拼接在url后面 不安全并且数据大小有限制

```python
前后端数据交互
        如何获取请求方式
        获取post请求携带的数据
            request.POST
        获取get请求携带的数据
            request.GET
        get和post在后端获取用户数据的时候 规律是一样的
        request.GET  # 你就把它当成一个大字典 里面放的是get请求携带过来的数据
        request.POST  # 你就把它当成一个大字典 里面放的是post请求携带过来的数据
        """上面的大字典 所有的value都是一个列表"""
        request.GET.get('key')  # 默认取的是列表的最后一个元素 并不是直接将列表取出
        request.GET.getlist('key')  # 直接将value的列表取出
        request.POST.get('key')  # 默认取的是列表的最后一个元素 并不是直接将列表取出
        request.POST.getlist('key')  # 直接将value的列表取出
        如：
        <QueryDict: {'username': ['admin', 'tank'], 'password': ['123']}>
        tank <class 'str'>
        123 <class 'str'>
        request.POST.get('username') 默认只取列列表的最后一个元素
        如果你想将列表完整的取出 你必须用getlist()
```

## 数据的处理

### 创建表(多种关系)

```python
from django.db import models

# Create your models here.
class Book(models.Model):
    title = models.CharField(max_length=32)
    # 总共八位 小数占两位
    price = models.DecimalField(max_digits=8,decimal_places=2)
    # 书和出版社是一对多的关系  外键字段键在多的一方
    publish_id = models.ForeignKey(to='Publish')  # to指定跟谁是外键关联的  默认关联的是表的主键字段
    """
    ForeignKey字段  django orm在创建表的时候 会自动给该字段添加_id后缀
    """
    # 书和作者是多对多的关系  外键字段建在任何一方都可以  但是 推荐你建在查询频率比较高的一方
    authors = models.ManyToManyField(to='Author')
    """authors字段仅仅是一个虚拟字段 不会再表中展示出来  仅仅是用来告诉django orm 书籍表和作者表示多对多的关系
        自动创建第三张表 
    """
class Publish(models.Model):
    name = models.CharField(max_length=32)
    addr = models.CharField(max_length=255)


class Author(models.Model):
    name = models.CharField(max_length=32)
    phone = models.BigIntegerField()
    # 一对一字段 建在哪张表都可以  但是推荐你建在 查询频率比较高的那张表
    author_detail = models.OneToOneField(to='AuthorDetail')
    """
        OneToOneField字段  django orm在创建表的时候 会自动给该字段添加_id后缀

        """

class AuthorDetail(models.Model):
    addr = models.CharField(max_length=255)
    age = models.IntegerField()

```



### 数据操作

​            表字段的增删改查
​                新增的字段
​                    1.直接提供默认值 default
​                    2.设置改字段可以为空 null=True
​                注意的是 不要轻易的注释models.py中任何跟数据库相关的代码
​                主要是跟数据库相关的代码 你在处理的时候一定要小心谨慎

```python
class Userinfo(models.Model):
    # 设置id字段为userinfo表的主键  id int primary key auto_increment
    id = models.AutoField(primary_key=True)  # 在django中 你可以不指定主键字段 django orm会自动给你当前表新建一个名为id的主键字段
    # 设置username字段  username varchar(64)  CharField必须要指i定max_length参数
    username = models.CharField(max_length=32)  # 在django orm中 没有char字段  但是django 暴露给用户 可以自定义char字段
    # 设置password字段  password int
    password = models.IntegerField()
    # phone = models.BigIntegerField(default=110)  # 新增的字段 可以提前设置默认值
    # addr = models.CharField(max_length=64,null=True)  # 新增的字段 可以设置为空


    def __str__(self):
        return '我是用户对象:%s'%self.username

```

### 数据的查

1. get() 

   1. 条件存在的情况下 获取的直接是数据对象本身

2. 条件不存在的情况下 会直接报错 。所以不推荐你使用get方法查询数据

3. filter()

   1. 条件存在的情况下 获取到的是一个可以看成列表的数据 列表里面放的才是一个个数据对象本身
   2. 条件不存在的情况下  并不会报错 返回的是一个可以看成空列表的数据
   3. filter括号内可以写多个参数逗号隔开  这多个参数在查询的时候 是and关系
   4. filter的结果支持索引取值  但是不支持负数  并且不推荐你使用索引  推荐你使用它封装好的方法 first取第一个数据对象

   ```python
   def login(request):
       if request.method == 'POST':
           username = request.POST.get("username")
           password = request.POST.get("password")
           # 先以用户名为依据查询数据
           # 1.get()  当查询条件不存在的时候 会直接报错   如果存在会直接给你返回 数据对象本身        不推荐使用
           # res = models.Userinfo.objects.get(username=username)  # select id,username,password from userinfo where username='jason'
           # print(res)
           # print(res.username)
           # print(res.password)
           # 2.filter()   当查询条件不存在的时候  不会报错而是返回一个空
           # 当条件存在的情况下 无论数据有几条返回的都是列表套对象的数据格式
           # filter可以当多个查询条件 并且是and关系
           res = models.Userinfo.objects.filter(username=username)  # select * from userinfo where username='jason' and password=123;
           # user_obj = res[0]
           # user_obj = res[0:3]
           # user_obj = res[-1]  # 你可以将filter查询出来的结果当做列表去对待 支持正数的索引取值和切片 不支持负数
           user_obj = res.first()  # 取queryset第一个元素
           print(user_obj)
       return render(request,'login.html')
   
   ```

   

   ### 数据的增

   1. create()

      1. 括号内些关键字参数的形式 创建数据

      2. 该方法会有一个返回值 返回值就是当前对象本身

         

   2. 利用对象点方法的方式
          user_obj = User(username='jason')
          user_obj.save()  # 将当前对象保存到数据库中

```python
def reg(request):
    if request.method == 'POST':
        username = request.POST.get("username")
        password = request.POST.get("password")
        # 直接将用户名和密码写入数据库
        # 方式1
        # user_obj = models.Userinfo.objects.create(username=username,password=password)
        # insert into userinfo(username,password) values('admin','666');
        # create方法会有一个返回值  返回值就是当前被创建的数据对象
        # 方式2
        user_obj = models.Userinfo(username=username,password=password)
        user_obj.save()
        
        print(user_obj)
    return render(request,'register.html')
```



### 数据的修改

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>

</head>
<body>
<div class="container-fluid">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <h2 class="text-center">编辑页面</h2>
            <form action="" method="post">
                username:<input type="text" class="form-control" name="username" value="{{ edit_obj.username }}">

                password:<input type="text" class="form-control" name="password" value="{{ edit_obj.password }}">

                <br>
                <input type="submit" class="btn btn-warning">
            </form>

        </div>
    </div>
</div>
</body>
</html>
```



```python
def edit_user(request):
    # 1.如何获取用户想要编辑的数据
    edit_id = request.GET.get('edit_id')
    if request.method == 'POST':
        # 将用户新修改的所有的数据
        username = request.POST.get("username")
        password = request.POST.get("password")
        """POST中也是可以获取GET请求携带的参数"""
        # 去数据库中修改对应的数据
        # 方式1：
        models.Userinfo.objects.filter(pk=edit_id).update(username=username,password=password)  # 批量更新
        # 方式2： 获取当前数据对象 然后利用对象点属性的方式 先修改数据  然后调用对象方法保存
        # 不推荐你使用第二种方式  效率低   挨个重新写入一遍
        # edit_obj = models.Userinfo.objects.filter(pk=edit_id).first()  # pk能够自动帮你查询出当前表的主键字段名
        # edit_obj.username = username
        # edit_obj.password = password
        # edit_obj.save()
        """update方法会将filter查询出来的queryset对象中所有的数据对象全部更新"""
        # 跳转到数据展示页面
        return redirect('/userlist')
    # 2.根据主键值去数据库中查询出当前对象 展示给用户看
    edit_obj = models.Userinfo.objects.filter(pk=edit_id).first()  # pk能够自动帮你查询出当前表的主键字段名
    # 3.将查询出来的数据对象传递给前端页面 展示给用户看
    return render(request,'edit_user.html',locals())
```



### 数据的删除

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    {% load static %}
    <link rel="stylesheet" href="{% static 'bootstrap/css/bootstrap.min.css' %}">
    <script src="{% static 'bootstrap/js/bootstrap.min.js' %}"></script>
</head>
<body>
<div class="container-fluid">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <h1 class="text-center">数据展示</h1>
            <table class="table table-hover table-striped table-bordered">
                <thead>
                    <tr >
                        <th>主键值</th>
                        <th>用户名</th>
                        <th>密码</th>
                        <th class="text-center">操作</th>
                    </tr>
                </thead>
                <tbody>
                    {% for user_obj in user_queryset %}
                        <tr>
                            <td>{{ user_obj.id }}</td>
                            <td>{{ user_obj.username }}</td>
                            <td>{{ user_obj.password }}</td>
                            <td class="text-center">
                                <a href="/edit_user/?edit_id={{ user_obj.id }}" class="btn btn-primary btn-sm">编辑</a>
                                <a href="/delete_user/?delete_id={{ user_obj.pk }}" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                    {% endfor %}
                </tbody>
            </table>
        </div>
    </div>
</div>
</body>
</html>
```



```python
def delete_user(request):
    # 获取想要删除的数据id 直接删除
    delete_id = request.GET.get('delete_id')
    models.Userinfo.objects.filter(pk=delete_id).delete()  # 批量删除
    return redirect('/userlist')
```



