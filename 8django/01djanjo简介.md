[TOC]







在我们的认知中python被称作调包侠，你知道这个嘛，就是因为在python中基本所有的事情都可以调用第三方写好的模块。

## 软件开发架构

首先说一下软件开发架构：

c/s 客户端	服务端

b/s 浏览器端	服务端

ps:b/s本质上也是c/s



## http协议

http是超文本传输协议

1. http有四大特性：
   * 基于TCP/IP之上作用于应用层。
   * 基于请求响应
   * 无状态: 不保存状态，就是不保存记录，可以多次访问，一直访问，(cookie,session,token)
   * 无连接：请求一次响应一次，然后断开(如果使用长连接，使用websocket(HTTP协议的大补丁))
   
2. 数据格式
   1. 数据格式之请求格式
      * 请求首行(请求方式，协议版本。。。)
      * 请求头(一大堆k:v键值对)
      * \r\n(空行，或者说换行)
      * 请求体:向服务器发送post请求的时候，请求体才会有数据，如果是get请求是没有请求体的。
   2. 数据格式之响应格式
      * 响应首行
      * 响应头
      * \r\n
      * 响应体
   
3. 请求方式

   * get请求：向一方(服务器)要数据。
   * post请求：向一方(服务器)提交数据（eg>用户登陆）

4. 响应状态码
   * 1xx	服务端已经接收到 你发送的数据，正在处理，你可以继续提交数据，informational(信息状态码)，接收的请求正在处理。
   * 2xx	请求成功。sunccess(成功状态码)请求正常处理完毕
   * 3xx	重定向，redirection(重定向状态码)，需要进行附加操作完成请求
   * 4xx	请求错误(404:请求资源不存在，403:拒绝访问),client Error(客户端错误状态码)，服务器无法处理请求
   * 5xx	服务器内部错误(500) server error(服务器错误状态码)，服务器处理请求出错。



5. **url：统一资源定位符**

## 手撸web框架

1. 手动书写socket
2. 手动处理http格式数据

```python
#Author:SkyOcean
# @Time     :2019/10/1821:31
# @Author   :SkyOcean
# @Site     :
# @File     :手撸.py
# @Software :PyCharm
import socket
server = socket.socket()
server.bind(('127.0.0.1',8080))
server.listen(5)
while True:
    conn,add = server.accept()
    data = conn.recv(1024)
    # print(data)
    conn.send(b'HTTP/1.1 200 ok\r\n\r\n')
    data=data.decode("utf-8")
    current_path = data.split("\r\n")[0].split(' ')[1]
    if current_path=='/index':
        conn.send(b'index')
    elif current_path=='/login':
        conn.send(b'login')
    else:
        conn.send(b'404 errror')


```



## 基于wsgiref模块

该模块实现了上面俩个手动的过程

```python
from wsgiref.simple_server import make_server
from urls import urls
from views import *




def run(env,response):
    """
    :param env: 请求相关的所有数据
    :param response: 响应相关的所有数据
    :return:
    """
    response('200 OK',[])
    # print(env)
    current_path = env.get('PATH_INFO')
    # if current_path == '/index':
    #     # 很多业务逻辑代码
    #     return [b'index']
    # elif current_path == '/login':
    #     return [b'login']
    # else:
    #     return [b'404 error']
    # 先定义一个变量名 用来存储后续匹配到的函数名
    func = None
    # for循环 匹配后缀
    for url in urls:
        if current_path == url[0]:
            func = url[1]  # 一旦匹配成功 就将匹配到的函数名赋值给func变量
            break  # 主动结束匹配
    # 判断func是否有值
    if func:
        res = func(env)
    else:
        res = error(env)
    return [res.encode('utf-8')]




if __name__ == '__main__':
    server = make_server('127.0.0.1',8080,run)
    # 实时监听该地址  只要有客户端来连接 统一交给run函数去处理
    server.serve_forever()  # 启动服务端
```



根据功能不同拆分成了不同的py文件

1. urls.py	路由与视图函数对象关系

   ```python
   from views import *
   
   urls = [
       ('/index',index),
       ('/login',login),
       ('/xxx',xxx),
       ('/get_time',get_time),
       ('/get_user',get_user),
       ('/get_db',get_db),
   ]
   ```

   

2. views.py	放的是视图函数（函数，类）(处理业务逻辑的)

   ```python
   def index(env):
       return 'index'
   
   
   def login(env):
       return 'login'
   
   def error(env):
       return '404 error'
   
   
   def xxx(env):
       return 'xxx'
   
   from datetime import datetime
   
   
   def get_time(env):
       current_time = datetime.now().strftime('%Y-%m-%d %X')
       with open(r'D:\上海Python11期视频\python11期视频\day56\templates\get_time.html','r',encoding='utf-8') as f:
           data = f.read()
       data = data.replace('$$time$$',current_time)
       return data
   
   from jinja2 import Template
   
   def get_user(env):
       d = {'name':'jason','pwd':'123','hobby':['read','running','music']}
       with open(r'D:\上海Python11期视频\python11期视频\day56\templates\get_user.html','r',encoding='utf-8') as f:
           data = f.read()
       temp = Template(data)
       res = temp.render(user=d)  # 将字典d传递给前端页面 页面上通过变量名user就能够获取到该字典
       return res
   
   import pymysql
   
   def get_db(env):
       conn = pymysql.connect(
           host = '127.0.0.1',
           port = 3306,
           user = 'root',
           password = 'root',
           database = 'day56',
           charset = 'utf8',
           autocommit = True
       )
       cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
       sql = "select * from userinfo"
       cursor.execute(sql)
       res = cursor.fetchall()
       print(res)
       with open(r'D:\上海Python11期视频\python11期视频\day56\templates\get_db.html','r',encoding='utf-8') as f:
           data = f.read()
       temp = Template(data)
       ret = temp.render(user_list = res)  # 将字典d传递给前端页面 页面上通过变量名user就能够获取到该字典
       return ret
   ```

   

3. templates	模板文件夹(用来存放一堆html文件)

   ```html
   <!--get_time.html-->
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <titget_timele>Title</titget_timele>
       <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
       <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
       <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
   </head>
   <body>
   $$time$$
   </body>
   </html>
   
   ```

   ```html
   <!--get_user.html-->
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
   <p>{{ user }}</p>
   <p>{{ user.name }}</p>
   <p>{{ user['pwd'] }}</p>
   <p>{{ user.get('hobby') }}</p>
   </body>
   </html>
   ```

   ```html
   <!--get_db.html-->
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
   <div class="container">
       <div class="row">
           <div class="col-md-8 col-md-offset-2">
               <h1 class="text-center">用户列表</h1>
               <table class="table table-bordered table-striped table-hover">
                   <thead>
                       <tr>
                           <th>id</th>
                           <th>name</th>
                           <th>pwd</th>
                       </tr>
                   </thead>
                   <tbody>
                       {% for user_dict in user_list %}
                           <tr>
                               <td>{{ user_dict.id }}</td>
                               <td>{{ user_dict.name }}</td>
                               <td>{{ user_dict.pwd }}</td>
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

   

拆分完成后，添加功能只需要在urls.py和views.py方面修改就行



## 动静态网页

 1. 静态网页

    数据是写死的 万年不变

 2. 动态网页
    		数据是实时获取的
        		eg:
        			1.后端获取当前时间展示到前端
        			2.后端获取数据库中的数据展示到前端

疑问：
	如何将后端获取的数据 传递给html页面
	
	后端获取的数据 传递给html页面  >>>:  模板的渲染
	
	jinja2  
	pip3 install jinja2


在前端字典的使用

```
模板语法(极其贴近pythaon后端语法)
	<p>{{ user }}</p>
	<p>{{ user.name }}</p>
	<p>{{ user['pwd'] }}</p>
	<p>{{ user.get('hobby') }}</p>
```

在前端数据库的使用

	模板语法(极其贴近pythaon后端语法)
	{% for user_dict in user_list %}
			<tr>
				<td>{{ user_dict.id }}</td>
				<td>{{ user_dict.name }}</td>
				<td>{{ user_dict.pwd }}</td>
			</tr>
		{% endfor %}


## python三大主流web框架

1. Django:
   * 大而全，自身的功能特别多，类似航空母舰
   * 有时候过于笨重
2. Flask
   * 小而精 自带的功能特别特别少，类似游骑兵
   * 支持的第三方的模块特别多，如果将flask第三方模块全部加起来，完全可以操过django
   * 过于依赖第三方模块，当第三方的模块不能使用的时候（源码不再开放，存在bug作者不修改，作者不再维护等），flask就会瘫痪。
3. Tornado
   * 异步非阻塞
   * 特别厉害，牛逼到可以开发游戏服务器



他们涉及到三个领域

A:socket部分

B:路由和视图函数对应关系

C:模板语言



Django:

​	A：用别人的  wsgiref模块

​	B:	自己写的

​	C：自己写的

Flask:

​	A:用别人写的	werkzeug模块(基于wsgiref模块)

​	B：自己写的

​	C：用别人的 jinja2

Tornado

​	三个都是自己写的



## Django的使用

### 注意事项

1. 计算机中的名称中不能有中文
2. 一个pycharm窗口就是一个项目
3. 项目名称里面尽量不要中文

### django版本的问题

1.X 版本  2.X版本  现在市面上用的比较多的还是1.X

推荐你使用1.11.9~1.11.13

下表中LTS 表示还在维护的版本

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191018204033008-785141484.png)



### Django安装

直接安装最新版本：pip3 install django

选择版本：pip3 install django==1.11.11

验证django是否安装成功：

​	命令行输入diango-admin

### app

一个django项目就类似于是一所大学，而app就类似于大学里面的学院

django其实就是用来一个个应用的

一个app就相当于一块独立的功能,比如：用户功能   管理功能

django支持任意多个app



## 如何使用django

### 命令行使用

1. 首先切换到要创建的文件下，然后创建django项目：`django-admin startproject 项目名`，如`django-admin startproject mysite`

2. 启动django项目：`python manage.py runserver IP和端口号`，如：`python manage.py runserver 127.0.0.1:8080`，不输入ip和端口号默认开启本机，如`python manage.py runserver`

3. 创建应用app:`python manage.py startapp app名`，如`python manage.py startapp app01`

   ![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191019151008294-635199357.png)

   1. 注意：
      
      * 新创建的app需要你去settings配置文件中进行注册，如果是pycharm只会帮助你注册第一个在你创建项目的时候写的应用，创建第二的app 的时候需要自己进行配置。
      
        ![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191018212513858-1941343691.png)

4. 注意：

   1. 使用命令行创建d'jango项目的时候，不会自动帮助你创建templates文件夹。只能自己手动创建。

   2. settings文件中，需要你手动在TEMPLATES中写配置文件:`os.path.join(BASE_DIR,'templates')`

     ![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191018212623898-706612617.png)

5. 命令行关闭直接退出

### pycharm使用

创建django项目

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191018212226915-514661697.png)

在启动django项目的时候 你一定要确保一个端口只有一个django项目

### 目录介绍

```
项目名(mysite)
    (mysite)跟项目名同名的文件夹：项目目录
    	__init__.py
        settings.py  暴露给用户的配置文件
        urls.py  路由与视图函数对应关系
        wsgi.py		runserver命令就使用wsgiref模块做简单的web server
        
    应用名(app)
        migrations文件夹  存放数据库迁移记录的
        admin.py  django后台管理
        apps.py  注册相关
        models.py  模型类 
        tests.py  测试文件
        views.py  存放视图函数
    templates文件夹  存放html文件
    manage.py  django入口文件,管理文件


```




​     

## diango连接数据库以及配置

1. django默认自带的一个小型的sqlite数据库   该数据库功能不是很强大 尤其是对日期格式的数据 不是很兼容
2. django默认使用的是mysqldb模块连接数据库  但是该模块不兼容 不推荐使用
3. 使用mysql进行如下配置

```python
第一步配置文件中配置
                DATABASES = {
                    'default': {
                        'ENGINE': 'django.db.backends.mysql',  # 指定数据库 MySQL postgreSQL
                        'NAME': 'day56',  # 到底使用哪个库
                        'USER':'root',
                        'PASSWORD':'root',
                        'HOST':'127.0.0.1', 
                        'PORT':3306,
                        'CHARSET':'utf8'#编码
                    }
                }
    
            第二步 
                django默认使用的是mysqldb连接数据库  但是该模块不支持了
                所以你要告诉django不要用mysqldb该用pymysql连接
                
                你可以在项目名下面的__init__.py也可以在应用名下面的__init__.py文件中指定
                import pymysql
                pymysql.install_as_MySQLdb()
```





## pycharm连接数据库MySQL（pycharm充当数据库的客户端）

### pycharm中找数据库的三种方式

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021133255022-1346257289.png)

<h3>下载database插件</h3>
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021133540512-1725415823.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021133632140-1930851986.png)



### 时区错误

Server returns invalid timezone. Go to 'Advanced' tab and set 'serverTimezone' property manually.

如果出现时区错误的话，有俩种不过方法

<h1>方法1</h1>
jdbc:mysql://localhost:3306?serverTimezone=GMT



在URL的后面添加?serverTimezone=GMT，但是比较麻烦，每次都要进行配置时区，建议采用方法二

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021134239129-1254907403.png)

------

<h1>方法2配置时区</h1>
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021141525376-1739718731.png)

### 连接数据库成功和会出现的问题

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021143025499-1763421936.png)



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021143136316-1025027368.png)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191021140017798-1220565488.png)



# pycharm连接数据库sqlite

一共两步

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191028161417871-621638020.png)
![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191028161503246-1791382452.png)

