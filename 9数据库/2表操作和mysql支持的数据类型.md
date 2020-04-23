# 表操作

## 表的基本操作

```python
前提：先选取要操作的数据库
1）进入指定库
mysql>:use db1;
2）确定当前使用的数据库
mysql>:select database();
3）查看当前数据库已有表
mysql>：show tables;

4）增加，创建表(字段1 类型, ..., 字段n 类型)
mysql>：create table 表名(字段们);
eg>: create table student(name char(16), age int);
eg>: create table t1(name varchar(16), age int);

5）查看创建表的详细信息
mysql>：show create table 表名;
eg>: show create table t1;


4）查看创建表的结构（字段结构信息）
mysql>：desc 表名;
eg>:desc t1;




5）删除表
mysql>: drop table 表名;
eg>: drop table teacher;
```



## 创建表的完整语法

```python
# 长度和约束在某些情况下是可以省略的
mysql>: create table 表名 (
	属性名1 类型(长度) 约束,
	...
	属性名n 类型(长度) 约束
) engine=引擎 default charset=utf8;
    
'''
create table 表名(
字段名1 类型[(宽度) 约束条件],
字段名2 类型[(宽度) 约束条件],
字段名3 类型[(宽度) 约束条件]
)engine=innodb charset=utf8;
'''
# []可选参数

```

举例：

```python
mysql> create database db1;
mysql> use db1;
mysql> create table student(id int primary key auto_increment,name varchar(16) unique,age tinyint unsigned default 0,height float(255,30) unsigned not null,mobile int,unique(name,mobile))engine=myisam charset=utf8;

mysql> desc student;
+--------+------------------------+------+-----+---------+----------------+
| Field  | Type                   | Null | Key | Default | Extra          |
+--------+------------------------+------+-----+---------+----------------+
| id     | int(11)                | NO   | PRI | NULL    | auto_increment |
| name   | varchar(16)            | YES  | UNI | NULL    |                |
| age    | tinyint(3) unsigned    | YES  |     | 0       |                |
| height | float(255,30) unsigned | NO   |     | NULL    |                |
| mobile | int(11)                | YES  |     | NULL    |                |
+--------+------------------------+------+-----+---------+----------------+
5 rows in set (0.02 sec)

mysql> show create table student \G;
*************************** 1. row ***************************
       Table: student
Create Table: CREATE TABLE `student` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(16) DEFAULT NULL,
  `age` tinyint(3) unsigned DEFAULT '0',
  `height` float(255,30) unsigned NOT NULL,
  `mobile` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `name` (`name`),
  UNIQUE KEY `name_2` (`name`,`mobile`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8
1 row in set (0.00 sec)

ERROR:
No query specified
```

## 表的详细操作

```mysql
1.修改表名
alter table 旧表名 rename 新表名;

2.修改表的引擎与字符编码
alter table 表名 engine="引擎名" charset="编码名";

3.复制表 *

# 结构
create table 新表名 like 旧表名;
eg:1
create table nt like tt; # 将tt的表结构复制到新表nt中, 约束条件一并复制
eg:2
create table nt1 select * from tt where 1=2; # 将tt的表结构复制到新表nt1中, 约束条件不会复制

# 结构+数据
create table 新表名 select * from 旧表名;
注: 会复制表结构+数据, 但不会复制约束条件


4.清空表
truncate 表名;
注：表被重置，自增字段重置


        1 .*#使用truncate清空全部数据主键自增长是从1开始(效率更高)*

        truncate table 表名;
        2 . #清空表数据并且将主键自增长从1开始(1.先清空表数据2.在把表的自增长设置为1)

        delete from 表名;

        alter table 表名 auto_increment=1;

```

## 表的修改(表名，字段名，字段属性)

```python
1）修改字段属性
mysql>:alter table 表名 modify name char(20);
eg>:alter table t1 modify name char(20);
2）修改字段名(将表t1的属性name修改为usr，并且字段属性改为char(16))
mysql>:alter table t1 change name usr char(16);
3） 修改表名
mysql>:alter table 表名 rename 修改后的表名;
eg>:alter table t1 rename t2;
```

# 记录（字段）的操作

## 记录（字段）的基本操作

```python
前提：知道具体操作的是那张表
1）查看某个数据库中的某个表的所有记录，如果在对应数据库中，可以直接查找表
mysql>: select * from [数据库名.]表名;
注：*代表查询所有字段

2）给表的所有字段插入数据
mysql>: insert [into] [数据库名.]表名 values (值1,...,值n);
eg：如果给有name和age两个字段的student表插入数据
1条>：insert into student values ('Bob', 18);
多条>：insert into student values ('张三', 18), ('李四', 20);
指定库>：insert owen.student values ('张三', 18), ('李四', 20);

3）根据条件修改指定内容
mysql>: update [数据库名.]表名 set 字段1=新值1, 字段n=新值n where 字段=旧值;
eg:> update student set name='王五', age='100' where name='张三';
注：i) 可以只修改部分字段 ii) 没有条件下，所有记录都会被更新
eg:> update student set name='呵呵';

4）根据条件删除记录
mysql>: delete from [数据库名.]表名 where 条件;
eg:> delete from student where age<30;
    删除字段为空的内容
    mysql>:delete from [数据库名.]表名 where age is NULL;

```

## 记录(字段)的详细操作

```mysql
create table tf1(
	id int primary key auto_increment,
    x int,
    y int
);

# 修改字段属性
alter table tf1 modify x char(4) default '';
#修改字段名(将表tf1的属性y修改为m，并且字段属性改为char(4)),加上约束default ''
alter table tf1 change y m char(4) default '';
#移动字段
alter table 表名 modify 字段 类型(长度) 约束 first;
alter table 表名 modify 字段 类型(长度) 约束 after 字段名;

# 增加字段
1）在末尾增加
mysql>: alter table 表名 add 字段名 类型[(长度) 约束];  # 末尾
eg>: alter table tf1 add z int unsigned;

2）在首位增加
mysql>: alter table 表名 add 字段名 类型[(宽度) 约束] first;  # 首位
eg>: alter table tf1 add a int unsigned first;

3）在指定字段后面添加字段
mysql>: alter table 表名 add 字段名 类型[(宽度) 约束] after 旧字段名;  # 某字段后
eg>: alter table tf1 add xx int unsigned after x;
# 删除字段
mysql>: alter table 表名 drop 字段名;  
eg>: alter table tf1 drop a;
```



```mysql
#例子：
#创建表
mysql> create table t2(id int primary key auto_increment,x int,y int);
Query OK, 0 rows affected (0.32 sec)
#插入值
mysql> insert into t2(x,y) values(10,20),(100,200),(1000,2000);
Query OK, 3 rows affected (0.05 sec)
Records: 3  Duplicates: 0  Warnings: 0
#查询表结构
mysql> desc t2;
+-------+---------+------+-----+---------+----------------+
| Field | Type    | Null | Key | Default | Extra          |
+-------+---------+------+-----+---------+----------------+
| id    | int(11) | NO   | PRI | NULL    | auto_increment |
| x     | int(11) | YES  |     | NULL    |                |
| y     | int(11) | YES  |     | NULL    |                |
+-------+---------+------+-----+---------+----------------+
3 rows in set (0.01 sec)

#修改字段属性
mysql> alter table t2 modify x bigint default 0;
Query OK, 3 rows affected (0.99 sec)
Records: 3  Duplicates: 0  Warnings: 0

#修改字段名和属性
mysql> alter table t2 change y m char(10) not null;
Query OK, 3 rows affected (0.79 sec)
Records: 3  Duplicates: 0  Warnings: 0
#在末尾添加字段
mysql> alter table t2 add age int,add gender enum("male","female","wasai") default "wasai";
Query OK, 0 rows affected (0.44 sec)
Records: 0  Duplicates: 0  Warnings: 0
#在开头添加字段
mysql> alter table t2 add name char(16) first;
Query OK, 0 rows affected (0.61 sec)
Records: 0  Duplicates: 0  Warnings: 0
#在name字段后面添加字段
mysql> alter table t2 add price int after name;
Query OK, 0 rows affected (0.59 sec)
Records: 0  Duplicates: 0  Warnings: 0
#查询创建表的详细信息
mysql> show create table t2 \G;
*************************** 1. row ***************************
       Table: t2
Create Table: CREATE TABLE `t2` (
  `name` char(16) DEFAULT NULL,
  `price` int(11) DEFAULT NULL,
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `x` bigint(20) DEFAULT '0',
  `m` char(10) NOT NULL,
  `age` int(11) DEFAULT NULL,
  `gender` enum('male','female','wasai') DEFAULT 'wasai',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8
#查询操作后的表的结构
mysql> desc t2;
+--------+-------------------------------+------+-----+---------+----------------+
| Field  | Type                          | Null | Key | Default | Extra          |
+--------+-------------------------------+------+-----+---------+----------------+
| name   | char(16)                      | YES  |     | NULL    |                |
| price  | int(11)                       | YES  |     | NULL    |                |
| id     | int(11)                       | NO   | PRI | NULL    | auto_increment |
| x      | bigint(20)                    | YES  |     | 0       |                |
| m      | char(10)                      | NO   |     | NULL    |                |
| age    | int(11)                       | YES  |     | NULL    |                |
| gender | enum('male','female','wasai') | YES  |     | wasai   |                |
+--------+-------------------------------+------+-----+---------+----------------+
7 rows in set (0.01 sec)
#查询表的内容
mysql> select * from t2;
+------+-------+----+------+------+------+--------+
| name | price | id | x    | m    | age  | gender |
+------+-------+----+------+------+------+--------+
| NULL |  NULL |  1 |   10 | 20   | NULL | wasai  |
| NULL |  NULL |  2 |  100 | 200  | NULL | wasai  |
| NULL |  NULL |  3 | 1000 | 2000 | NULL | wasai  |
+------+-------+----+------+------+------+--------+
3 rows in set (0.00 sec)
#删除name字段
mysql> alter table t2 drop name;
Query OK, 0 rows affected (0.50 sec)
Records: 0  Duplicates: 0  Warnings: 0
mysql> select * from t2;
+-------+----+------+------+------+--------+
| price | id | x    | m    | age  | gender |
+-------+----+------+------+------+--------+
|  NULL |  1 |   10 | 20   | NULL | wasai  |
|  NULL |  2 |  100 | 200  | NULL | wasai  |
|  NULL |  3 | 1000 | 2000 | NULL | wasai  |
+-------+----+------+------+------+--------+
3 rows in set (0.00 sec)

```

## 约束条件

|       约束条件        | 解释                                                         |
| :-------------------: | :----------------------------------------------------------- |
|      primary key      | 主键，指定该列的值可以唯一地标识该列记录,唯一标识，表都会拥有，不设置为默认找第一个 不空，唯一 字段，未标识则创建隐藏字段 |
| unique+auto_increment | 相当于primary key                                            |
|      foreign key      | 外键，指定该行记录从属于主表中的一条记录，主要用于参照完整性 |
|      unique key       | 唯一约束，唯一数据，该条字段的值需要保证唯一,不能重复        |
|    auto_increment     | 自增，只能加给key的int类型字段，作为辅助修饰，一个表中只能设置一个自增字段，清除自增（先清空表，truncate table 表名;） |
|       not null        | 非空约束，指定某列不能为空；针对一些字段，如注册时的用户名，出生人的性别等，这些需求下的字段，只不能设置为Null，必须要对其赋值 |
|        default        | 默认值 - 对有默认值意外的字段进行赋值时，有默认值的字段会被赋默认值，没默认值的系统给一个默认值null |
|       unsigned        | 无符号 - 存储的数字从0开始(主要用在整型上)。                 |
|       zerofill        | 0填充 - 存整数时数据长度小于取值范围长度，会在数字左方用0填充，主要用在整型以及浮点型)。 |



```python
# not null 与 default 限制
# 不能为空，没有默认值的x，必须赋值
# y、z在没有赋值情况下，才有默认值，设置值后，采用默认值
mysql>: create table td1 (x int not null, y int default 0, z int default 100);

# 报错，auto_increment必须设置给 键字段
mysql>: create table td2 (x int auto_increment);
# 报错，auto_increment必须设置给 int字段
mysql>: create table td2 (x char(4) auto_increment);
# 报错，auto_increment字段最多出现 1次
mysql>: create table td2 (x int unique auto_increment, y int unique auto_increment);

# 正确，主键和唯一键分析
# x为主键：没有设置primary key时，第一个 唯一自增键，会自动提升为主键
mysql>: create table td21 (x int unique auto_increment, y int unique);
# y为主键：没有设置primary key时，第一个 唯一自增键，会自动提升为主键
mysql>: create table td22 (x int unique, y int unique auto_increment);
# x为主键：设置了主键就是设置的，主键没设置自增，那自增是可以设置在唯一键上的
mysql>: create table td23 (x int primary key, y int unique auto_increment);
# x为主键：设置了主键就是设置的，主键设置了自增，自增字段只能有一个，所以唯一键不能再设置自增了
mysql>: create table td24 (x int primary key auto_increment, y int unique);
# 默认主键：没有设置主键，也没有 唯一自增键，那系统会默认添加一个 隐式主键（不可见）
mysql>: create table td25 (x int unique, y int unique);

# 唯一键：确保一个字段，数据不能重复
# 主键：是一条记录的唯一标识（可以理解为数据的编号）

# 联合唯一
# ip在port不同时，可以相同，ip不同时port也可以相同，均合法
# ip和port都相同时，就是重复数据，不合法
mysql>: create table tu1 (ip char(16), port int, unique(ip, port));

# 也可以设置成 联合主键，道理同 联合唯一
mysql>: create table tu2 (ip char(16), port int, primary key(ip, port));
# sql可以多行书写
mysql>: 
create table t22(
	ip char(16),
    port int,
    primary key(ip,port)
);



清除自增

1 .*#使用truncate清空全部数据主键自增长是从1开始(效率更高)*

truncate table 表名;
2 . #清空表数据并且将主键自增长从1开始(1.先清空表数据2.在把表的自增长设置为1)

delete from 表名;

alter table 表名 auto_increment=1;


```

### 约束例题



例题：创建一个学生student表，有主键id字段，name唯一字段、age字段、height字段、mobile字段



​	主键为自增字段

​	name和mobile联合唯一

​	age和height不可以出现负数

​	增加记录时，name和height字段必须赋值

​	增加记录时，age未赋值，用0填充，mobile未赋值，用Null填充

create table student(id int primary key auto_increment,name varchar(16) unique,age tinyint unsigned default 0,height float(255,30) unsigned not null,mobile int,unique(name,mobile));

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923204049123-1811538626.png)

​	一次性插入一条数据

insert into student(name,age,height,mobile) values('owen',18,180.0,123456789);

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923204057522-1269868664.png)

​	再一次性插入五条数据

insert into student(name,age,height,mobile) values('on',18,180.0,123456789),('chen',18,180.0,123456789),('owe',18,180.0,152456752365),('rock',18,180.0,123456789),('nick',18,180.0,123456789);

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923204103896-1972219820.png)

​	清空表，并清空主键自增记录

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923204107068-2071894019.png)



## 数据库表的引擎：驱动数据的方式-数据库优化

```python
前提: 引擎是建表是规定, 提供给表使用的, 不是数据库
1.展示所有引擎
mysql> show engines; 

# 重点: 
mysql> use db1;选择数据库
2. innodb(默认): 支持事务, 行级锁, 外键（安全性比较高，加锁）
mysql>: create table t1(id int)engine=innodb;
3. myisam: 查询效率要优于innodb, 当不需要支持事务, 行级锁, 外键, 可以通过设置myisam来优化数据库（速度比较快，无锁）
mysql>: create table t2(id int)engine=myisam;
4. blackhole：黑洞，存进去的数据都会消失（可以理解不存数据）
mysql>: create table t3(id int)engine=blackhole;

5. memory：表结构是存储在硬盘上的，但是表数据全部存储在内存中，重启清空
mysql>: create table t4(id int)engine=memory;

insert into t1 values(1);
insert into t2 values(1);
insert into t3 values(1);
insert into t4 values(1);

select * from t1; ...
```

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923164132711-1451803900.png)





![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923164134921-261812521.png)

# mysql支持的数据类型

```python
#mysql数据库支持存在那些数据
整型、浮点型、字符型、时间类型、枚举类型、集合类型

```

## 整形

| 类型         | 大小   | 范围（有符号）                                          | 范围（无符号）unsigned约束      | 用途       |
| :----------- | :----- | :------------------------------------------------------ | :------------------------------ | :--------- |
| tinyint      | 1 字节 | (-128，127)                                             | (0，255)                        | 小整数值   |
| smallint     | 2 字节 | (-32 768，32 767)                                       | (0，65 535)                     | 大整数值   |
| mediumint    | 3 字节 | (-8 388 608，8 388 607)                                 | (0，16 777 215)                 | 大整数值   |
| int或integer | 4 字节 | (-2 147 483 648，2 147 483 647)                         | (0，4 294 967 295)              | 大整数值   |
| bigint       | 8 字节 | (-9 233 372 036 854 775 808，9 223 372 036 854 775 807) | (0，18 446 744 073 709 551 615) | 极大整数值 |

```python
'''约束 *
unsigned：无符号
zerofill：0填充
'''
# 不同类型所占字节数不一样, 决定所占空间及存放数据的大小限制
# eg:
create table t8(x tinyint);
insert into t8 values(200);  # 非安全模式存入,值只能到最大值127
select (x) from t8;


'''宽度
1.不能决定整型存放数据的宽度, 超过宽度可以存放, 最终由数据类型所占字节决定
2.如果没有超过宽度,且有zerofill限制, 会用0填充前置位的不足位
3.没有必要规定整型的宽度, 默认设置的宽度就为该整型能存放数据的最大宽度 *
'''
# eg:1
create table t9(x int(5));
insert into t9 values(123456); 
select (x) from t9; # 结果: 123456
insert into t9 values(2147483648); 
select (x) from t9; # 结果: 2147483647
insert into t9 values(10); 
select (x) from t9; # 结果: 10
# eg:2
create table t10(x int(5) unsigned zerofill); # 区域0~4294967295
insert into t10 values(10); 
select x from t10; # 结果: 00010
insert into t10 values(12345678900); 
select x from t10; # 结果: 4294967295
```

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923170036498-46733705.png)

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923170038286-1749110352.png)

## 浮点型

| 类型    | 大小                                                   | 范围（有符号）                                               | 范围（无符号）unsigned约束                                   | 用途            |
| :------ | :----------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------- |
| float   | 4 字节 float(255,30)                                   | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| double  | 8 字节 double(255,30)                                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| decimal | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 double(65,30) | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

```python
# 在安全模式下测试浮点型类型

'''类型
float(M, D)：4字节，3.4E–38～3.4E+38
double(M, D)：8字节，1.7E–308～1.7E+308
decimal(M, D)：所在字节M，D大值基础上+2，其实就是M值+2就是decimal字段所占字节数
'''
'''宽度：
限制存储宽度
(M, D) => M为位数，D为小数位，M要大于等于D（一共M位，小数位是D位）。
float(255, 30)：精度最低，最常用
double(255, 30)：精度高，占位多
decimal(65, 30)：字符串存，全精度
'''

# 建表：
mysql>: create table tb4 (age float(256, 30)); # Display width out of range for column 'age' (max = 255)
mysql>: create table tb5 (age float(255, 31)); # 超出范围，Too big scale 31 specified for column 'age'. Maximum is 30.
mysql>: create table tb5 (age float(65, 30)); # 在合理取值范围
    
mysql>: create table t12 (x float(255, 30));
mysql>: create table t13 (x double(255, 30));
mysql>: create table t14 (x decimal(65, 30));
# 1.111111164093017600000000000000 
mysql>: insert into t12 values(1.11111111111111111119);  
# 1.111111111111111200000000000000
mysql>: insert into t13 values(1.11111111111111111119);
# 1.111111111111111111190000000000
mysql>: insert into t14 values(1.11111111111111111119);

# 重点：长度与小数位分析
# 报错，总长度M必须大于等于小数位D
mysql>: create table t14 (x decimal(2, 3));

# 能存储 -0.999 ~ 0.999，超长度的小数位会才有四舍五入，0.9994可以存，就是0.999,0.9995不可以存
mysql>: create table t14 (x decimal(3, 3));  # 整数位 3 - 3，所以最大为0

# 能存储 -9.999 ~ 9.999，超长度的小数位会才有四舍五入，9.9994可以存，就是9.999,9.9995不可以存
mysql>: create table t14 (x decimal(4, 3));  # 整数位 4 - 3，所以最大为9

# 能存储 -99.999 ~ 99.999，超长度的小数位会才有四舍五入，99.9994可以存，就是99.999,99.9995不可以存
mysql>: create table t14 (x decimal(5, 3));  # 整数位 5 - 3，所以最大为99

```

举例验证：

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923222408992-796454696.png)

## 字符串：数据库优化-char效率要高于varchar

| 类型            | 大小                | 用途                            |
| :-------------- | :------------------ | :------------------------------ |
| char(常用)      | 0-255字节           | 定长字符串                      |
| varchar（常用） | 0-65535 字节        | 变长字符串                      |
| tinyblob        | 0-255字节           | 不超过 255 个字符的二进制字符串 |
| tinytext        | 0-255字节           | 短文本字符串                    |
| blob            | 0-65 535字节        | 二进制形式的长文本数据          |
| text            | 0-65 535字节        | 长文本数据                      |
| mediumblob      | 0-16 777 215字节    | 二进制形式的中等长度文本数据    |
| mediumtext      | 0-16 777 215字节    | 中等长度文本数据                |
| longblob        | 0-4 294 967 295字节 | 二进制形式的极大文本数据        |
| longtext        | 0-4 294 967 295字节 | 极大文本数据                    |

```python
'''类型
char：定长，永远采用设置的长度存储数据
varchar：不定长，在设置的长度范围内，变长的存储数据
'''
'''宽度
限制存储宽度
char(4)：存 "a" "ab" "abc" "abcd"都采用4个长度，"abcde" 只能存储前4位(安全模式下报错)
varchar(4)：存 "a" "ab" "abc" "abcd"分别采用1,2,3,4个长度存储，"abcde" 只能存储前4位(安全模式下报错)

char就按定长存储，如果数据长度变化大，通常更占空间，但是存取数据按固定定长操作，效率高
varchar存储数据时，会先计算要存储数据的长度，动态变长存储数据，所以一般较省空间，但是计算是需要耗时的，所以效率低

varchar计算出的数据长度信息也是需要开辟空间来存储，存储在数据头（数据开始前）中，也需要额外消耗1~2个字节
所以如果数据都是固定长度，或是小范围波动，char相比就不会更占空间，且效率高
'''

# 建表：
mysql>: create table ts1 (s1 char(4), s2 varchar(4));
mysql>: insert into ts1 values('adcde', 'xyzabc');  # 'adcd', 'xyza'
```



## 时间类型

每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。

TIMESTAMP类型有专有的自动更新特性，将在后面描述。

| 类型                                  | 大小 (字节) | 范围                                                         | 格式                | 用途                     |
| :------------------------------------ | :---------- | :----------------------------------------------------------- | :------------------ | :----------------------- |
| date（年月日）                        | 3           | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 年月日                   |
| time（小时分钟秒）                    | 3           | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时分秒                   |
| year（年）                            | 1           | 1901/2155                                                    | YYYY                | 年份值                   |
| datatime（年月日，小时分钟秒）        | 8           | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD\HH:MM:SS | 年月日时分秒             |
| timestamp（不赋值的时候，是系统时间） | 4           | 1970-01-01 00:00:00/2038 结束时间是第 2147483647 秒，北京时间 2038-1-19 11:14:07，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYYMMDD\HHMMSS     | 混合日期和时间值，时间戳 |

```python
'''类型
year：yyyy(1901/2155)
date：yyyy-MM-dd(1000-01-01/9999-12-31)
time：HH:mm:ss
datetime：yyyy-MM-dd HH:mm:ss(1000-01-01 00:00:00/9999-12-31 23:59:59)
timestamp：yyyy-MM-dd HH:mm:ss(1970-01-01 00:00:00/2038-01-19 ??)
'''

# 建表：
mysql>: create table td1 (my_year year, my_date date, my_time time);
mysql>: insert into td1 values(1666, '8888-8-8', '8:8:8');  # 时间需要在取值访问内

mysql>: create table td2 (my_datetime datetime, my_timestamp timestamp);   
mysql>: insert into td2 values('2040-1-1 1:1:1', '2040-1-1 1:1:1');  # 时间需要在取值访问内   
mysql>: insert into td2(my_datetime) values('2040-1-1 1:1:1');  # timestamp不复制会才有系统当前时间

# datetime：8字节，可以为null
# timestamp：4字节，有默认值CURRENT_TIMESTAMP
```

举例：year,date,time

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923224134342-2021656893.png)

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923224135961-1852332461.png)

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923224137632-638995559.png)

举例:timestamp

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923224733790-209923219.png)

```python
mysql> insert into td4 values (19700101080001);
Query OK, 1 row affected (0.00 sec)

mysql> select * from td4;
+---------------------+
| t1                  |
+---------------------+
| 1970-01-01 08:00:01 |
+---------------------+
row in set (0.00 sec)
# timestamp时间的下限是19700101080001
mysql> insert into td4 values (19700101080000);
ERROR 1292 (22007): Incorrect datetime value: '19700101080000' for column 't1' at row 1

mysql> insert into td4 values ('2038-01-19 11:14:07');
Query OK, 1 row affected (0.00 sec)
# timestamp时间的上限是2038-01-19 11:14:07
mysql> insert into td4 values ('2038-01-19 11:14:08');
ERROR 1292 (22007): Incorrect datetime value: '2038-01-19 11:14:08' for column 't1' at row 1
mysql> 

#添加一列 默认值是'0000-00-00 00:00:00'
mysql> alter table td4 add id2 timestamp;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> show create table td4 \G;
*************************** 1. row ***************************
       Table: td4
Create Table: CREATE TABLE `td4` (
  `id1` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `id2` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
row in set (0.00 sec)

ERROR: 
No query specified

# 手动修改新的列默认值为当前时间
mysql> alter table td4 modify id2 timestamp default current_timestamp;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> show create table td4 \G;
*************************** 1. row ***************************
       Table: td4
Create Table: CREATE TABLE `td4` (
  `id1` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `id2` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8
row in set (0.00 sec)

ERROR: 
No query specified

mysql> insert into td4 values (null,null);
Query OK, 1 row affected (0.01 sec)

mysql> select * from td4;
+---------------------+---------------------+
| id1                 | id2                 |
+---------------------+---------------------+
| 2018-09-21 14:56:50 | 0000-00-00 00:00:00 |
| 2018-09-21 14:59:31 | 2018-09-21 14:59:31 |
+---------------------+---------------------+
rows in set (0.00 sec)
```

举例：datetime

![](https://img2018.cnblogs.com/blog/1739658/201909/1739658-20190923230552188-467869800.png)

## 枚举与集合

enum中文名称叫枚举类型，它的值范围需要在创建表时通过枚举方式显示。enum只允许从值集合中选取单个值，而不能一次取多个值。

set和enum非常相似，也是一个字符串对象，里面可以包含0-64个成员。根据成员的不同，存储上也有所不同。set类型可以允许值集合中任意选择1或多个元素进行组合。对超出范围的内容将不允许注入，而对重复的值将进行自动去重。

| 类型 | 大小                                                         | 用途           |
| :--- | :----------------------------------------------------------- | :------------- |
| enum | 对1-255个成员的枚举需要1个字节存储; 对于255-65535个成员，需要2个字节存储; 最多允许65535个成员。 | 单选：选择性别 |
| set  | 1-8个成员的集合，占1个字节 9-16个成员的集合，占2个字节 17-24个成员的集合，占3个字节 25-32个成员的集合，占4个字节 33-64个成员的集合，占8个字节 | 多选：兴趣爱好 |

```python
# 枚举与集合：为某一个字段提供选项的 - 枚举只能单选(1个)，集合可以多选(0-n个)

# 建表
# enum、set默认值为NULL
mysql>: create table tc1 (name varchar(20), sex enum('男', '女', '哇塞'), hobbies set('男', '女', '哇塞'));
mysql>: insert into tc1 values('ruakei', '哇塞哇塞', '未知');  
    
# enum、set手动设置默认值 '男' 与 '哇塞'
mysql>: create table tc2 (name varchar(20), sex enum('男', '女', '哇塞') default '男', hobbies set('男', '女', '哇塞') default '哇塞');
mysql>: insert into tc2 values('ruakei', '哇塞哇塞', '未知');  
mysql>: insert into tc2(name) values('ruakei');
    
# 对sex、hobbies两个字段赋值错误，系统默认用空字符串填充(非安全模式)，安全模式抛异常
# 如果对出sex、hobbies两个字段外的其他字段进行赋值，这两个字段会才有默认值

# 注：对set类型的字段进行赋值，用一个字符串，字符串内部用,将多个选项隔开，且不能添加空格等其他额外字符
mysql>: insert into tc2 values('ruakei_1', '女', '男,女,哇塞'); 
    
    
    
    
 结果：

mysql> select * from tc2;
+----------+------+----------------+
| name     | sex  | hobbies        |
+----------+------+----------------+
| ruakei   | 男   | 哇塞           |
| ruakei   | 男   | 哇塞           |
| ruakei_1 | 女   | 男,女,哇塞     |
+----------+------+----------------+
3 rows in set (0.03 sec)
```

