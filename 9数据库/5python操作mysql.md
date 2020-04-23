# pymysql：python操作mysql

## 安装pymysql

```
>: pip3 install pymysql
```



## 增删改查

```python
# 选取操作的模块 pymysql

# pymysql连接数据库的必要参数：主机、端口、用户名、密码、数据库
# 注：pymysql不能提供创建数据库的服务，数据库要提前创建
import pymysql

# 1）建立数据库连接对象 conn
# 2）通过 conn 创建操作sql的 游标对象
# 3）编写sql交给 cursor 执行
# 4）如果是查询，通过 cursor对象 获取结果
# 5）操作完毕，端口操作与连接


# 1）建立数据库连接对象 conn
conn = pymysql.connect(user='root', passwd='root', database='oldboy')
# conn = pymysql.connect(user='root', passwd='root', database='oldboy', autocommit=True)

# 2）通过 conn 创建操作sql的 游标对象
# 注：游标不设置参数，查询的结果就是数据元组，数据没有标识性
# 设置pymysql.cursors.DictCursor，查询的结果是字典，key是表的字段
cursor = conn.cursor(pymysql.cursors.DictCursor)

# 3）编写sql交给 cursor 执行
```

## 创建表

```python
# 创建表
sql1 = 'create table t1(id int, x int, y int)'
cursor.execute(sql1)
```

## 增

```python
sql2 = 'insert into t1 values(%s, %s, %s)'

# 增1
cursor.execute(sql2, (1, 10, 100))
cursor.execute(sql2, (2, 20, 200))
# 重点：在创建conn对象时，不设置autocommit，默认开启事务，增删改操作不会直接映射到数据库中，
# 需要执行 conn.commit() 动作
conn.commit()

# 增多
cursor.executemany(sql2, [(3, 30, 300), (4, 40, 400)])
conn.commit()
```

## 删

```python
sql3 = 'delete from t1 where id=%s'
cursor.execute(sql3, 4)
conn.commit()
```

## 改

```python
sql4 = 'update t1 set y=666 where id=2'
cursor.execute(sql4)
conn.commit()
```

## 查

```python
sql5 = 'select * from t1'
row = cursor.execute(sql5)  # 返回值是受影响的行
print(row)

# 4）如果是查询，通过 cursor对象 获取结果
# fetchone() 偏移一条取出，fetchmany(n) 偏移n条取出，fetchall() 偏移剩余全部
r1 = cursor.fetchone()
print(r1)
r2 = cursor.fetchone()
print(r2)
r3 = cursor.fetchmany(1)
print(r3)
r4 = cursor.fetchall()
print(r4)
```

```python
# 5）操作完毕，端口操作与连接
cursor.close()
conn.close()
```





# 游标操作

```python
import pymysql
from pymysql.cursors import DictCursor

# 1）建立数据库连接对象 conn
conn = pymysql.connect(user='root', passwd='root', db='oldboy')
# 2）通过 conn 创建操作sql的 游标对象
cursor = conn.cursor(DictCursor)
# 3）编写sql交给 cursor 执行
sql = 'select * from t1'
# 4）如果是查询，通过 cursor对象 获取结果
row = cursor.execute(sql)
if row:
    r1 = cursor.fetchmany(2)
    print(r1)

    # 操作游标
    # cursor.scroll(0, 'absolute')  # absolute绝对偏移，游标重置，从头开始偏移
    cursor.scroll(-2, 'relative')  # relative相对偏移，游标在当前位置进行左右偏移

    r2 = cursor.fetchone()
    print(r2)

# 5）操作完毕，端口操作与连接
cursor.close()
conn.close()
```



# pymysql事务

```python
import pymysql
from pymysql.cursors import DictCursor
conn = pymysql.connect(user='root', passwd='root', db='oldboy')
cursor = conn.cursor(DictCursor)

try:
    sql = 'create table t2(id int, name char(4), money int)'
    row = cursor.execute(sql)
    print(row)
except:
    print('表已创建')
    pass


```

```python
# 空表才插入
row = cursor.execute('select * from t2')
if not row:
    sql = 'insert into t2 values(%s,%s,%s)'
    row = cursor.executemany(sql, [(1, 'tom', 10), (2, 'Bob', 10)])
    conn.commit()


```

```python

# 可能会出现异常的sql
"""
try:
    sql1 = 'update t2 set money=money-1 where name="tom"'
    cursor.execute(sql1)
    sql2 = 'update t2 set moneys=money+1 where name="Bob"'
    cursor.execute(sql2)
except:
    print('转账执行异常')
    conn.rollback()
else:
    print('转账成功')
    conn.commit()
"""


```

解决

```python
try:
    sql1 = 'update t2 set money=money-1 where name="tom"'
    r1 = cursor.execute(sql1)
    sql2 = 'update t2 set money=money+1 where name="ruakei"'  # 转入的人不存在
    r2 = cursor.execute(sql2)
except:
    print('转账执行异常')
    conn.rollback()
else:
    print('转账没有异常')
    if r1 == 1 and r2 == 1:
        print('转账成功')
        conn.commit()
    else:
        conn.rollback()
```



# sql注入问题和解决

```python
import pymysql
from pymysql.cursors import DictCursor
conn = pymysql.connect(user='root', passwd='root', db='oldboy')
cursor = conn.cursor(DictCursor)

try:
    sql = 'create table user(id int, name char(4), password char(6))'
    row = cursor.execute(sql)
    print(row)
except:
    print('表已创建')
    pass




```

```python
# 空表才插入
row = cursor.execute('select * from user')
if not row:
    sql = 'insert into user values(%s,%s,%s)'
    row = cursor.executemany(sql, [(1, 'tom', '123'), (2, 'bob', 'abc')])
    conn.commit()





```

```python
# 用户登录
usr = input('usr: ')
pwd = input('pwd: ')

# 自己拼接参数一定有sql注入，将数据的占位填充交给pymysql

"""
sql = 'select * from user where name="%s" and password="%s"' % (usr, pwd)
row = cursor.execute(sql)
if row:
    print('登录成功')
else:
    print('登录失败')
"""

# 知道用户名时
# 输入用户时：
#   tom => select * from user where name="tom" and password="%s"
#   tom" # => select * from user where name="tom" #" and password="%s"

# 不自定义用户名时
#   " or 1=1 # => select * from user where name="" or 1=1 #" and password="%s"


解决注入问题

sql = 'select * from user where name=%s and password=%s'
row = cursor.execute(sql, (usr, pwd))
if row:
    print('登录成功')
else:
    print('登录失败')
```



# 索引

索引就是 键 - key

1）键 是添加给数据库表的 字段 的
2）给表创建 键 后，该表不仅会形参 表结构、表数据，还有 键的B+结构图
3）键的结构图是需要维护的，在数据完成增、删、改操作时，只要影响到有键的字段，结构图都要维护一次
    所以创建键后一定会降低 增、删、改 的效率
4）键可以极大的加快查询速度(开发需求中，几乎业务都和查有关系)
5）建立键的方式：主键、外键、唯一键、index

```python
import pymysql
from pymysql.cursors import DictCursor
conn = pymysql.connect(user='root', passwd='root', db='oldboy')
cursor = conn.cursor(DictCursor)

# 创建两张表
# sql1 = """create table a1(
#     id int primary key auto_increment,
#     x int,
#     y int
# )"""
# cursor.execute(sql1)
# sql2 = """create table a2(
#     id int primary key auto_increment,
#     x int,
#     y int,
#     index(x)###############索引，对x的索引，增加索引，提高查询率。
# )"""
# cursor.execute(sql2)

# 每个表插入5000条数据
# import random
# for i in range(1, 5001):
#     x = i
#     y = random.randint(1, 5000)
#     cursor.execute('insert into a1(x, y) values(%s, %s)', (x, y))
#     cursor.execute('insert into a2(x, y) values(%s, %s)', (x, y))
#
# conn.commit()

import time
# a1的x、a1的id、a2的x
b_time = time.time()
sql = 'select * from a1 where id=4975'
cursor.execute(sql)
e_time = time.time()
print(e_time - b_time)

b_time = time.time()
sql = 'select * from a1 where x=4975'
cursor.execute(sql)
e_time = time.time()
print(e_time - b_time)

b_time = time.time()
sql = 'select * from a2 where x=4975'
cursor.execute(sql)
e_time = time.time()
print(e_time - b_time)


```

