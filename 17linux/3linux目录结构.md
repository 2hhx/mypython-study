# Linux目录结构(重点)

## 1.Linux目录与Windows目录对比

### 1.1 Windows目录结构

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122511624-322546941.png)

### 1.2 Linux目录结构

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122520265-2136687994.png)

**深刻理解Linux 树状文件目录是非常重要的,只有记住他们,你才能在命令行中任意切换,想去哪里去哪里**



## 2 Linux世界里---一切皆文件

对于Linux而言,所有的东西都是文件

比如说,cpu会映射到/dev下的cpu这个目录

再比如说,硬盘(disk)会被映射到/dev下的disk这个目录



## 3 Linux目录结构详解

### 3.1 /bin

存放最经常使用的指令的,比如说cp,ls,kill

### 3.2 /sbin

系统管理员使用的系统管理指令

### 3.3 /home

存放普通用户的主目录,在Linux中每个用户都有一个自己的目录,一般该目录是以用户的账号命名的

### 3.4 /root

系统管理员的用户主目录

### 3.5 /boot

存放的是启动Linux时使用的一些核心文件

### 3.6 /lib

库文件存放目录

### 3.7 /etc

存放所有系统管理所需要的配置文件,比如说mysql中的配置文件,my.conf

### 3.8 /usr

用户的很多应用程序和文件都放在这个目录下,有点像Windows下的program files目录

### 3.9 /proc,别动

这是系统内存的映射

### 3.10 /srv,别动

service的缩写,存放的是一些服务启动之后需要使用的数据

### 3.11 /sys,别动

系统相关文件

### 3.12 /tmp

用来存放临时文件

### 3.13 /dev

类似于windows的设备管理器,把所有的硬件用文件的形式存储

### 3.14 /media

Linux会识别一些设备,例如U盘,光驱等等,识别后,Linux会把识别的设备挂载到这个目录下

### 3.15 /mnt

用于让用户临时挂载别的文件系统,我们可以将外部的存储挂载在/nmt/上,然后进入该目录就可以查看里面的内容的,如我们之前设置的共享文件夹

### 3.16 /opt

正常这个文件夹是用来放安装包的

### 3.17 /usr/local

安装后的程序存放的地方

### 3.18 /var

存放经常需要被修改的文件,比如各种日志文件

### 3.19 /selinux

全名--- security enhanced linux,安全加强linux

这个类似于windows中的杀毒软件,是一种安全系统,比如收到攻击的时候这个文件会被触发



