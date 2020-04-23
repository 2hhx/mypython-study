## 0 分库分表思路

```python
mysql---->换成非关系型数据库，文档型数据库（mongodb，es）

实际开发中：关系型数据库，一般不主动创建外键关联（author：id，authordetail_id    authordetail表:id）,影响效率，django中关联查询，__连表查询，照样用，代码一点都不需要改 db_constints=False
  
表模型：xx=models.ForainKey(to="",to_field="",db_constints=False)    不主动创建出外键 ，__连表查询该怎么用怎么用？
makemigrations  migrate  该怎么用怎么用，完全不影响

er图---画图

mongodb：在爬虫那学的，只能做爬虫，   我们实际的项目，redis，mongod：增长很快的数据，访问记录日志， mysql，es
{userid:1,adsfadsfasdfasefads}  

Tablestore  自己学的看到的加到简历  

润色

文档型，json数据----》表关联
author表
{id:1,name：lqz,authordetail_id:1}
authordetail表
{id:1,address:上海}


事务：分布式锁实现+代码控制


数据量非常非常大了，就要用关系型数据库：分库分表

垂直和水平  库 和表
垂直分库：把相关业务的表，单独拆到一个数据库中  用户相关放到一个库中     订单相关的放到一个库中 
垂直分表：


垂直分表，水平分表：写到简历上

专注点：放在分表上，业务不用写了   有统一标准的（别人写好的，给你用，免费，收费的）
orm  ：对象关系映射    sqlachemy：要求会，sqlachemy   不写web项目，也可以使用sqlachemy


sqlachemy：单独的，python：orm  

你一定要写一个，放到git：开源软件作者
数据库中间件：MyCat：专注于分库分表解决方案


django是什么架构：mtv       mvc   它认为你说错了，他对django不熟，你说的都不对，你瞎说，你学了些什么乱七八糟
以至于后面不管说什么对的，这个人都认为咱同学在瞎扯淡，


中间件： 咱们同学说的中间件（django的中间件），跟下面的东西完全不是一个概念
对面那个人做java的：服务器中间件（tocmcat，jboss，uwsgi），消息队列中间件（rabbitmq，kafka），数据库中间件（mycat）
```



## 1 数据库中间件MyCat介绍

````python
介于我们的程序和数据库之间的一个东西：负责分库分表

国内：技术实力最牛B的就是阿里，热衷于开源   腾讯开源过什么？
java技术栈：dubbo（java上用的，开源的一个rpc框架，python一般用grpc：谷歌开源的），Cobar，中文的
rpc：远程过程调用 （python用的比较少）在远端调用一个方法，就像调用我本地的方法一样
google技术是最牛B的



ios：必须用mac系统，object-c（跟c不完全一样），成本比较高      语言：swift，苹果公司一个工程师写出来
刚推出来没几年，工程师辞职了，去了facebook   到现在：swift
android：java，windows，mac，linux。。。（谷歌一直在跟甲骨文打官司）      google推出 kotlin，用来开发android  
java  sun出的，后来被甲骨文收购了，java收费。。。


新东西，可以去学，可以去用，3.8出了，先不要着急去用，先让别人猜猜坑    项目越大，用户越多  越不能改  大公司用的都是一些老技术     python 2.几
 
  
java写的：实现了 MySQL 公开的二进制传输协议，巧妙地将自己伪装成一个  MySQL Server

python写一个开源出来，写着玩，用的人比较少，性能可能跟不上（go实现，放到git上，不需安装额外的东西）java  jdk

对于开发人员来说根本感觉不到mycat的存在：代码一点都不需要动，只是连接改一下就行了

````



## 2 下载安装

```python
# 1 centos7 上装jdk
	-mycat基于java写的
  -详见md
# 2 至少两台mysql （docker模拟）
docker run -di --name=test1_mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
docker run -di --name=test2_mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
# 3 下载安装mycat
	#http://www.mycat.org.cn/
  # 解压
  # bin：启动目录
  	-./mycat start    后台执行
    -./mycat stop
    -./mycat restart
    -./mycat console   看到窗口，所有的日志信息能打印
  # config配置文件(xml配置，java写的一般用xml     yaml) 
  	-server.xml：mycat的配置，指定mycat的端口，用户，密码
    -rule.xml：规则，分片规则，分表规则
    -schema.xml：mycat和数据库之间连接的配置，连接到哪几个数据库，对哪几个表分表
  	
  
```





## 3 id范围分表

默认情况  id范围0---500万 放在3306:lqz/user

id范围500万---1000万 放在3307:lqz/user

0--5000  3306:lqz/user

5000-1000 3307:lqz/user

修改：schema.xm

```xml
<?xml version="1.0"?>
<!DOCTYPE mycat:schema SYSTEM "schema.dtd">
<mycat:schema xmlns:mycat="http://io.mycat/">

	<schema name="TESTDB" checkSQLschema="true" sqlMaxLimit="100" randomDataNode="dn1">
		<table name="user" primaryKey="id" dataNode="dn1,dn2" rule="auto-sharding-long" autoIncrement="true" fetchStoreNodeByJdbc="true">
			<childTable name="customer_addr" primaryKey="id" joinKey="customer_id" parentKey="id"> </childTable>
		</table>
	</schema>
	<dataNode name="dn1" dataHost="localhost1" database="lqz" />
	<dataNode name="dn2" dataHost="localhost2" database="lqz" />
	<dataHost name="localhost1" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		<!-- can have multi write hosts -->
		<writeHost host="hostM1" url="jdbc:mysql://101.133.225.166:3306" user="root"
				   password="123456">
		</writeHost>

	</dataHost>
	<dataHost name="localhost2" maxCon="1000" minCon="10" balance="0"
			  writeType="0" dbType="mysql" dbDriver="jdbc" switchType="1"  slaveThreshold="100">
		<heartbeat>select user()</heartbeat>
		<writeHost host="hostM1" url="jdbc:mysql://101.133.225.166:3307" user="root"
				   password="123456">
		</writeHost>
	</dataHost>
	
	
</mycat:schema>
```

autopartition-long.txt

```pyhthon
# range start-end ,data node index
# K=1000,M=10000.
0-5k=0
5k-10k=1
```

### 重启mycat

```python
1 ./mycat restart 
2 在3306库下创建lqz数据库和user表
4 在3307库下创建lqz数据库和user表
5 操作mycat服务，就直接可以操作上面两个库

```

## 4 hash分表 

id不是趋势递增

```python
#sheme.xml
	<schema name="TESTDB" checkSQLschema="true" sqlMaxLimit="100" randomDataNode="dn1">
		<!-- auto sharding by id (long) -->
		<!--splitTableNames 启用<table name 属性使用逗号分割配置多个表,即多个表使用这个配置-->
<!--fetchStoreNodeByJdbc 启用ER表使用JDBC方式获取DataNode-->
		<table name="user" primaryKey="nid" dataNode="dn1,dn2" rule="auto-sharding-long" autoIncrement="true" fetchStoreNodeByJdbc="true">
		</table>
		<table name="article" primaryKey="id" dataNode="dn1,dn2" rule="sharding-by-murmur">
		</table>
	</schema>
#rule.xml 按那一列做hash，id这一列

<tableRule name="sharding-by-murmur">
		<rule>
			<columns>id</columns>
			<algorithm>murmur</algorithm>
		</rule>
	</tableRule>

1 在3306库下创建lqz数据库和table表
2 在3307库下创建lqz数据库和table表
3 重启 ./mycat restart 

4 在mycat中插入数据，按id字段hash，会均匀的分到两个库中
```



你们公司，就是单个数据库，主从，读写分离，分库分表





简历上：通过mycat中间分库分表，id范围分，hash分表





# 作业

如何提高网站并发量，redis----尽量让数据别打到mysql ，读写分离，查询效率还低，就用分库分表

1 通过id范围分表

2 通过hash分表