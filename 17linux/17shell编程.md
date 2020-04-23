# shell编程

## 1 shell编程是个啥

Shell
是一个命令行解释器，它为用户提供了一个向 Linux 内核发送请求以便运行程序的系统级程序

**画图说明**



## 2 shell编程打印hello world

### 2.1 代码部分

```
#!/bin/bash
echo 'hello world'
```

代码解释:

1.#!/bin/bash:

​	告诉计算机,使用bash解释器来执行代码

2.echo:

​	控制台输出



### 2.2 执行代码

#### 方式一:

给脚本可执行权限

```
chmod 744 myshell.sh
```

然后直接运行脚本



#### 方式二:(不推荐)

直接调用shell解释器执行

```
sh myshell.sh
```



## 3 注释

### 单行注释

```
#内容
```

### 多行注释

```
:<<!
内容
!
```





## 4 变量

### 4.1 变量的介绍

```
1.Linux中变量的分类:系统变量	自定义变量
2.系统变量:
	$PATH
	$HOME
	$PWD
	$SHELL
	$USER
3.显示当前shell中所有的变量:set
```



### 4.2 变量的定义

#### 基本语法

```
1.定义变量:变量名=变量值
2.撤销变量:unset 变量名
3.声明静态变量:readonly 变量名.		静态变量不能unset
```

#### 快速入门

```
1.定义变量a
2.撤销变量a
3.声明静态变量b=2,尝试unset撤销
```



#### 定义规则

```
1.变量名称可以由字母、数字和下划线组成，但是不能以数字开头
2.等号两侧不能有空格
3.变量名称一般习惯为大写
```



#### 将命令的返回值赋给变量

```
1.A=`ls -la` 反引号，运行里面的命令，并把结果返回给变量 A
2.A=$(ls -la) 等价于反引号
```



## 5 设置环境变量

### 基本语法

```
1.export 变量名=变量值		将shell变量输出给环境变量
2.source 配置文件			 让修改后的配置信息立即生效
3.echo $变量值				  查看环境变量的值
```



### 快速入门

```
1.在/etc/profile文件中定义MY_NAME环境变量
2.查看环境变量MY_NAME的值

强调:在使用MY_NAME前,需要让其生效
3.source /etc/profile

4,在另外一个shell程序中使用MY_NAME

```





## 6 位置参数变量

### 介绍

当我们执行一个 shell
脚本时，如果希望获取到命令行的参数信息，就可以使用到位置参数变量



### 基本语法

```
1.
$n （功能描述：n 为数字，$0 代表命令本身，$1-$9 代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如${10}）

2.
$* （功能描述：这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体）

3.
$@  （功能描述：这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待）

4.
$#（功能描述：这个变量代表命令行中所有参数的个数）

```



### 快速入门

```
编写一个shell脚本,pasition.sh,在脚本中获取到命令行的各个参数信息
```





## 7 预定义变量

### 介绍

就是 shell 设计者事先已经定义好的变量，可以直接在
shell 脚本中使用



### 基本语法

```
$$ 	（功能描述：当前进程的进程号（PID））
$!	（功能描述：后台运行的最后一个进程的进程号（PID））
$?	（功能描述：最后一次执行的命令的返回状态。如果这个变量的值为 0，证明上一个命令正确执行；如果这个变量的值为非 0（具体是哪个数，由命令自己来决定），则证明上一个命令执行失败)
```



### 快速入门

```
在一个shell脚本pre.sh中简单实用一下预定义变量(提示, ./myshell.sh &  后台运行myshell.sh)
```





##8  运算符

### 基本语法

```
1.$((运算式))
2.$[运算式]
3.`expr m + n`
	特点:运算符之间要有空格
	+
	-
	/
	%
	\*
	\(	\)
	
```



### 快速入门

```
写一个demo.sh完成:
1.3种方式计算(2+3)*4的值
2.方式2求出命令行两个参数的和
```





## 9 判断

### 基本语法

```
[ 条件 ]			注意:条件前后要有空格

特别的:
[ 非空 ]	  为true
[]		   为false
[ haha ] && echo true || echo false
```



### 判断语句

#### 字符串比较

```
=	判等
!=  判不相等
```

#### 整数比较

```
-lt		小于
-le		小于等于
-gt		大于
-ge		大于等于
-eg		等于
-ne		不等于
```

#### 文件权限判断

```
-r	有读的权限	[ -r 文件 ]
-w	有写的权限
-x	有执行权限
```

#### 文件类型判断

```
-f 存在并且是一般文件	[-f 文件]
-e 文件存在
-d 存在并且是一个目录
```



### 快速入门

```
1.'ok'是否等于'ok'
2.'ok100' 是否等于 'ok'
```



```
3.23 是否大于 23
4.23 是否大于等于 23
```



```
5./root是否存在
6./root是否是一般文件
```





## 10 流程控制

### 10.1 if判断

#### 基本语法

```
if [ 条件 ]
then
	代码
fi
```

```
if [ 条件 ]
then
	代码
else
	代码
fi
```

```
if [ 条件 ]
then
	代码
elif [ 条件 ]
then
	代码
else
	代码
fi
```



#### 快速入门

```
编写shell脚本,if.sh:
如果输入参数,大于等于60,则输出'及格了',如果小于60,则输出'不及格'
```





### 10.2 case 选择分支

#### 基本语法(相当诡异,令人发指,what a fuck)

```
case $变量名 in
'值1')
代码
;;
'值2')
代码
;;
*)
代码					都没命中执行
;;
esac
```



#### 快速入门

```
编写shell脚本,case.sh:
当命令行参数是1时,输出'周一';是2时,输出'周二',是3时,输出'周三',其它情况,输出'其它'
```





### 10.3 for循环

#### 遍历

##### 基本语法

```
for 变量 in 值1 值2 值3
do
	代码
done
```

##### 快速入门

```
编写foreach.sh:
打印命令行输入的参数[这里可以看出$*和$@的区别]
```



#### 循环

##### 基本语法

```
for ((初始值;循环条件;变量变化))
do
	代码
done
```

##### 快速入门

```
编写for.sh:
从1加到100,并输出结果
```



### 10.4 while循环

#### 基本语法

```
while [ 条件 ]
do
	代码
done
```



#### 快速入门

```
编写while.sh:
从命令行中输出一个数n,统计1+...+n的值是多少
```





## 11 与用户交互

### 基本语法

```
read 选项 变量
选项:
-p:提示信息
-t:等待输入的时间
```

### 快速入门

```
编写input.sh:
1.读取控制带输入的值
	read -p "请输入名字:" name
	echo '$name'
2.读取控制台输入的值,等待6秒
	read -p "请输入名字:" -t 6 name
	echo '$name'
```





## 12 函数

### 12.1 系统函数

#### basename

##### 基本语法

```
basename [pathname] [suffix]
获得路径最后一部分

如果指定的suffix,那么会去掉结果中suffix的部分
```

##### 快速入门

```
1.返回/home/aaa/test.txt中'test.txt'的部分
	basename /home/aaa/test.txt
2.返回/home/aaa/test.txt中'test'的部分
	basename /home/aaa/test.txt .txt
```



#### dirname

##### 基本语法

```
dirname [pathname]
获得基础路径
```

##### 快速入门

```
1.返回/home/aaa/test.txt中'/home/aaa'的部分
```



### 12.2 自定义函数

#### 基本语法

```
function 函数名(){
	代码;
	#参数使用:$1,$2,...,${10}...
        	echo ''   #返回结果
	return  #0-255 返回函数执行的状态
}

调用:
函数名 值1 值2
```



#### 快速入门

```
编写func.sh:
用函数的形式,计算两个参数的和
```





## 13 shell综合案例

备份数据库

mysqldump -uroot -proot –host=localhost 要备份的数据库名字

```
在/root下编写mysql_db_backuo.sh

需求:
1.每天凌晨2点10分,备份数据库mydb  到/data/backup/db
2.备份开始和备份结束时能够给出提示信息
3.备份后的文件要求以备份时间为文件名,并打包成.tar.gz的形式,如2019-09-28-044403.tar.gz
4.在备份的同时,检查是否有10天前的备份文件,如果有就删除
```

```
#!/bin/bash

#备份的路径
BACKUP=/data/backup/db
#当前的时间作为文件名
DATETIME=$(date +%Y_%m_%d_%H%M%S)

echo "=======开始备份======"
echo "=====备份的路径是 $BACKUP/$DATETIME.tar.gz"

#主机
HOST=localhost
#用户名
DB_USER=root
#密码
DB_PWD=997997
#备份的数据库
DATABASE=mydb



如果备份路径不存在,就创建
[ ! -d "$BACKUP/DATETIME" ] && mkdir -p "$BACKUP/$DATETIME"
#执行mysql的备份指令
mysqldump -u$DB_USER -p$DB_PWD --host=$HOST $DATABASE | gzip > $BACKUO/$DATETIME/$DATETIME.sql.gz
#打包备份文件
cd $BACKUP
tar -zcvf $DATETIME.tar.gz $DATETIME
#删除临时目录
rm -rf $BACKUP/$DATETIME


#删除10天前的文件
find $BACKUP -mtime +10 --name "*.tar.gz" -exec rm -rf {} \;

echo "=====备份成功+++++"
```

vim

```
#! /bin/bash
USER="root"
PWD="root"
DB="mydb"
HOST="localhost"
:<<!方法1！
DATE=`date +%Y_%m_%d_%H%M`
:<<!方法2!
#DATE=$(date +%Y_%m_%d_%H%M)
BACKUP_DIR="/data/backup/db"



echo "============备份开始============"

echo "============备份路径:$BACKUP_DIR/$DATE.tar.gz============="

:<<! #方法1
if [! -e $BACKUP_DIR/$DATE ]
then
	mkdir -p $BACKUP_DIR
fi

!

:<<!方法二！
[! -e $BACKUP_DIR/$DATE ] && mkdir -p $BACKUP_DIR


mysqldump -u$USER -p$PWD --host=$HOST $DB | gzip > $BACKUP_DIR/$DATE/$DATE.sql.gz



cd $BACKUP_DIR
tar -zcvf $DATE.tar.gz $DATE
rm -rf $DATE
echo "===========备份成功====="

:<<!exec 将前面的结果返回到后面进行执行!


find $BACKUP_DIR -ctime +10 -name "*.tar.gz" -exec rm -rf {} \;

echo "========过期文献删除成功=========="








```



