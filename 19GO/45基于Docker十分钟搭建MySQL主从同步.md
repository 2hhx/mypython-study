目录

  * 一 主从配置原理
  * 二 操作步骤
    * 2.1我们准备两台装好mysql的服务器（我在此用docker模拟了两台机器）
    * 2.2 远程连接入主库和从库
    * 2.3 测试主从同步

## 一 主从配置原理

![image-20191104214516600](https://tva1.sinaimg.cn/large/006y8mN6gy1g8mcd6efcdj31q10u01kx.jpg)

mysql主从配置的流程大体如图：

1）master会将变动记录到二进制日志里面；

2）master有一个I/O线程将二进制日志发送到slave;

3) slave有一个I/O线程把master发送的二进制写入到relay日志里面；

4）slave有一个SQL线程，按照relay日志处理slave的数据；

## 二 操作步骤

### 2.1我们准备两台装好mysql的服务器（我在此用docker模拟了两台机器）

环境 | mysql版本 | ip地址:端口号  
---|---|---  
主库（master） | 5.7 | 172.16.209.100:33307  
从库（slave） | 5.7 | 172.16.209.100:33306  
  
用docker拉起两个mysql容器，步骤如下（对docker不熟悉的同学可以查看docker快速入门章节）：

    
    
    # 拉取mysql5.7镜像
    docker pull mysql:5.7
    #在home目录下创建mysql文件夹，下面创建data和conf.d文件夹
    mkdir /home/mysql
    mkdir /home/mysql/conf.d
    mkdir /home/mysql/data/
     
    创建my.cnf配置文件
    touch /home/mysql/my.cnf
    
    my.cnf添加如下内容：
    [mysqld]
    user=mysql
    character-set-server=utf8
    default_authentication_plugin=mysql_native_password
    secure_file_priv=/var/lib/mysql
    expire_logs_days=7
    sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
    max_connections=1000
    
    [client]
    default-character-set=utf8
    
    [mysql]
    default-character-set=utf8
    #启动主库容器（挂载外部目录，端口映射成33307，密码设置为123456）
    docker run  -di -v /home/mysql/data/:/var/lib/mysql -v /home/mysql/conf.d:/etc/mysql/conf.d -v /home/mysql/my.cnf:/etc/mysql/my.cnf -p 33307:3306 --name mysql-master -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
    #启动从库容器（挂载外部目录，端口映射成33306，密码设置为123456）
    docker run  -di -v /home/mysql2/data/:/var/lib/mysql -v /home/mysql2/conf.d:/etc/mysql/conf.d -v /home/mysql2/my.cnf:/etc/mysql/my.cnf -p 33306:3306 --name mysql-slave -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7

### 2.2 远程连接入主库和从库

    
    
    #连接主库
    mysql -h 172.16.209.100 -P 33307 -u root -p123456
    #在主库创建用户并授权
    ##创建test用户
    create user 'test'@'%' identified by '123';
    ##授权用户
    grant all privileges on *.* to 'test'@'%' ;
    ###刷新权限
    flush privileges;
    #查看主服务器状态(显示如下图)
    show master status; 

![image-20191105085514111](https://tva1.sinaimg.cn/large/006y8mN6gy1g8mvq2x880j30ys0620ux.jpg)

    
    
    #连接从库
    mysql -h 172.16.209.100 -P 33306 -u root -p123456
    #配置详解
    /*
    change master to 
    master_host='MySQL主服务器IP地址', 
    master_user='之前在MySQL主服务器上面创建的用户名'， 
    master_password='之前创建的密码', 
    master_log_file='MySQL主服务器状态中的二进制文件名', 
    master_log_pos='MySQL主服务器状态中的position值';
    */
    #命令如下
    change master to master_host='172.16.209.100',master_port=33307,master_user='test',master_password='123',master_log_file='mysql-bin.000005',master_log_pos=0;
    #启用从库
    start slave;
    #查看从库状态（如下图）
    show slave status\G;

![image-20191105090050611](https://tva1.sinaimg.cn/large/006y8mN6gy1g8mvvzo3xzj30u00u5keo.jpg)

### 2.3 测试主从同步

    
    
    #在主库上创建数据库test1
    create database test1;
    use test1;
    #创建表
    create table tom (id int not null,name varchar(100)not null ,age tinyint);
    #插入数据
    insert tom (id,name,age) values(1,'xxx',20),(2,'yyy',7),(3,'zzz',23);
    
    
    #在从库上查看是否同步成功
    #查看数据库
    show database;
    use test1;
    #查看表
    show tables;
    #查看数据
    select * from test1;

可以看到大功告成

