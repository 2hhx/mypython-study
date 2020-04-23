# Centos7 安装jdk1.8

## 一 下载linux版jdk

我们安装jdk1.8

地址：https://www.oracle.com/java/technologies/oracle-java-archive-downloads.html

<img src="Centos7%20%E5%AE%89%E8%A3%85jdk1.8.assets/image-20200315233745095.png" alt="image-20200315233745095" style="zoom:30%;" />

找到对应版本

<img src="Centos7%20%E5%AE%89%E8%A3%85jdk1.8.assets/image-20200315233831310.png" alt="image-20200315233831310" style="zoom:30%;" />

下载：需要账号（从网上找个别人的账号或者注册一个即可：账号2696671285@qq.com ，密码Oracle123）
<img src="Centos7%20%E5%AE%89%E8%A3%85jdk1.8.assets/image-20200315233907575.png" alt="image-20200315233907575" style="zoom:30%;" />

## 二  解压

```python
#创建目录
mkdir /usr/local/java
# 将下载的tar.gz 解压到当前路径下
tar -zxvf jdk-8u201-linux-x64.tar.gz
'''
命令介绍：
tar　　　　　　备份文件
-zxvf　　　　　
-z　　　　　　 　　　　　　　　  通过gzip指令处理备份文件
-x　　　　　　　　　　　　　　   从备份文件中还原文件
-v　　　　　　　　　　　　　　   显示指令执行过程
-f　　　　　　 　　　　　　　　   指定备份文件
jdk-8u201-linux-x64.tar.gz　　　　文件名
'''

```

## 三 配置环境变量

```python
#vim 打开profile
vim /etc/profile
#在最后一行输入
export JAVA_HOME=/usr/local/java/jdk1.8.0_231
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=$PATH:${JAVA_HOME}/bin
  
#使配置生效
source /etc/profile

#检查
java -version
java version "1.8.0_231"
Java(TM) SE Runtime Environment (build 1.8.0_231-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.231-b11, mixed mode)
```

