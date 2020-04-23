# rpm和yum

## 1 rpm包的管理

### 1.1 介绍

一种用于互联网下载包的打包及安装工具.它生成具有.RPM
扩展名的文件。RPM
是 RedHat
Package Manager（RedHat 软件包管理工具）的缩写，类似
windows 的 setup.exe



### 1.2 rpm包的简单查询指令

```
rpm	–qa				查询已安装的 rpm 列表
```



### 1.3 rpm 包名的基本格式

一个 rpm 包名：firefox-45.0.1-1.el6.centos.x86_64.rpm

```
firefox:名称
45.0.1-1:版本号
el6.centos.x86_64:centos6.X---64位
```



### 1.4 rpm其它指令

```
rpm -q 软件包名		 			查看是否已经安装
rpm -qi 软件包名	 			查看软件包信息
rpm -ql 软件包名	 			查看软件包中的文件安装位置
rpm -qf 文件(如:/etc/passwd)	 查看某个文件属于哪个rpm包
```



### 1.5 卸载rpm包

```
rpm -e 包名
```

```
例子:
rpm -e firefox
```



**包依赖问题:**

如果其它软件包依赖于你要卸载的软件包，卸载时则会产生错误信息

```
 rpm -e --nodeps 包名				强制删除
```



### 1.6 安装rpm包

```
rpm -ivh RPM 包全路径名称

i:install 安装
v:verbose 提示
h:hash 进度条
```

```
例子:安装firefox

1.挂载centos的iso镜像文件
2.media下找到rpm
3.拷贝到opt下
4.安装
```



## 2 yum

### 2.1 说明

Yum
是一个
[Shell ](https://baike.baidu.com/item/Shell)前端软件包管理器。基于 [RPM ](https://baike.baidu.com/item/RPM)包管理，能够从指定的服务器自动下载
RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。使用 yum 的前提是可以联网。

类型python中的pip

换源：按照清华源安装



### 2.2 基本指令

```
yum list|grep xx		查询yum服务器上是否有需要安装的包
yum install xxx			下载安装
yum uninstall xxx		删除
```

```
例子:
1.使用yum下载安装firefox
```





