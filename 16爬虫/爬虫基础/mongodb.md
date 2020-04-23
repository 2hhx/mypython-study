
#管理员
user admin
创建用户：
db.createUser({user:"yxp",pwd:"997997",roles:["root"]})
连接数据库：
mongo -u yxp -p 997997 --authenticationDatabase admin
查看数据库
show dbs
切换数据库
use xxx
没有创建,插入数据（创建）
db.table1.insert({"name":"haha"})
删除数据库
db.dropDatabase()
查看表
show tables
创建表
 db.table2.insert({"name":"mac"})
删除表
db.table2.drop()
表中插入数据
插入单行
db.table1.insert({"name":"rocky"})
插入多行
db.table1.insertMany({"name":"rocky"},{"name":"nick"})
查找表中数据
db.table1.find({"name":"haha"})
查找表中不是自己要的数据
db.table1.find({"name":{"$ne":"haha"}})
更改数据
db.table1.update({"name":"haha"},{"name":"lin"})

# 常用数据库

###mongoDB4.0:

下载:https://www.mongodb.com/

安装:略

**注意:**使用前修改bin目录下配置文件mongodb.cfg,删除最后一行的'mp'字段

####1. 启动服务与终止服务

```
net start mongodb

net stop mongodb
```

## 2创建管理员用户

```
mongo

use admin

db.createUser({user:"yxp",pwd:"997997",roles:["root"]})
```

## 3.使用账户密码连接mongodb

```
mongo -u adminUserName -p userPassword
```

## 4.数据库

### 查看数据库

```
show dbs
```

### 切换数据库

```
use db_name
```

### 增加数据库

```
db.table1.insert({'a':1})  创建数据库(切换到数据库插入表及数据)
```

### 删除数据库

```
db.dropDatabase()  删数据库(删前要切换)
```

## 5.表

```
使用前先切换数据库
```

### 查看表

```
show tables 查所有的表
```

### 增加表

```
db.table1.insert({'b':2})  增加表(表不存在就创建)
```

### 删除表

```
db.table1.drop()    删表
```

## 数据

### 增加数据

```
db.test.insert(user0)    插入一条
db.user.insertMany([user1,user2,user3,user4,user5])   插入多条
```

### 删除数据

```
db.user.deleteOne({ 'age': 8 })   删第一个匹配
db.user.deleteMany( {'addr.country': 'China'} )  删全部匹配
db.user.deleteMany({})  删所有
```

### 查看数据

```
db.user.find({'name':'alex'})   查xx==xx
db.user.find({'name':{"$ne":'alex'}})   查xx!=xx
db.user.find({'_id':{'$gt':2}})    查xx>xx
db.user.find({"_id":{"$gte":2,}})  查xx>=xx
db.user.find({'_id':{'$lt':3}})  查xx<xx
db.user.find({"_id":{"$lte":2}})  查xx<=xx
```

### 改数据

```
db.user.update({'_id':2},{"$set":{"name":"WXX",}})   改数据
```

## pymongo

```
conn = pymongo.MongoClient(host=host,port=port, username=username, password=password)
db = client["db_name"] 切换数据库
table = db['表名']
table.insert({})  插入数据
table.remove({})   删除数据
table.update({'_id':2},{"$set":{"name":"WXX",}})   改数据
table.find({})  查数据
```

# 

