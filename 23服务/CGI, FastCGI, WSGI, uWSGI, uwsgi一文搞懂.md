[TOC]

# 网关接口

# CGI, FastCGI, WSGI, uWSGI, uwsgi一文搞懂



## 一 CGI

```python
# 1、通用网关接口（Common Gateway Interface/CGI）是一种重要的互联网技术，可以让一个客户端，从网页浏览器向执行在网络服务器上的程序请求数据。CGI描述了服务器和请求处理程序之间传输数据的一种标准。

# 2、CGI程序可以用任何脚本语言或者是完全独立编程语言实现，只要这个语言可以在这个系统上运行。

# 3、用来规范web服务器传输到php解释器中的数据类型以及数据格式，包括URL、查询字符串、POST数据、HTTP header等，也就是为了保证web server传递过来的数据是标准格式的

# 4、一句话总结： 一个标准，定义了客户端服务器之间如何传数据
```



## 二 FastCGI

```python
# 1、快速通用网关接口（Fast Common Gateway Interface／FastCGI）是一种让交互程序与Web服务器通信的协议。FastCGI是早期通用网关接口（CGI）的增强版本。

# 2、FastCGI致力于减少网页服务器与CGI程序之间互动的开销，从而使服务器可以同时处理更多的网页请求。

# 3、使用FastCGI的服务器：
    Apache HTTP Server (部分)
    Cherokee HTTP Server
    Hiawatha Webserver
    Lighttpd
    Nginx
    LiteSpeed Web Server
    Microsoft IIS
# 4、一句话总结： CGI的升级版

```

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcy7m2g0j9j310q0gqjtv.jpg" alt="image-20200318002154734" style="zoom:50%;" />

## 三 WSGI

```python
# 1、Web服务器网关接口（Python Web Server Gateway Interface，缩写为WSGI）是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口。自从WSGI被开发出来以后，许多其它语言中也出现了类似接口。

# 2、wsgi server (比如uWSGI） 要和 wsgi application（比如django ）交互，uwsgi需要将过来的请求转给django 处理，那么uWSGI 和 django的交互和调用就需要一个统一的规范，这个规范就是WSGI WSGI（Web Server Gateway Interface）

# 3、WSGI，全称 Web Server Gateway Interface，或者 Python Web Server Gateway Interface ，是为 Python 语言定义的 Web 服务器和 Web 应用程序或框架之间的一种简单而通用的接口。自从 WSGI 被开发出来以后，许多其它语言中也出现了类似接口。

# 4、WSGI 的官方定义是，the Python Web Server Gateway Interface。从名字就可以看出来，这东西是一个Gateway，也就是网关。网关的作用就是在协议之间进行转换。

# 5、WSGI 是作为 Web 服务器与 Web 应用程序或应用框架之间的一种低级别的接口，以提升可移植 Web 应用开发的共同点。WSGI 是基于现存的 CGI 标准而设计的

# 6、一句话总结： 为Python定义的web服务器和web框架之间的接口标准
```



## 四 uWSGI

```python
wsgiref，werkzeug（一个是符合wsgi协议的web服务器+工具包（封装了一些东西））
uWSGI 用c语言写的，性能比较高
gunicorn：python写的
tornado：也可以部署django项目
# 1、它是一个Web服务器（类似的有wsgiref，gunicorn），它实现了WSGI协议、uwsgi、http等协议。用于接收前端服务器转发的动态请求并处理后发给 web 应用程序。

# 2、Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换

# 3、一句话总结： 一个Web Server，即一个实现了WSGI的服务器，大体和Apache是一个类型的东西，处理发来的请求。
```

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcy7ly6362j31060e244f.jpg" alt="image-20200318000134358" style="zoom:50%;" />

## 五 uwsgi

```python


location / {
          #方式一
          #include uwsgi_params; # 导入一个Nginx模块他是用来和uWSGI进行通讯的
         	#uwsgi_connect_timeout 30; # 设置连接uWSGI超时时间
          #uwsgi_pass 101.133.225.166:8080;
          #方式二
          #include uwsgi_params; # 导入一个Nginx模块他是用来和uWSGI进行通讯的
          #uwsgi_pass unix:///var/www/script/uwsgi.sock; # 指定uwsgi的sock文件所有动态请求
          #方式三
          proxy_pass http://101.133.225.166:8088
    
        }  
```



```python
# 1、它是uWSGI服务器实现的独有的协议，用于定义传输信息的类型，是用于前端服务器与 uwsgi 的通信规范。

# 1、一句话总结： uWSGI自有的一个协议
uWSGI：web服务器，等同于wsgiref
uwsgi:uWSGI自有的协议

```

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcy7lt147ij31gu0ekqbq.jpg" alt="image-20200318000713341" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/00831rSTly1gcy7ln056dj30u0151jwj.jpg" alt="image-20200318001134520" style="zoom:50%;" />

## 五 ASGI

1.支持异步

2.websocket，与http2



ASGI(Asynchronous Server Gateway Interface, 异步服务器网关接口) 是WSGI的传人，为了规范支持异步的Python网络服务器，框架和应用之间的通信而定制。
相较于WSGI定义了同步的Python应用间的通信规范，ASGI同时囊括了同步和异步应用的通信规范，并且向后兼容遵循WSGI的应用、服务以及框架。

## 介绍

ASGI是WSGI的传承者，是python网络服务器、框架和应用进行长连接的通信标准。
WSGI在python网络应用的自由与创新方面很成功，而ASGI希望在此基础上推进异步Python的应用。

## 为什么WSGI不合适

你可能会问“为什么选择不更新WSGI”？这一问题在过去多年被时常问起，原因通常被归结为WSGI的单次调用接口不适合WebSocket这种介入程度更高的协议。
WSGI应用都是单词、同步调用的，他们仅在接受一个请求后返回应答，这种模式不支持长连接，比如HTTP长轮询或者WebSocket连接。
即使我们将其改为异步调用，发起请求的方式仍然是单一的，所以那些会多次传入事件的协议无法调用这些应用。

## ASGI工作原理

ASGI由单独的异步应用沟通成。其中包括`scope`，包含传入请求的所有信息；`send`，用于向客户端发送事件的异步方法，`receive`，用于接受客户端发来事件的异步方法。
ASGI不仅让应用可以多次接受或发送事件，并且可以结合协程时应用同时处理器他任务（比如监听外部触发的事件，就像Redis队列一样）。
一个最简单的应用就像函数一样：

```
async def application(scope, receive, send):
    event = await receive()
    ...
    await send({"type": "websocket.send", ...})
1234
```

每一个事件都是按预定的格式编排的python字典，这种格式构成了通信规范的基础，并时的应用可以在服务器之间交换信息。
每个事件都一个`type`属性，用于表明事件的结构。比如从`receive`方法可能收到如下结构的事件，用于发送HTTP响应的开头：

```
{
    "type": "http.response.start",
    "status": 200,
    "headers": [(b"X-Header", b"Amazing Value")],
}
12345
```

你也可能将如下结构的事件传递给`send`方法用于发送WebSocket信息：

```
{
    "type": "websocket.send",
    "text": "Hello world!",
}
1234
```

## 兼容WSGI

ASGI被设计为WSGI的超集，定义了明确的方法用于两者之间的转换，通过转换装饰器（在asgiref库中提供）可以让WSGI应用在ASGI服务器内运行。在异步事件循环外会有一个线程池用于运行同步WSGI应用。

# WSGI, UWSGI和ASGI

## 首先是介绍什么是WSGI， 接着是什么是UWSGI， 接着是ASGI

​        首先需要介绍的是CGI， CGI全称(Common Gateway Interface, 通用网关接口),定义的是客户端与Web服务器交流方式的一个程序.例如正常情况下客户端发送来一个请求,CGI根据HTTP协议的将请求内容进行解析, 经过计算以后会将计算出来的内容封装好,比如服务器返回一个html页面,并且根据http协议构建返回的内容格式,涉及到的tcp连接、http原始请求和相应的格式这些， 都是由一个软件来完成，完成以上的工作需要一个程序来完成， 便是CGI。

　　关于WSGI, 全称**Web服务器网关接口(Python Web Server Gateway Interface, WSGI),**是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口..简单来说就是**用来处理Web服务端与客户端的通信问题的**,(以django框架为例,使用的是wsgiref模块,该模块的功能)

```
以django框架为例,使用的是wsgiref模块,该模块的功能是:
    监听8000端口,把http请求根据WSGI协议将其转换到applcation中的environ参数, 然后调用application函数.
    wsgiref会把application函数提供的响应头设置转换为http协议的响应头,把application的返回(return)作为响应体,根据http协议,生成响应,返回给浏览器.
```

而**UWSG**I是一个Web服务器, 实现了WSGI协议,uwsgi,http等协议,

uwsgi是一个二进制协议, 能够携带任何类型的信息,uwsgi数据包的前4个字节用于面描述信息的类型,该协议主要工作在tcp方式下,**uwsgi是一种线路协议而不是通信协议,**因此常用于在uWSGI服务器与其他网络服务器的数据通信.

uwsgi 协议是一个 uWSGI服务器自有的协议,用于定义传输信息的类型

**Nginx**中HttpUwsgiModule的作用是与uWSGI服务器进行交换。WSGI是一种Web服务器网关接口。它是一个Web服务器（如nginx，uWSGI等服务器）与web应用（如用Flask框架写的程序）通信的一种规范。

对于管理人员来说，uWSGI服务器提供了各种配置方法：命令行、环境变量、XML、INI、YAML、JSON、SQlite3数据库和LDAP。

除此之外，它的设计完全模块化，这意味着，可以使用不同的插件以便满足不同的技术应用，从而实现兼容性.
关于**ASGI**

 是异步网关协议接口,介于网络服务和python饮用应用之间的标准接口,能够处理多种通用的协议类型,包括http,http2和websocket.

**关于WSGI和ASGI的区别是**

WSGI是基于http协议模式开发的,不支持websocket,而ASGI的诞生解决了python中的WSGI不支持当前的web开发中的一些新的协议标准,同时ASGI支持原有模式和Websocket的扩展, 即ASGI是WSGI的扩展.

**关于ASGI的应用案例, 下一篇博客我们再聊**