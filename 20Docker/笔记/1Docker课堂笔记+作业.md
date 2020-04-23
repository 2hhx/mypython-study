# 今日内容：Docker

课件地址：https://www.cnblogs.com/xiaoyuanqujing/articles/11774978.html

## 一 docker介绍

### 云服务器厂商：阿里云，腾讯云，新睿云，

### 阿里云：阿里飞天

### 小的云服务厂商：openstack，python写的

### 虚拟机：虚拟化的一种

## Docker

### 基于go语言实现的

### 二次开发：在原有的系统上进行开发，原有系统源码拿不到

### docker 小历史：业余项目，对外收费，卖软件，软件卖不出去，没人用，公司濒临倒闭，把docker开源了，出了两个版本，一个是社区版本（免费），一个是企业版（收费）

### ERP厂商 ，北用友，南金蝶，浪潮国际 ：c#，python   odoo

### 售前技术工程师  售后技术支持工程师



### docker 实现轻量级的操作系统虚拟化，基于linux操作系统上的lxc实现的

### 没有docker之前，项目上线

#### 开发完成--代码由运维人员从git上来下来---在服务器上装环境（python，mysql，reidis，环境版本问题）---运行项目

### 如果有了docker

#### 开发完成--代码由运维人员从git上来下来---docker镜像（不再需要重复的安装软件，解决的就是软件版本不一致的问题）---运行

### docker 更轻量级

#### 启动速度快，占用体积小，一个centos7的系统，只需要几十个m（没有操作系统）

#### 在操作系统之上通过lxc技术，实现虚拟化出操作系统

#### docker 是一个软件，通过这个软件虚拟化出操作系统





### django、flask 同步框架

### tornado，sanic异步框架:一旦异步，以后全都得用异步

#### asyncio 提前看一下，有兴趣的同学，学习一下

# 二 docker架构和使用

### docker 是一个软件，c/s架构的软件（mysql，redis）

### 但是，mysql客户端和服务端通信时，基于socket，自己定制的协议，pymysql就是个mysql的客户端（跟navicate，go语言的连接mysql的代码和java连接mysql的代码都是一个东西）

### docker 客户端跟服务端通信，是通过http协议，resful规范（新的软件，基本上都是走http协议，resful规范），es

## 重点 ：镜像和容器

### 基于镜像来运行容器：镜像是面向对象中的类，容器是面向对象中的对象

### 注册中心：docker 是一个软件，需要虚拟化出操作系统，操作系统从哪来？从注册中心拉下来的，从注册中心来下来的东西，叫镜像（比如要拉一个centos7的镜像），镜像运行起来，叫容器（才是真正的跑起来的操作系统）

### 一个centos7镜像，跑起来两个容器，在一个容器上装了mysql，在另一个上装了python3.6

## 2.1docker 安装

### windows（win10及以上，下一个docker软件，下一步安装就可以了）/linux(乌班图，centos)/Mac

### 乌班图上很简单，因为docker就是在乌班图上开发的

### 在centos上安装，要在7.x以上，在上面装docker

### mac本质就是unix操作系统，在终端连接

### ssh root@101.133.225.166

## 2.2 docker 安装

```python

sudo yum update
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  
sudo yum install docker-ce
#docker软件安装完成，服务端和客户端都安装完成，
docker -v #查看客户端版本 
```

## 2.3 启动docker

### docker 安装完，服务端没有启动，需要启动

```python
systemctl start docker # 7.x用这个命令启动
systemctl stop docker #停止docker服务端
  
  
# 了解
启动docker：

systemctl start docker
停止docker：

systemctl stop docker
重启docker：

systemctl restart docker
查看docker状态：

systemctl status docker
开机启动：

systemctl enable docker
查看docker概要信息

docker info
查看docker帮助文档

docker --help
```

## 2.4 镜像操作

### 查找镜像：docker search centos，会去https://hub.docker.com/查找

### 直接去该地址查找即可

镜像名称，是否是官方镜像，描述，stars数

### 拉去centos7的版本镜像

docker pull centos:centos7

docker pull centos:8

docker pull python:3.6   其实下载下什么东西？镜像，其实是一堆文件，

那centos7和python的区别是什么？如果下载python3.6 ：就是一个操作系统上安装了python环境，从官方拉，是乌班图+python

docker pull mysql:5.6    下载了一个镜像，镜像里安装了mysql，就相当于操作系统上安装了mysql

### 查看服务端所有的镜像

docker images

### 删除镜像 (容器如果没有删除，镜像删除不了)

docker rmi id号   id号可以缩写

docker rmi 5e3

## 2.5 容器操作

镜像运行，是启动容器，一个个的容器，就是一个个的操作系统

一个centos7的镜像，可以跑起多个容器，每一个容器就是一个操作系统

### 查看容器

docker ps：查看正在运行的容器

docker ps –a：查看所有容器（包括停止和运行的）

### 创建启动容器

docker run -it --name=mycentos centos:centos7 /bin/bash

exit  退出容器,容器也就停止了

docker run -di --name=mycentos2 centos:centos7     不进入容器 内部

docker exec -it mycentos2  /bin/bash    进入到容器内容

exit退出，容器不停止



docker stop id号或者名字    容器停止

docker start id号或者名字   启动容器



# 作业：

 安装centos7.5虚拟机

安装docker

启动docker服务

拉取centos7镜像

守护进程形式启动镜像

进入，退出镜像

（在镜像内安装python3.6）



































































