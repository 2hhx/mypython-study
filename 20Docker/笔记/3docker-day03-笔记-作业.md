

# docker-day03

## 昨日回顾

### 1 文件拷贝  docker cp  向里拷贝向外拷贝

### 2 目录挂载 容器启动时 -v 宿主机路径:容器路径

如果映射三个路径，应该怎么写？-v 路径:路径 -v 路径：路径 -v 路径：路径

### 3 查看容器信息，查看容器的ip ，容器和容器通信

docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名称（容器ID）

### 4 应用部署 mysql，redis

服务的配置文件在哪？怎么改？因为有目录映射，以后启动容器做好目录映射，只需要修改宿主机的配置文件，重启容器，配置就生效了

远程连接mysql，连不上？会有一个id号，拿到id号直接去搜索引擎搜索，查看原因

### 5 把容器保存为镜像，

通过镜像跑起来的容器，上面的软件是一定的，有的情况不够我们实际的使用，我们需要安装自己需要的软件，一旦安装了，容器和镜像的内容就不一样了，可以直接通过容器保存为镜像，然后把镜像备份，可以迁移到其他机器上，直接运行起容器，该有的软件，服务，都有

docker commit 容器名字 镜像名字

### 6 镜像备份

docker  save -o mynginx.tar mynginx_i   在当前路径下保存一个tar文件

### 7 恢复镜像

docker load -i mynginx.tar    在docker images 中就可以看到镜像了

### 8 私有仓库

拉取私有仓库镜像：docker pull registry

启动私有仓库：docker run -di --name=registry -p 5000:5000 registry

打开浏览器 输入地址http://192.168.1.12:5000/v2/_catalog看到`{"repositories":[]}`

修改daemon.json

{"insecure-registries":["192.168.1.12:5000"]} 

重启docker服务 systemctl restart docker

重启私服容器：docker start registry

标记镜像：docker tag django2.0 192.168.1.12:5000/django2.0

上传镜像：docker push 192.168.1.12:5000/django2.0

在从浏览器打开：http://192.168.1.12:5000/v2/_catalog，就能看到了

只要配置了私服是这个地址，就能直接拉取镜像了



## 今日内容：dockerfile，部署一个小项目

### dockerfile：由一系列命令和参数构成的脚本，通过这个文件构建镜像，dockerfile就是一个文件，占得空间非常小

### dockerfile是一个文件+一堆命令

### 命令

FROM 镜像名字:镜像标签    FROM centos:centos7   指定基于哪个镜像

MAINTAINER 作者           作者是谁   

ENV key value               环境变量

RUN 命令                    要执行的命令

ADD 文件路径            拷贝文件到镜像内（自动解压）

COPY 文件路径       拷贝文件到镜像内（不会自动解压）

WORKDIR 路径        工作目录（工作的目录，一旦启动起容器来，进入，在的路径）



### 通过dockerfile，构建一个django 1.11.9的镜像，内部装上requests模块,在home目录下创建一个test文件夹,传一个a.txt到镜像中

FROM python:3.6
MAINTAINER lqz

RUN pip install django==1.11.9

RUN mkdir /home/test

RUN pip install requests

ADD a.txt /hom

WORKDIR /home

### 在当前路径下创建一个a.txt，dockerfile的名字必须是Dockerfile

### 构建：docker build -t='镜像名字' .

docker build -t='django1.11.9' .

### 通过docker images 就可以查看构建的镜像

### 通过镜像，跑起容器

docker run -id --name=django1.11.9_my django1.11.9:latest

### 进入到容器内部

docker exec -it django1.11.9_my /bin/bash



### 案例一：通过容器部署项目

通过uwsgi部署到容器中，起一个nginx容器，做转发

### 1 先把文件传到服务器解压

### 2 启动一个容器，做目录挂载

docker run -id -p 8088:8088 -v /root/djangotest:/home --name=my_django_test python:3.6

### 3 进入容器

docker exec -it 92c /bin/bash

### 4 安装django环境

pip install django==1.11.9

### 5 运行项目

python manage.py runserver 0.0.0.0:8088    用的是wsgiref

### 6 通过uWSGI跑项目

安装uwsgi：pip install uwsgi

写入：

vim uwsgi.ini
#写入
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

启动

uwsgi --ini uwsgi.ini #启动

### 7 用nginx转发

docker pull nginx

mkdir -p nginx/conf nginx/html nginx/logs

#新建配置文件
在conf目录下新建nginx.conf

```python
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  localhost;
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
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

docker run --name mynginx -id -p 80:80 -v /root/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /root/nginx/html:/etc/nginx/html -v /root/nginx/logs:/var/log/nginx nginx

在外部浏览器访问宿主机ip地址80端口，就可以访问项目了



### 补充1

K8s:容器编排，夸服务器，多台服务器上的多个docker容器

docker-compos：单机容器编排，8个容器，同时启动，

### 补充2

scp  远程拷贝命令,从mac拷贝到了阿里云服务器的/root路径

scp untitled3.zip root@101.133.225.166:/root

解压文件：yum install zip   yum install unzip

### 补充3

### 在python3.6的容器中安装软件需要apt-get

apt-get update

apt-get install vim



### 补充4 cgi wsgi 

cgi：通用网关接口

wsgi：python的协议，Web服务网管接口，简单来说它是一种Web服务器和应用程序间的通信规范。

wsgiref，uWSGI（c语言实现的，性能比较高），gunicorn。。。：根据这个协议的实现

java、php都会有一些协议

tomcat就是python中的uwsgi

php服务器





作业：

编写一个dockerfile，实现镜像具有django1.11.9

通过docker部署nginx和项目，实现uwsgi运行项目，外网访问



























