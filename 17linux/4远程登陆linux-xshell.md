# 远程登录Linux系统

## 4.1 为什么要远程登录

## 4.2 Xshell5安装

略

## 4.3 连接登录

### 4.3.1 连接前提

需要Linux开启一个sshd的服务,监听22号端口,一般默认是开启的

**查看是否开启:**

```
chkconfig --list | grep sshd
```

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122526632-2079305660.png)



**手动开启:**

```
chkconfig  --level 5 sshd on

service sshd restart
```



### 4.3.2 Xshell连接配置

**查看虚拟机ip:**

```
ifconfig
```

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122536918-1139865305.png)



**配置Xshell:**

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122523963-907403924.png)



**测试:**

1.在/home目录下创建文件

```
cd /home
touch hello.py
```

2.重启服务器

```
reboot
```

