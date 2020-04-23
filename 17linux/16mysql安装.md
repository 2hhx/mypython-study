# mysql安装

以源码安装的方式编译和安装Mysql 5.6

## 1 卸载旧版本

```
rpm -qa | grep mysql		检查是否有旧版本

查询结果:mysql-libs-5.1.73-7.el6.x86_64

rpm -e mysql-libs			删除旧版本
rpm -e --nodeps mysql-libs	强行删除

```

## 2 安装mysql

### 2.1 安装源码需要编译

```
下载c的编译工具
yum -y install make gcc-c++ cmake bison-devel  ncurses-devel
```

### 2.2 上传本地mysql5.6源码包至/opt

```
xftp连接上传
```

### 2.3 编译

```
tar -zxvf mysql-5.6.14.tar.gz		解压
cd mysql-5.6.14						切换目录

编译准备:
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci

编译并安装:
make && make install
```

出错了，删除CMakeCache.txt，在从新执行cmake

### 2.4 配置mysql

```
权限管理:0

1.创建mysql组,及用户
	groupadd mysql
	useradd -g mysql mysql



初始化配置:
1.cd /usr/local/mysql
2.scripts/mysql_install_db（当前目录必须在mysq下）（最好用mysql用户）
3.修改/usr/local/mysql权限
	chown -R mysql:mysql /usr/local/mysql

	#测试是否成功
		support-files/mysql.server start
		bin/mysql
在启动MySQL服务时，会先在/etc目录下找my.cnf，找不到则会搜索"$basedir/my.cnf"，在本例中就是 /usr/local/mysql/my.cnf

查看/etc下是否有my.cnf,有就换个名字,防止干扰
	
1.mv /etc/my.cnf /etc/my.cnf.bak

添加服务(mysql服务放进/etc/init.d),并设置开机自启:
1.cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
2.chkconfig mysql on
3.service mysql star
1.vi /etc/profile
2.在文件中加入:
	PATH=$PATH:/usr/local/mysql/bin
	export PATH   #把path提升为环境变量
3.source /etc/profile #从新运行文件 让环境变量生效。
```



