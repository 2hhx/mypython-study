
    Docker官方建议在Ubuntu中安装，因为Docker是基于Ubuntu发布的，而且一般Docker出现的问题Ubuntu是最先更新或者打补丁的。在很多版本的CentOS中是不支持更新最新的一些补丁包的。
    
    由于我们学习的环境都使用的是CentOS，因此这里我们将Docker安装到CentOS上。注意：这里建议安装在CentOS7.x以上的版本，在CentOS6.x的版本中，安装前需要安装其他很多的环境而且Docker很多补丁不支持更新。
    

## 第一步：yum 包更新到最新

    
    
    sudo yum update

## 第二步：安装需要的软件包

yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

    
    
    sudo yum install -y yum-utils device-mapper-persistent-data lvm2

## 第三步：设置yum源为阿里云

    
    
    sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

## 第四步：安装docker

    
    
    sudo yum install docker-ce

## 第五步：安装后查看docker版本

    
    
    docker -v

## 第六步：设置ustc的镜像

ustc是老牌的linux镜像服务提供者了，还在遥远的ubuntu 5.04版本的时候就在用。ustc的docker镜像加速器速度很快。ustc
docker mirror的优势之一就是不需要注册，是真正的公共服务。

<https://lug.ustc.edu.cn/wiki/mirrors/help/docker>

编辑该文件：

    
    
    vi /etc/docker/daemon.json  

在该文件中输入如下内容：

    
    
    {
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
    }

## 第七步：Docker的启动与停止

**systemctl** 命令是系统服务管理器指令

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

