

下午从网上找几份简历截图，发给大家，里面有好的，有不好的，

# 昨日回顾

## 1 dockerfile 的编写

### 1.1一个文件，文件名字必须叫Dockerfile，一系列命令的集合

FROM 镜像名字:镜像标签    FROM centos:centos7   指定基于哪个镜像

MAINTAINER 作者           作者是谁   

ENV key value               环境变量

RUN 命令                    要执行的命令 

ADD 文件路径            拷贝文件到镜像内（自动解压）

COPY 文件路径       拷贝文件到镜像内（不会自动解压）

WORKDIR 路径        工作目录（工作的目录，一旦启动起容器来，进入，在的路径）

EXPOSE 8080       容器对外暴露的端口

CMD 命令             一旦启动容器，会执行该条命令，这条命令，必须是夯住的命令



### 1.2构建镜像

docker build -t='镜像名字' .

### 1.3 镜像构建出来后，就可以查看，可以基于镜像运行容器

## 2 通过容器部署项目（nginx+uwsgi+django）

### 2.1 项目传到宿主机上（从git上拉下来），启动了一个python3.6的容器，并且做了目录挂载和端口映射

docker run -id -p 8088:8088 -v /root/djangotest:/home --name=my_django_test python:3.6

如果不做目录映射，需要把项目拷贝到容器内部（docker cp）

### 2.3 进入到容器内部，安装模块

docker exec 进入 

pip install django==1.11.9

pip install -r requm.txt

python manage.py runserver 0.0.0.0:8088 能跑起来，再继续

### 2.4 使用uwsgi运行项目

pip install uwsgi   （uwsgi是用c语言写的，用python做了封装）

在项目根路径创建一个uwsgi.ini的文件，写入

```python
[uwsgi]
#配置和nginx连接的socket连接
socket=0.0.0.0:8080
#也可以使用http
#http=0.0.0.0:8080
#配置项目路径，项目的所在目录，路径是容器的路径，部署宿主机的路径
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

通过uwsgi启动

uwsgi --ini uwsgi.ini

uwsgi --stop uwsgi.pid    #停止

kill -9 进程杀死

查看是否启动

ps aux |grep uwsgi

### 2.5 配置nginx转发

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

### 补充

python manage.py collectstatic   收集静态文件，把所有的静态js，css，图片收集到一个文件夹中，把这个文件夹单独放到一个地址，用nginx转发，做动静分离

192.168.1.11/static/head.png

192.168.1.11/media/head.png

# 今日内容

## 1 dockerfile构建镜像

### 1.1 创建Dockerfile，在根路径下一定要有项目untitled3.tar



#### tar -cvf untitled3.tar untitled3

#### untitled3项目内部必须有个uwsgi.ini   最后一句的daemonize去掉



注意：默认linux是解压不了zip格式的，需要安装unzip和zip

现在统一用的tar格式 ，linux就可以解压

```python
FROM python:3.6
MAINTAINER lqz
WORKDIR /home
RUN pip install django==1.11.9
RUN pip install uwsgi
EXPOSE 8081
ADD ./untitled3.tar /home/
CMD ["uwsgi", "--ini", "/home/untitled3/uwsgi.ini"] 

```

### 1.2 构建镜像

docker build -t='mydockerfile_django' .

### 1.3 查看镜像

docker images   就可以看到mydockerfile_django镜像

### 1.4 通过镜像运行容器(不需要挂载目录，项目已经在镜像内部了)

docker run -di --name=my_dockerfile_django -p 8081:8081 mydockerfile_django:latest

项目就跑起来了

## 2数据库主从搭建

### 2.1 主从同步原理

mysql主从配置的流程大体如图：

1）master会将变动记录到二进制日志里面；

2）master有一个I/O线程将二进制日志发送到slave;

3) slave有一个I/O线程把master发送的二进制写入到relay日志里面；

4）slave有一个SQL线程，按照relay日志处理slave的数据；

### 2.2 注意点

1 咱们用docker模拟了两台服务器，服务器的系统，mysql的版本必须一致

### 2.3 具体步骤,启动主库

```python

docker pull mysql:5.7
#在home目录下创建mysql文件夹，下面创建data和conf.d文件夹 
mkdir /home/mysql 
mkdir /home/mysql/conf.d 
mkdir /home/mysql/data/
# 创建my.cnf配置文件
touch /home/mysql/my.cnf
#写入

[mysqld]
user=mysql
character-set-server=utf8
default_authentication_plugin=mysql_native_password
secure_file_priv=/var/lib/mysql
expire_logs_days=7
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
max_connections=1000
##主库----start--- 同一局域网内注意要唯一
server-id=100  
## 开启二进制日志功能，可以随便取（关键）
log-bin=mysql-bin
##主库----end--- 
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8


#启动主库容器（挂载外部目录，端口映射成33307，密码设置为123456）
docker run  -di -v /home/mysql/data/:/var/lib/mysql -v /home/mysql/conf.d:/etc/mysql/conf.d -v /home/mysql/my.cnf:/etc/mysql/my.cnf -p 33307:3306 --name mysql-master -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```

### 2.4 启动从库

```python
#在home目录下创建mysql2文件夹，下面创建data和conf.d文件夹 
mkdir /home/mysql2
mkdir /home/mysql2/conf.d 
mkdir /home/mysql2/data/
#mkdir /home/mysql2 /home/mysql2/conf.d /home/mysql2/data/
# 创建my.cnf配置文件
touch /home/mysql2/my.cnf

[mysqld]
user=mysql
character-set-server=utf8
default_authentication_plugin=mysql_native_password
secure_file_priv=/var/lib/mysql
datadir=/var/lib/mysql
expire_logs_days=7
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
max_connections=1000

server-id=101
log-bin=mysql-slave-bin
relay_log=edu-mysql-relay-bin

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8


docker run  -di -v /home/mysql2/data:/var/lib/mysql -v /home/mysql2/conf.d:/etc/mysql/conf.d -v /home/mysql2/my.cnf:/etc/mysql/my.cnf -p 33306:3306 --name mysql-slave -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
         

```

### 2.5 远程连接到主库和从库

```python
#主库
mysql -h 101.133.225.166 -P 33307 -u root -p123456
##创建test用户,设置任意ip地址可以访问，密码为123
create user 'test'@'%' identified by '123';
##授权用户，把所有权限授权给test，他就相当于root了
grant all privileges on *.* to 'test'@'%' ;
###刷新权限
flush privileges;
#查看主服务器状态(显示如下图)
show master status; 



#从库
mysql -h 101.133.225.166 -P 33306 -u root -p123456
#配置详解
/*
change master to 
master_host='MySQL主服务器IP地址', 
master_user='之前在MySQL主服务器上面创建的用户名'， 
master_password='之前创建的密码', 
master_log_file='MySQL主服务器状态中的二进制文件名', 
master_log_pos='MySQL主服务器状态中的position值';
*/
#命令如下
change master to master_host='101.133.225.166',master_port=33307,master_user='test',master_password='123',master_log_file='mysql-bin.000003',master_log_pos=0;
#启用从库
start slave;
#查看从库状态（如下图）
show slave status\G;



```

### 2.6 测试

```
#在主库上创建数据库test1
create database test1;
use test1;
#创建表
create table tom (id int not null,name varchar(100)not null ,age tinyint);
#插入数据
insert tom (id,name,age) values(1,'xxx',20),(2,'yyy',7),(3,'zzz',23);
#在从库上查看是否同步成功
#查看数据库
show database;
use test1;
#查看表
show tables;
#查看数据
select * from test1;
```





### 补充：查看容器启动日志

docker logs b8bdd9c57f22

## 3 django实现读写分离



```python
makemigrations

#同步数据库
migrate   #表示同步到default数据库
migrate app01 --database=db1
```

### 3.2 手动控制读写分离

```
def index(request):

    #向数据表中存条数据
    #写到哪个库中了？默认情况下写到default
    # ret=models.Book.objects.using('default').create(name="xxx")
    # print(ret)


    #读第一条数据，默认从哪读？default
    #指定去从库读？在queryset对象后加个.using('db1')

    # ret=models.Book.objects.all().first()
    ret=models.Book.objects.all().using('db1').first()
    print(ret.name)
    return HttpResponse("ok")
```

### 3.3 自动读写分离

```python
#第一步：在项目根路径下创建一个py文件（database_router.py）
class DatabaseAppsRouter(object):
    def db_for_read(self, model, **hints):
      	#读可能去db1或者db2中读，
        return 'db1'
    def db_for_write(self, model, **hints):
        print(model)
        return 'default'

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        print(db)
        print(app_label)
        print(model_name)
        if app_label=='app01' and db=="db1":
            return False
        return None
 # 第二部，在setting中配置
DATABASE_ROUTERS = ['mydjangotest.database_router.DatabaseAppsRouter']

```



### 3.4 allow_migrate的使用

```python
migrate app01 --database=db1   #不会同步

migrate app01 --database=default #可以同步

#用来控制数据表同步时，不能同步到从库中
```









作业：

1 通过dockerfile构建可以运行项目的镜像

2 搭建mysql主从

3 django做读写分离









