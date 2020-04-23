# docker

## 昨日回顾：

### 1 介绍docker，一种虚拟化的技术，比虚拟机更轻量级，启动速度更快，基于linux的lxc容器技术，go语言编写，社区版（docker-ce），企业版

### 2 企业应用比较广泛，方便

### 3 安装：7.x以上

### yum install -y yum-utils device-mapper-persistent-data lvm2

### sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

### yum install docker-ce

### 4 启动停止

docker -v：客户端的版本

systemctl start docker

systemctl stop docker

### 5 镜像操作

注册中心拉下来

docker search 名字   搜索镜像

docker pull 镜像名字:镜像的tag   拉取镜像，在docker服务端就可以查看到

docker images   查看所有镜像

docker rmi 镜像id   删除镜像

### 6 容器操作

docker ps    查看正在运行的容器

docker ps -a  查看所有容器，包括停止的

docker run -it --name=mycentos -p 80:80 -v 宿主机目录:容器目录 centos:7 /bin/bash     一旦退出容器，容器也就停止了

docker run -id --name=mycentos -p 80:80 -v 宿主机目录:容器目录 centos:7   不进入容器，可以选择进去

docker exec -it mycentos /bin/bash   进入容器内部

exit 出来

docker stop 容器名/容器id    停止容器

docker start 容器名/容器id    启动容器



## 今日内容

### 1拷贝文件

1 docker cp 1.txt mycentos2:/home   从宿主机拷贝文件到容器内部

2 docker cp mycentos2:/home/1.txt /home/1.txt  从容器往外拷贝，不需要进入容器内部

### 2 目录挂载

docker run -di -v /home/lqz:/home --name=mycentos3 centos:centos7

### 3 删除容器

docker rm 容器id/名字    删除容器

### 4 查看容器的ip地址

1 查看容器详情：docker inspact 容器名字/id    NetworkSettings下的IPAddress是容器的ip地址

2 查看容器ip    docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名称（容器ID）





### 4 服务部署

### 部署mysql

拉取：docker pull mysql:5.6

启动：docker run -di --name=mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.6

远程连接宿主机的3306端口，即可连接到容器的mysql服务

### 部署redis

docker  pull reids     不加tag ，拉最新的版本，也就是lasted版本

docker run -di --name=myredis -p 6379:6379 redis  启动redis

远程连接即可

### 5 迁移与备份

### 将容器保存为镜像

docker commit 容器名字 要打包成的镜像名字

docker commit mycentos2 lqz_centos7

基于打包好的镜像，再跑起容器来，那么容器内部原来装的软件，都会有

docker run -di --name=lqz_centos7_1 lqz_centos7:latest

启动起容器来，软件都会有

### 镜像备份

docker  save -o lqz_centos7.tar lqz_centos7

### 镜像恢复

docker load -i lqz_centos7.tar

### 6 私有仓库

1 docker pull registry   拉下一个镜像

2 docker run -di --name=registry -p 5000:5000 registry   启动镜像形成容器

3 打开浏览器 输入地址http://101.133.225.166:5000/v2/_catalog    没有上传镜像，是空的 {"repositories":[]}

### 配置私有仓库

vi /etc/docker/daemon.json

{"insecure-registries":["101.133.225.166:5000"]} 

重启docker 服务

systemctl restart docker

重启registry的容器

docker restart registry

### 上传到私有仓库

打标签

docker tag lqz_centos7 101.133.225.166:5000/lqz_centos7

上传标记的镜像

docker push 101.133.225.166:5000/lqz_centos7



本地删除镜像后，可以从私有仓库拉下来

docker pull 101.133.225.166:5000/lqz_centos7

再跑起容器，内部就有aaa  bbb文件夹



作业：

1 从宿主机拷贝文件到容器，从容器拷贝文件到宿主机

2 启动容器，目录挂载

3 在容器中部署mysql服务

4 在容器中 部署redis服务

5 将容器打包成镜像

6 将镜像压缩成tar

7 将自己制作的镜像传到私有仓库































