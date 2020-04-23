# 程序员装逼之----Ubuntu

## 1 Ubuntu介绍

1.Ubuntu是一个以桌面应用为主的开源操作系统,它的界面做的非常好看

2.专业的程序员一般会选择Ubuntu

```
原因:
1.为了装逼
2.开发环境更加接近真实服务器环境,因为都是基于GNU/Linux内核开发的
3.穷
```

3.下载地址:[http://cn.ubuntu.com/download](http://cn.ubuntu.com/download/)



## 2 安装

略,比centos更加简单



## 3 设置Ubuntu支持中文

默认安装的
ubuntu 中只有英文语言，因此是不能显示汉字的。要正确显示汉字，需要安装中文语言包。

### 步骤:

```
1.单击左侧图标栏打开 System Settings（系统设置）菜单，点击打开 Language Support（语言支持）选项卡。
2.点击 Install / Remove Languages，在弹出的选项卡中下拉找到 Chinese(Simplified)，即中文简体， 在后面的选项框中打勾。然后点击 Apply Changes 提交，系统会自动联网下载中文语言包。（保证
ubuntu 是联网的）。
3.这时“汉语（中国）”在最后一位因为当前第一位是”English”，所以默认显示都是英文。我们如果希望默认显示用中文，则应该将“汉语（中国）”设置为第一位。设置方法是拖动，鼠标单击
“汉语（中国）”，当底色变化（表示选中了）后，按住鼠标左键不松手，向上拖动放置到第一位。
4.设置后不会即刻生效，需要下一次登录时才会生效。
```



## 4  root用户

ubuntu安装后,默认是普通用户,这时候要获得权限就得:

1.sudo

2.su root

 

### 设置root用户的密码并使

```
1.sudo passwd
```





## 5 Ubuntu使用python

ubuntu安装成功后,默认会带上python2 和 python3,无需另外安装

ubuntu下可以安装各种python的ide环境,包括pycharm

https://baijiahao.baidu.com/s?id=1622347860160507809&wfr=spider&for=pc





## 6 apt软件管理工具

apt 是 Advanced Packaging Tool 的简称，是一款安装包管理工具。在 Ubuntu 下，我们可以使用 apt

命令可用于软件包的安装、删除、清理等，



### apt软件相关命令

```
sudo apt-get update	更新源
sudo apt-get install package 安装包
sudo apt-get remove package 删除包
sudo apt-cache show package	获取包的相关信息，如说明、大小、版本等
sudo apt-get source package	下载该包的源代码


----------------------以上命令最为常用---------------------------


sudo apt-cache search package 搜索软件包
sudo apt-get install package --reinstall	重新安装包
sudo apt-get -f install	修复安装
sudo apt-get remove package --purge 删除包，包括配置文件等
sudo apt-get build-dep package 安装相关的编译环境
sudo apt-get upgrade 更新已安装的包
sudo apt-get dist-upgrade 升级系统
sudo apt-cache depends package 了解使用该包依赖那些包sudo apt-cache rdepends package 查看该包被哪些包依赖

```



### 更换镜像源

清华开源软件镜像站:https://mirrors.tuna.tsinghua.edu.cn/

ubuntu的软件源配置文件是/ect/apt/source.list



#### 1.备份/ect/apt/source.list

```
mv /ect/apt/source.list /ect/apt/source.list.backup
```

**若权限不够,切换root用户,或使用sudo**



#### 2.替换/ect/apt/source.list内容

```
1.vim /ect/apt/source.list
2.写入清华镜像源文件内容
```



#### 小案例

```
1.apt-get remove vim
2.apt-get install vim
3.apt-cache show vim
```



## 7 ssh远程登录

和 CentOS 不一样，Ubuntu 默认没有安装
SSHD 服务,因此需要安装



###7.1  安装

```
apt-get install openssh-server
service sshd restart

查看监听状态:
netstat -nap | more
```

此时xshell就可以连接了



### 7.2 其它

```
openssh-sever会安装客户端和服务端,
所以ubuntu在此时也可以连接其它有sshd服务的机器


基本语法：
ssh 用户名@IP
例如：ssh mac@192.168.188.131
使用 ssh 访问，如访问出现错误。可查看是否有该文件 ～/.ssh/known_ssh  尝试删除该文件解决。
登出命令：exit 或者 logout
```

