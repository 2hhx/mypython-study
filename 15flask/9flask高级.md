# 08-Flask高级

![1554291766450](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155214676-1216796716..png)

# 01 请求扩展

### 01 before_first_request ：项目启动后第一次请求的时候执行

    
    
    @app.before_first_request
    def before_first_request():
        print('第一次请求的时候执行')

### 02 before_request：每次请求之前执行

    
    
    @app.before_request
    def before_request():
        print('每次请求之前执行')
        # return '直接return'    # 如果有一个写了return返回值，那么其他的before_request不会执行，视图也不会执行。

**注意：**

  * 可以写多个

  * 如果有一个写了return返回值，那么其他的before_request不会执行，视图也不会执行。

### 03 after_request：每次请求之后执行，请求出现异常不会执行

    
    
    def after_request(result):
        print('每次请求之后执行，请求出现异常不会执行')
        # 这个result是封装的响应对象，需要return否则报错
        return result

### 04 errorhandler：可以自定义监听响应的状态码并处理：

    
    
    @app.errorhandler(404)
    def errorhandler(error):
        print(error)  # 是具体的错误信息
        return '404页面跑到了火星上面去了'
    
    @app.errorhandler(500)
    def errorhandler(error):
        print('errorhandler的错误信息')
        print(error)
        return '服务器内部错误500'

### 05 teardown_request：每次请求之后绑定了一个函数，在`非debug`模式下即使遇到了异常也会执行。

    
    
    @app.teardown_request
    def terardown_reqquest(error):
        print('无论视图函数是否有错误，视图函数执行完都会执行')
        print('想要此函数生效，debug不能为True')
        print('error 是具体的错误信息')
        print(error)

### 06 template_global()：全局模板标签

    
    
    @app.template_global()
    def add(a1, a2):
        return a1+a2
    #{{add(1,2)}}

这个可以在模板中作为全局的标签使用,在模板中可以直接调用，调用方式：

    
    
    {{add(1,2)}}

### 07 template_filter：全局模板过滤器

    
    
    @app.template_filter()
    def add_filter(a1, a2, a3):
        return a1 + a2 + a3

这个可以在模板中作为全局过滤器使用，在模板中可以直接调用，调用方式( _注意同template_global的区别_ ) ：

    
    
    {{1|add_filter(2,3,4)}}

**优势：**

全局模板标签和全局模板过滤器简化了需要手动传一个函数给模板调用：

    
    
    # app.py
    ```
    
    def test(a1,a2):
        return a1+a2
    @app.route('/')
    def index():
        return render_template('index.html',test=test)
    
    ```
    
    # index.html
    ```
    {{test(22,22)}}
    
    ```
    

# 02 flask中间件

![1554292073014](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155214907-880079762..png)

Flask的中间件的性质，就是可以理解为在整个请求的过程的前后定制一些个性化的功能。

##### flask的中间件的实现案例：

    
    
    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route('/')
    def index():
        print('视图函数中')
        return 'hello world'
    
    class my_middle:
    
        def __init__(self,wsgi_app):
            self.wsgi_app = wsgi_app
    
        def __call__(self, *args, **kwargs):
            print('中间件的代码上')
            obj = self.wsgi_app( *args, **kwargs)
            print('中间件的代码下')
    
            return obj
    
    if __name__ == '__main__':
       
        app.wsgi_app = my_middle(app.wsgi_app)
         # app.wsgi_app(environ, start_response)
        app.run()
        # 梳理一下 根据werkzeug我们可以知道 每次请求必然经历了app（）
        # 所以我们要查看Flask的源码找到__call__方法
        # 找到了__call__方法后发现执行了return self.wsgi_app(environ, start_response)
        # 然后flask里面所有的内容调度都是基于这个self.wsgi_app(environ, start_response)，这就是就是flask的入口
        # 如何实现中间件呢？ 原理上就是重写app.wsgi_app，然后在里面添加上一些自己想要实现的功能。
        # 首先分析  app.wsgi_app需要加括号执行  所以我们把app.wsgi_app做成一个对象，并且这个对象需要加括号运行
        # 也就是会触发这个对象的类的__call__()方法
        # 1 那么就是app.wsgi_app=对象=自己重写的类(app.wsgi_app) ，我们需要在自己重写的类里面实现flask源码中的app.wsgi_app,在实例化的过程把原来的app.wsgi_app变成对象的属性
        # 2         app.wsgi_app() =对象() = 自己重写的类.call()方法
        # 3         那么上面的代码就可以理解了，在自己重写的类中实现了原有的__call__方法
    

##### 梳理：

  * 根据`werkzeug`我们可以知道 每次请求必然经历了`app（）`
  * 所以我们要查看Flask的源码找到`__call__`方法
  * 找到了Flask的`__call__`方法后发现执行了`return self.wsgi_app(environ, start_response)`

  * flask里面所有的内容调度都是基于这个`self.wsgi_app(environ, start_response)`，这就是就是`flask`的入口，也就是selef是app，也就是`app.wsgi_app（environ, start_response）`为程序的入口。
  * 如何实现中间件呢？ 原理上就是重写app.wsgi_app，然后在里面添加上一些自己想要实现的功能。
  * 首先分析 app.wsgi_app需要加括号执行 所以我们把app.wsgi_app做成一个对象，并且这个对象需要加括号运行。
  * 也就是会触发这个对象的类的`__call__()`方法。

##### 实操理解：

  1. **app.wsgi_app=对象=自己重写的类(app.wsgi_app)**

​ _提示：我们需要在自己重写的类里面实现flask源码中的app.wsgi_app,在实例化的过程把原来的 app.wsgi_app变成对象的属性_

  2. ​ **app.wsgi_app(） =对象() = 自己重写的类.call()方法**

app.wsgi_app(实参） =对象(实参) = 自己重写的类.call(实参)方法

  3. ​ **那么上面的代码就可以理解了，在自己重写的类中实现了原有的call方法，并且重新调用了原有的app.wsgi_app**

![1554292169870](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155215254-2005194839..png)

# 03 蓝图：

## 3.1 蓝图的基本使用

在我的flask中，我们可以利用蓝图对程序目录的划分。

思考如果我们有很多个视图函数，比如下面这样我们是不是应该抽取出来专门的py文件进行管理呢？

    
    
    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route('/login/')
    def login():
        return "login"
    
    @app.route('/logout/')
    def logout():
        return "logout"
    
    @app.route('/add_order/')
    def add_order():
        return "add_order"
    
    @app.route('modify_order')
    def modify_order():
        return "modify_order"
    
    
    if __name__ == '__main__':
        app.run()

上面的这种是不是会显得主运行文件特别乱，这个时候我们的蓝图就闪亮登场了。

##### **3.1.1实例：**

项目目录：

    
    
    -templates
    -static
    -views
        -user.py
        -order.py
    -app.py

views/user.py

    
    
    from flask import Blueprint
    
    # 1 创建蓝图
    user_bp = Blueprint('user',__name__)
    
    # 2 利用蓝图创建路由关系
    @user_bp.route('/login/')
    def login():
        return "login"
    
    @user_bp.route('/logout/')
    def logout():
        return "logout"
    

views/order.py

    
    
    from flask import Blueprint
    
    order_bp = Blueprint('order',__name__)
    
    @order_bp.route('/add_order/')
    def add_order():
        return "add_order"
    
    @order_bp.route('/modify_order/')
    def modify_order():
        return "modify_order"
    

app.py

    
    
    from flask import Flask
    from views.user import user_bp
    from views.order import order_bp
    
    app = Flask(__name__)
    # 3 注册蓝图
    app.register_blueprint(user_bp)
    app.register_blueprint(order_bp)
    
    
    if __name__ == '__main__':
        app.run()

访问：

![1552424766233](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155215474-577184084..png)

其他的几条路由也是直接访问，在此就不做展示了。

**讲解：**

观察views/user.py

  * ​我们可以把所有的视图函数抽出来多个文件。

  * 在这里我们通过`user_bp = Blueprint('user',__name__)`创建一个蓝图对象

​ 参数讲解：

    * user_bp ：是用于指向创建出的蓝图对象，可以自由命名。
    * Blueprint的第一个参数自定义命名的`‘user’`用于`url_for`翻转`url`时使用。
    * `__name__`用于寻找蓝图自定义的模板和静态文件使用。
  * 蓝图对象的用法和之前实例化出来的app对象用法很像，可以进行注册路由。

观察app.py

  * 这里我们需要手动的去注册一下蓝图，才会建立上url和视图函数的映射关系。

**关键词：**

  1. 创建蓝图

`user_bp = Blueprint('user',__name__)`

  2. 利用蓝图创建路由关系

@bp.route('/login/')  
def login():  
return "login"

  3. 注册蓝图  
app.register_blueprint(bp)

![1554292266596](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155215669-2065781985..png)

## 3.2 蓝图的高级使用（重点备课内容）

### 3.2.1 蓝图中实现path部分的url前缀

创建蓝图的时候填写`url_prefix`可以为增加url的path部分的前缀，可以更方便的去管理访问视图函数。

    
    
    from flask import Blueprint
    
    # 1 创建蓝图
    user_bp = Blueprint('user',__name__,url_prefix='/user')
    # 注意斜杠跟视图函数的url连起来时候不要重复了。
    

![1552425130691](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155215854-919173490..png)

**注意：**

  1. 斜杠跟视图函数的url连起来时候不要重复了。

图解：

![1552426158515](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155216014-1683480120..png)

2.url加前缀的时候也可以再注册蓝图的时候加上，更推荐这么做，因为代码的可读性更强。

    
    
    app.register_blueprint(user_bp,url_prefix='/order')

### 3.3.2 蓝图中自定义模板路径

创建蓝图的时候填写`template_folder`可以指定自定义模板路径

    
    
    # 1 创建蓝图                                           #所对应的参数路径是相对于蓝图文件的
    user_bp = Blueprint('user',__name__,url_prefix='/user',template_folder='views_templates')

**注意** ：

  1. 蓝图虽然指定了自定义的模板查找路径，但是查找顺序还是会先找主app规定的模板路径(templates)，找不到再找蓝图自定义的模板路径。

  2. `Blueprint`的`template_folder`参数指定的自定义模板路径是相对于蓝图文件的路径。

图解：

**(01)**

![1552425776786](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155216222-141055298..png)

**(02)**

![1552425616132](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155216436-1555848420..png)

### 3.3.3 蓝图中自定义静态文件路径

创建蓝图的时候填写`static_folder`可以指定自定义静态文件的路径

    
    
    user_bp = Blueprint('user',__name__,url_prefix='/user',template_folder='views_templates',
                        static_folder='views_static')

**注意：**

  1. 在模板中使用自定义的静态文件路径需要依赖`url_for()`
  2. 下节讲解如何在模板中应用蓝图自定义的静态文件。

### 3.3.4 url_for()翻转蓝图

#### 视图中翻转url:

    
    
    url_for('创建蓝图时第一个参数.蓝图下的函数名')
    # 如：
    url_for('user.login')

#### 模板中翻转url:

    
    
    {{ url_for('创建蓝图时第一个参数.蓝图下的函数名') }}
    # 如：
    {{ url_for('user.login') }}

#### 模板中应用蓝图自定义路径的静态文件：

    
    
    {{ url_for('创建蓝图时第一个参数.static',filename='蓝图自定义静态文件路径下的文件') }}
    # 如：
    {{ url_for('user.static',filename='login.css') }}

### 3.3.5 蓝图子域名的实现

创建蓝图的时候填写`subdomain`可以指定子域名，可以参考之前注册路由中实现子域名。

**（1）** 配置C:\Windows\System32\drivers\etc\hosts

    
    
    127.0.0.1 bookmanage.com
    127.0.0.1 admin.bookmanage.com

**（2）** 给app增加配置

    
    
    app.config['SERVER_NAME'] = 'bookmanage.com:5000'

**（3）** 创建蓝图的时候添加子域名 `subdomain='admin'`

    
    
    # 1 创建蓝图                                           
    user_bp = Blueprint('user',__name__,url_prefix='/user',template_folder='views_templates',
                        static_folder='views_static',subdomain='admin')
    
    
    # 2 利用蓝图创建路由关系
    @user_bp.route('/login/')
    def login():
        return render_template('login_master.html')

**（4）** 访问admin.bookmanage.com:5000/user/login/

![1552428071447](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155216641-888219773..png)

### 3.3.6 蓝图中使用自己请求扩展

在蓝图中我们可以利用创建好的蓝图对象，设置访问蓝图的视图函数的时候触发蓝图独有的请求扩展。

**例如：**

    
    
    order_bp = Blueprint('order',__name__)
    
    
    @order_bp.route('/add_order/')
    def add_order():
        return "add_order"
    
    @order_bp.before_request
    def order_bp_before_request():
        return '请登录'

注意：

  * 只有访问该蓝图下的视图函数时候才会触发该蓝图的请求扩展。
  * 可以这么理解：相当app的请求扩展是全局的，而蓝图的请求扩展是局部的只对本蓝图下的视图函数有效。

![1554292322852](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155216797-536887290..png)

## 3.3 使用蓝图之中小型系统

目录结构：

    
    
    -flask_small_pro
        -app01
            -__init__.py
            -static
            -templates
            -views
                -order.py
                -user.py
         -manage.py 
            

`__init__.py`

    
    
    from flask import Flask
    from app01.views.user import user_bp
    from app01.views.order import order_bp
    
    
    app = Flask(__name__)
    
    app.register_blueprint(user_bp,url_prefix='/user')
    app.register_blueprint(order_bp)

user.py

    
    
    from flask import Blueprint
    
    user_bp = Blueprint('user',__name__)
    
    
    @user_bp.route('/login/')
    def login():
        return 'login'
    
    @user_bp.route('/logout/')
    def logout():
        return 'logout'
    

order.py

    
    
    from flask import Blueprint
    
    order_bp = Blueprint('order',__name__)
    
    @order_bp.route('/add_order/')
    def add_order():
        return 'buy_order'
    
    
    @order_bp.route('/modify_order/')
    def modify_order():
        return 'modify_order'

manage.py

    
    
    from app01 import app
    
    
    if __name__ == '__main__':
        app.run()

## 3.4 使用蓝图之使用大型系统

这里所谓的大型系统并不是绝对的大型系统，而是相对规整的大型系统，相当于提供了一个参考，在真实的生成环境中会根据公司的项目以及需求，规划自己的目录结构。

文件路径：

    
    
    │  run.py  
    │
    │
    └─pro_flask  # 文件夹
        │  __init__.py 
        │
        ├─admin  # 文件夹
        │  │  views.py
        │  │  __init__.py
        │  │
        │  ├─static # 文件夹
        │  └─templates  # 文件夹
        │
        └─web   # 文件夹
           │  views.py
           │  __init__.py
           │
           ├─static  # 文件夹
           └─templates # 文件夹
        
        

run.py 启动app

    
    
    from pro_flask import app
    
    if __name__ == '__main__':
        app.run()

`__init__.py` 实例化核心类，导入蓝图对象，注册蓝图。

    
    
    from flask import Flask
    from .admin import admin
    from .web import web
    
    app = Flask(__name__)
    app.debug = True
    
    app.register_blueprint(admin, url_prefix='/admin')
    app.register_blueprint(web)

admin.views.py 完成注册路由以及视图函数

    
    
    from . import admin
    
    
    @admin.route('/index')
    def index():
        return 'Admin.Index'

`admin.__init__.py` 生成蓝图对象导入views，使得views的代码运行完成注册路由

    
    
    from flask import Blueprint
    
    admin = Blueprint(
        'admin',
        __name__,
        template_folder='templates',
        static_folder='static'
    )
    from . import views

web文件夹下和admin文件夹下目录构成完全一致，这里就不举例子了。

