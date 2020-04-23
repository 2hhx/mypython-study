## 一 Docker引擎

docker引擎是一个c/s结构的应用，主要组件见下图：

  * Server是一个常驻进程
  * REST API 实现了client和server间的交互协议
  * CLI 实现容器和镜像的管理，为用户提供统一的操作界面
  * image是镜像
  * container是容器

注意：

Docker 容器通过 Docker 镜像来创建。

容器与镜像的关系类似于面向对象编程中的对象与类。

**Docker ------ > 面向对象**

**容器 \------> 对象**

**镜像 \------> 类**

###
![](https://img2018.cnblogs.com/blog/1350514/201905/1350514-20190523183937775-2046235548.png)

## 二 Docker构架

Docker使用C/S架构，Client
通过接口与Server进程通信实现容器的构建，运行和发布。client和server可以运行在同一台集群，也可以通过跨主机实现远程通信。

**client：客户端**

**docker_host：宿主主机**

**registry：仓库：私服和中央仓库（Docker Hub）**

![](https://img2018.cnblogs.com/blog/1350514/201905/1350514-20190523184032744-1773786251.png)

## 三 核心概念

#### 镜像(image)

ocker 镜像是用于创建 Docker 容器的模板

#### 容器(container)

容器是独立运行的一个或一组应用

#### 客户端（Client）

Docker 客户端通过命令行或者其他工具使用 Docker API
(<https://docs.docker.com/reference/api/docker_remote_api>) 与 Docker 的守护进程通信。

#### 主机(Host)

一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。

#### 仓库(Registry)

Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。

Docker Hub(<https://hub.docker.com>) 提供了庞大的镜像集合供使用。

