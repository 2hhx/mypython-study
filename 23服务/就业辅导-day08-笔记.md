## 1 CGI, FastCGI, WSGI, uWSGI, uwsgi

```python
from wsgiref.simple_server import make_server

def mya(environ, start_response):
    print(environ)
    start_response('200 OK', [('Content-Type', 'text/html')])
    if environ.get('PATH_INFO') == '/index':
        with open('index.html','rb') as f:
            data=f.read()

    elif environ.get('PATH_INFO') == '/login':
        with open('login.html', 'rb') as f:
            data = f.read()
    else:
        data=b'<h1>Hello, web!</h1>'
    return [data]

if __name__ == '__main__':
    myserver = make_server('', 8011, mya)
    print('监听8010')
    myserver.serve_forever()

wsgiref简单应用
```



## 2 项目上线

```python

django：前后端混合开发，dtl（模板语言）
项目上线：
-linux安装环境
-项目传到liunux上（git clone 下来）
-python manage.py collectstatic
-数据库迁移（两条命令）
-uWSGI启动django项目（socket，http）
-nginx转发到uWSGI
-nginx做动静分离


#nginx配置：
#动态请求
location / {
          proxy_pass http://101.133.225.166:8088   # 动态请求转到uWSGI中
    
        } 
#静态请求
location /static/ {
        /root/opt/lqz;   #静态请求，直接去目录下拿  /root/opt/lqz 文件夹
    }
  location /media/ {
        /root/opt/lqz;
    }
  
#uWSGI配置
[uwsgi]
#配置和nginx连接的socket连接
socket=0.0.0.0:8080
#也可以使用http
#http=0.0.0.0:8080
#配置项目路径，项目的所在目录
chdir=/home/untitled3
#配置wsgi接口模块文件路径
wsgi-file=untitled3/wsgi.py
#配置启动的进程数
processes=4
#配置每个进程的线程数
threads=2
#配置启动管理主进程
master=True
#配置存放主进程的进程号文件
pidfile=uwsgi.pid
#配置dump日志记录
daemonize=uwsgi.log
```

## 3 补充

```python
www.lqz.top/media/a.png
浏览器----》wsgiref---》django路由（meidia）--》视图（media文件夹na）---》返回了

-图片放在自己服务器情况
	浏览器----》nginx----》media文件夹拿---》返回了
-图片放在七牛云
	浏览器---》别人的ngixn上---》返回


media的图片，保存在哪？程序控制的，项目路径的media，放在其他路径可以吗？   保存在哪？
后期做项目：图片，视频，不保存在自己服务器上，阿里云oss，文件服务器，七牛云

python manage.py collectstatic  ：收集静态文件   把static和media文件夹下的东西，统一收集到一个目录下
```



