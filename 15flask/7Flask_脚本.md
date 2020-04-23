# 06-Flask_脚本



# 1 集成Python shell

在我们实际的开发中，不免有一些任务需要在`shell`下完成。比如为`cms`后台添加超级管理员的需求，又比如迁移数据库的需求，定时任务等等，诸如这类需求更适合在shell中去操作（大部分需要在shell中去操作的都是权限比较高的任务）。



_提示：迁移数据库就是用来解决数据库更新问题，解决之前我们学的`db.create_all()`和`db.drop_all()`更新数据库的时候丢失数据的问题。_

flask官方提供了一个扩展组件`flask-script`可以实现在`shell`下操作我们的`Flask`项目。

## 1.1 flask-script的用法：

1 由于`flask-script`是Flask的一个扩展组件，同往常一样首先在虚拟环境中安装我们的`flask_script`包。


​    
​    pip install flask-script    

### **1.1.1 实例：flask-script的简单实现**

_提示：实例下面有讲解_

项目目录


​    
​    │  manage.py
​    │  server.py
​    │
​    ├─static  # 文件夹
​    ├─templates # 文件夹

server.py


​    
​    from flask import Flask
​    
    app = Flask(__name__)


​    
​    @app.route('/')
​    def hello_world():
​        return 'Hello World!'


​    
​    if __name__ == '__main__':
​        app.run()


manage.py


​    
​    from flask_script import Manager
​    from server import app
​    
    manager = Manager(app)
    
    @manager.command
    def hello():
        print('hello world')


​    
​    if __name__ == '__main__':
​        manager.run()


**解读：manage.py**

**（1）** 从`flask_script`模块中导入`flask_script`的核心类`Manager`


​    
​    from flask_script import Manager 

**（2）** 从`server.py`模块中把`app`对象导入


​    
​    from server import app

**（3）** 从`Manager()`类传入`app`对象实例化出`manager`对象，`manager`对象用于以后所有添加命令相关操作


​    
​    manager = Manager(app)

**（4）** 利用`@manager.command`装饰器添加`以被装饰函数的名字命名的一条命令`与`被装饰函数的映射`


​    
​    @manager.command   # 相当于添加了一条hello命令，可以调用到hello函数
​    def hello():
​        print('hello world')

**（5）**`manager`调用`run`方法之前定义的命令才会生效


​    
​    if __name__ == '__main__':
​        manager.run()

**在`shell`下操作命令**

在`shell`中切入到该`manage.py`的目录下，并且进入虚拟环境。输入命令`python manage.py hello`


​    
​    >>python manage.py hello

命令中的`hello`是`@manager.command`装饰器装饰的函数名

执行命令后会调用`hello`函数

如图所示实现了调用`hello`函数

![1549965903321](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155049684-1372091286..png)



### 1.1.1命令添加方式：

#### 第一种（无参命令）：

使用`manager.commad`方式添加命令：


​    
​    ...
​    @manager.command
​    def demo():
​        print('无参命令')
​    ...

切入到manage.py所在的目录中，切入到虚拟环境，执行如下命令


​    
​    >>python manage.py demo

![1549967813932](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155050107-2129786014..png)

#### 第二种（有参命令）:

使用`manager.option('-简写的命令'，‘--全写的命令’，dest=‘传给函数的形参’)`添加命令：


​    
​    ...
​    @manager.option("-u","--username",dest="username")
​    @manager.option("-p","--password",dest="password")
​    def login(username, password):
​        print("用户名:{}  密码: {}".format(username,password))
​    ...

切入到manage.py所在的目录中，切入到虚拟环境，执行如下命令.


​    
​    >>python manage.py login -u mark -p 123

![1549968703583](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155050295-237683571..png)

#### 第三种（子命令）：

比如一个功能对应着很多个命令，这个时候就可以用子命令来实现，可以将这些命令的映射单独放到一个文件方便管理。在这个放着很多命令映射的文件中实例化Manager类出一个新的对象，并在`manage.py`文件中通过`manager.add_command("子命令",Manager对象)`来添加子命令



**实例：**

在之前的1.1.1实例的项目目录中新建文件`db_script.py`


​    
​    │  manage.py
​    │  server.py
​    │  db_script.py
​    │
​    ├─static  # 文件夹
​    ├─templates # 文件夹

db_script.py


​    
​    from flask_script import Manager
​    
    db_Manager = Manager()
    
    @db_Manager.command
    def init():
        print('初始迁移仓库')
    
    @db_Manager.command
    def migrate():
        print('生成迁移脚本')
    
    @db_Manager.command
    def upgrade():
        print("迁移脚本映射到数据库")


manage.py


​    
​    from flask_script import Manager
​    from server import app
​    from db_script import db_Manager # 导入子命令文件的Manager类实例化出的对象
​    manager = Manager(app)
​    
    manager.add_command("db",db_Manager) # 添加子命令
    
    ...

切入到manage.py所在的目录中，切入到虚拟环境，执行如下命令.


​    
​    python manage.py db init
​    python manage.py db migrate
​    python manage.py db upgrade

![1549969697643](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155050820-1276211128..png)



# 2 项目重构

## 2.1 解耦配置信息以及模型文件信息触发循环导入问题

随着项目代码的增多
我们再把连接数据库的信息放到主`app`文件当中会应影响我们代码的可读性，那么我们相关数据库配置的信息应该放到一个`config`文件当中去，像我们当时加载debug配置一样使用`app.config.from_object(config)`一样加载我们的数据库连接信息。

新建`config.py`文件,把连接数据库相关的信息放到`config.py`中去

然后在主`app`文件中加载配置信息`app.config.from_object(config)`

config.py


​    
​    HOST = '127.0.0.1'
​    PORT = '3306'
​    DATABASE_NAME = '01_db'
​    USERNAME = 'root'
​    PASSWORD = 'root'
​    
    DB_URI = "mysql+pymysql://{username}:{password}@{host}:{port}/{databasename}?charset=utf8mb4"\
        .format(username=USERNAME,password=PASSWORD,host=HOST,port=PORT,databasename=DATABASE_NAME)
    
    SQLALCHEMY_DATABASE_URI = DB_URI
    SQLALCHEMY_TRACK_MODIFICATIONS = False


那么主`app`中的模型的文件也十分影响代码易读性，也应该新开一个`modles`文件夹，把我们的模型表放到`modles`中去

models.py


​    
​    from app import db


​    
​    class UserInfo(db.Model):
​        id = db.Column(db.Integer,primary_key=True,autoincrement=True,nullable=False)
​        name = db.Column(db.String(30),server_default='',comment="姓名")
​        tel = db.Column(db.String(16),server_default='',comment="电话"

app.py


​    
​    from flask import Flask
​    from flask_sqlalchemy import SQLAlchemy
​    import config
​    from models import UserInfo
​    
    app = Flask(__name__)
    app.config.from_object(config)
    
    db = SQLAlchemy(app)
    
    @app.route('/')
    def hello_world():
        return 'Hello World!'


​    
​    if __name__ == '__main__':
​        app.run()

这是代码易读性提高了，但是新的问题随之出现了，出现了一个循环导入的问题。

`app.py`
文件导入了`models`，我们`python`中而导入文件必然会把需要导入的文件从上到下执行一遍，那么就触发了`models`的执行，而models执行的时候需要从`app`导入`db`，出现了一个死循环如下图，这就是python循环导入的问题。

![1549982777399](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155051193-1643881132..png)

## 2.2 重构项目解决循环导入问题

为了解耦配置信息以及模型表信息，导致了`models.py`和`app.py`出现了循环导入问题，我们的解决方案是新开启一个文件`exts.py`，在`exts.py`中生成db对象，解决循环导入问题。

![1549982940543](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155051362-794772624..png)

**实例2.2.1：解决循环导入问题之后重构项目**

项目目录：


​    
​    │  app.py
​    │  config.py
​    │  exts.py
​    │  models.py
​    │
​    ├─static    # 文件夹
​    ├─templates  # 文件夹

config.py


​    
​    HOST = '127.0.0.1'
​    PORT = '3306'
​    DATABASE_NAME = '01_db'
​    USERNAME = 'root'
​    PASSWORD = 'root'
​    
    DB_URI = "mysql+pymysql://{username}:{password}@{host}:{port}/{databasename}?charset=utf8mb4"\
        .format(username=USERNAME,password=PASSWORD,host=HOST,port=PORT,databasename=DATABASE_NAME)
    
    SQLALCHEMY_DATABASE_URI = DB_URI
    SQLALCHEMY_TRACK_MODIFICATIONS = False

exts.py


​    
​    from flask_sqlalchemy import SQLAlchemy
​    
    db = SQLAlchemy()

models.py


​    
​    from exts import db
​    
    class UserInfo(db.Model):
        __tablename__ = 'user_info'
        id = db.Column(db.Integer, primary_key=True, autoincrement=True)
        username = db.Column(db.String(20),nullable=False,server_default='')

app.py


​    
​    from flask import Flask
​    from exts import db
​    import config
​    from models import UserInfo
​    
    app = Flask(__name__)
    app.config.from_object(config)
    
    db.init_app(app)
    
    @app.route('/')
    def hello_world():
        return 'Hello World!'
    
    if __name__ == '__main__':
        app.run()




# 3 使用Flask-Migrate迁移数据库

之前我们更新数据库的方式是先删除表然后再创建表简单粗暴，但是会丢失掉所有原来表中的数据。做web开发的我们应该深知数据无价，所以这个时候需要数据库迁移工具来完成这个工作，`SQLAlcheme`的开发者`Michael
Bayer`开发了一个数据库迁移工具---`Alembic`来实现数据库的迁移，`SQLAlchemy`翻译成汉语是炼金术，而蒸馏器（`Alembic`）正是炼金术士最需要的工具。

我们的`flask-sqlalchmy`扩展组件正是基于`SQLAlchemy`，当然`Flask`也有专门做数据库迁移的扩展组件`Flask-
Migrate`，同样`Flask-Migrate`正是基于`Alembic`。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155051827-500166934..png)

## 3.1 Flask-Migrate的用法：

1 由于`flask-migrate`是Flask的一个扩展组件，同往常一样首先在虚拟环境中安装我们的`flask_migrate`包。


​    
​    pip install flask-migrate

为了导出数据库迁移命令，Flask-Migrate提供了一个MigrateCommand类，可附加到Flask-
Script的manager对象上。在这个例子中，MigrateCommand类使用db命令附加。

我们的`Flask_Migrate`的操作是在`shell`下完成的，所以要基于`Flask-script`，`Flask-
Migrate`提供了一个`MigrateCommand`类，需要附加到`Flask-
Script`的`manager`对象上，完成命令的创建，并且`Flask_Migrate`同时体统了`Migrate`类，需要加载核心对象`app`和数据库对象`db`。完成迁移工具的配置。

### 实例3.1.1：配置Flask_Migrate

首先在`实例2.2.1`中创建manage.py

manage.py代码如下


​    
​    from flask_script import Manager
​    from flask_migrate import Migrate,MigrateCommand
​    from exts import db
​    from server import app
​    manager = Manager(app)


​    
​    Migrate(app,db)
​    
    manager.add_command('db',MigrateCommand)

**解读：**

**（1）** 首先从`flask_migrate`中导入 `Migrate，MigrateCommand`。


​    
​    from flask_migrate import Migrate,MigrateCommand

**（2）**`Migrate`加载`app`对象和`db`对象获取数据库的配置信息以及模型表信息。


​    
​    Migrate(app,db)

**（3）** 把`MigrateCommand`附加到`manager`创建迁移数据库的子命令


​    
​    manager.add_command('db',MigrateCommand)

### **迁移脚本命令**

**（1）** **创建迁移仓库**

首先切换到项目目录下并且切入到虚拟环境中输入命令`python manage.py db init`


​    
​    >> python manage.py db init

该命令初始化我们的迁移仓库，并且在我们的项目目录中创建迁移仓库文件

![1549988156697](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155051992-1462678497..png)

**（2）** **创建迁移脚本**

依然在我们的`shell`中输入命令`python manage.py db migrate`


​    
​    >> python manage.py db migrate

该命令会在数据库创建一张 `alembic_version`
表，存放着数据库迁移脚本的版本信息，该命令会搜集到需要迁移的模型表信息，写入到脚本中，但是并没有真正的映射到数据库中。

**（3）更新数据库**

依然在我们的`shell`中输入命令`python manage.py db upgrade`


​    
​    python manage.py db upgrade

对于第一次迁移来说，其作用和db.create_all()方法一样，但是在随后的迁移中，upgrade命令可以把模型表改动的部分映射到数据库中，实现了一个更新的效果，并且不影响之前保存的数据。

_提示：在首次执行这个命令之前如果该数据库的库内已经有了一些表，并且这些表没有与我们的模型映射，会自动删除掉这些表。_



