## 1 什么是url？

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220557136-940869562.png)  

url是统一资源定位符（Uniform Resource
Locator的简写），对可以从互联网上得到的资源的位置和访问方法的一种简洁的表示，是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220700697-149176983.jpg)  

一个URL由以下几部分组成：

    
    
    scheme://host:port/path/?parameter=xxx#anchor
    https://www.baidu.com/Public/linux/?fr=aladdin#23

  * scheme：代表的是访问的协议，一般为http或者https以及ftp等。
  * host：主机名，域名，比如www.baidu.com。
  * port：端口号。当你访问一个网站的时候，浏览器默认使用80端口。
  * path：路径。比如：www.baidu.com/Public/linux/?python=aladdin#23，www.baidu.com后面的Public/linux就是path。
  * query-string：查询字符串，比如：www.baidu.com/s?wd=python，？后面的python=aladdin就是查询字符串。
  * anchor：锚点，后台一般不用管，前端用来做页面定位的。比如：https://www.oldboyedu.com/Public/linux/?fr=aladdin#23 ,#后面的23就是锚点

## 2 为什么要有url？

顾名思义统一资源定位符，是用来做定位用的，我们的web开发无非是要调用程序，而调用的具体程序我们称之为python的视图函数，URL建立了与Python视图函数一一对应的映射关系，通俗易懂可以理解为一条命令触发了一个python的函数或类。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220720147-2030343658.png)  

## 3 如何应用url？

### **3.1url和路由的区别** 。

我们调用接口需要调用的是一段具体的代码，也就是一个python类或者python函数，而url就是对这段代码的具体映射，也就是说我们可以通过url找到一个具体的python类或者python函数，这便是url。而路由是根据url定位到具体的pyhon类或python函数的程序，这段程序我们称之为路由。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220748625-986762942.png)  

在Flask程序中使用路由我们称之为注册路由，是使用程序实例提供的app.route（）装饰器注册路由，而括号内的字符串就是url，注册路由的过程就是完成了
url和python类或函数映射的过程，可以理解为会有一张表保存了url与python类或函数的对应关系。这样我们以url访问flask就可以找到对应的程序。

例：

    
    
    @app.route('/')
    def hello_world():
        return 'Hello World!'

按照这种关系我们再写一个路由

    
    
    @app.route('/student_list/')
    def student_list():
        return 'students'

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220840136-1807955680.png)  

### 3.2 url传参的两种

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220816076-278731137.png)  

#### 3.2.1动态路由传参

如果你仔细观察日常所用服务的某些URL格式，会发现很多地址中都包含可变部分。例如，你想根据学生的id找到具体的学生，http://127.0.0.1:5000/student_list//
仍然在path部分 ，但是你没必要写多个路由，我们Flask支持这种可变的路由。

见代码：

    
    
    @app.route('/student_list/<student_id>/')
    def student_list(student_id):
        return '学生{}号的信息'.format(student_id)

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220908482-1110597384.png)  

关键字：在path中有可变的部分 ，达到了传参的效果，我们称之为动态路由传参

##### **3.2.1.1 动态路由的过滤**

可以对参数限定数据类型，比如上面的文章详情，限定student_id必须为整数类型

    
    
    @app.route('/student_list/<int:student_id>/')
    def article_detail(student_id):
        return '学生{}号的信息'.format(student_id)

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220922920-807813792.png)  

  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009220936874-874639531.png)  

**主要有这几种类型过滤：**

`string`: 默认的数据类型，接收没有任何斜杠" /"的字符串

`int`: 整型

`float`: 浮点型

`path`: 和string类型相似，但是接受斜杠，如：可以接受参数/aa/bb/cc/多条放在一起

`uuid`: 只接受uuid格式的字符串字符串，

​ _✔提示：uuid为全宇宙唯一的串_

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221013213-1874652592.jpg)  

​ **上面几种约束均为如下格式** ，例子中的int可以换为 `string，float,path,uuid`：

    
    
    @app.route('/student_list/<int:student_id>/')
    def article_detail(student_id):
        return '学生{}号的信息'.format(student_id)

`any`: 可以指定多种路径，如下面的例子

_url_path的变量名是自己定义的_

    
    
    @app.route('/<any(student,class):url_path>/<id>/')
    def item(url_path, id):
        if url_path == 'student':
            return '学生{}详情'.format(id)
        else:
            return '班级{}详情'.format(id)

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221041219-1698918386.png)  

  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221104508-1430009087.png)

**动态路由的适用场景？**

如果想增加网站的曝光率，可以考虑使用动态路由，因为是把path作为参数，搜索引擎的算法会定义你为一个静态页面，不会经常改变，有利于搜索引擎的优化。但是如果是公司内部的管理系统就没有必要使用动态路由，因为内部系统对曝光率没有要求。

**关键词** ：

  * 上面我们接受参数使用的是path(路径)形式，这种传参的形式就叫动态路由传参，有利于搜索引擎的优化。

#### 3.2.2 查询字符串传参

我们上面介绍了什么是查询字符串：

如果我们在浏览器中输入www.baidu.com/s?wd=python&ad=flask的参数，这个 `？` 后的key=value便是查询字符串，

可以写多个key=value用`&`相连我们将查询字符串作为参数去请求我们的flask程序，这便是查询字符串传参。

我们之前再注册路由的时候会在里面的`path`部分以及函数的`形参`设置参数来接受path的参数，我们在查询字符串传参的时候需要从`flask`模块里面导入`request`对象，用`request.args`属性在我们的程序中根据查询字符串的`key`取出查询字符串的`value`。

`args`
是`request`的一个属性，其本质是一个`Werkzeug`依赖包的的`immutableMultiDict`的对象，用于解析我们传入的查询字符串，`immutableMultiDict`对象也继承了`Dict`类，所以可以使用字典的`.get()`方法来获取，当然了如果我们有获取原生未解析的原生查询字符串的需求，可以使用`query_string`属性。

例：

    
    
    from flask import Flask,request
    ...
    @app.route('/student_name/')
    def school_name_list():
        name = request.args.get('name')
        age = request.args.get('age')
        
        return "学生的姓名为{}，年龄为{}".format(name, age)

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221154012-1905288238.png)  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221132366-279922593.png)  

### 3.3 url_for()的使用:

#### 3.3.1简介视图函数：

我们在访问一个网址的时候在调用flask项目的时候需要调用的是一段具体的代码，也就是一个python类或者python函数，在这里这个python类我们称之为视图类，python函数我们称之为视图函数。

#### 3.3.2 url_for()的作用：

如果我们在视图函数中想使用一个url，比如给前端返回，或者我们在这个视图函数中返回一个模板文件都会使用到url，url相当于一把钥匙可以开启一些资源。如果你修改了注册路由编写的url规则，相当于修改了钥匙。那么其他的视图函数依旧是使用了原来的钥匙就无效了，如果项目是一个大项目，你一点点手动的去改涉及到的的url就不合理了。url_for()就是用来解决这个问题的。

#### 3.3.3url_for()的原理：

利用视图函数名字一般不会改变的特性，利用视图函数的`名字`去动态精准的获取url，以便于开发使用。

    
    
    url_for('视图函数名字')   # 输出该视图函数url
    

**具体例子** ：

    
    
    from flask import Flask,url_for
    
    app = Flask(__name__)
    app.config.update(DEBUG=True)
    
    @app.route('/')
    def demo1():
        print(url_for("book"))  # 注意这个引用的是视图函数的名字 字符串格式
        print(type(url_for("book")))
    
        return url_for("book")
    
    
    @app.route('/book_list/')
    def book():
    
        return 'flask_book'
    
    if __name__ ==  "__main__":
        app.run()
    

我们直接访问http://127.0.0.1:5000/，经过路由的分发会触发demo1的执行。如图  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221227882-1573532257.png)  

#### 3.3.4 url_for如何处理动态的视图函数？

如果想获取动态路由，必须以关键字实参的形式为动态的path部分赋值，注意动态的path部分必须被赋值，

案例：

    
    
    @app.route('/demo2/')
    def demo2():
        
        student_url = url_for('student', id=5, name='mark') # id 就是动态path的key 必须赋值，                                                         # name 将作为查询字符串传入
        print(student_url)
    
        return student_url
    
    
    @app.route('/student/<int:id>/')
    def student(id):
        return 'student {}'.format(id)
    

控制台输出：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221309450-1448799431.png)  

浏览器输出：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221321943-149522120.png)  

#### 3.3.5 url_for如何为url添加查询字符串？

如果想在路径后面拼出来查询字符串，以关键字实参的形式放到url_for()里面作为参数，会自动拼成路径

案例：

    
    
    @app.route('/demo3/')
    def demo3():
        school_url = url_for('school', school_level='high', name='college') 
        # 具体要拼接的查询参数 以关键字实参的形式写在url_for里
        print(school_url)
    
        return school_url
    
    @app.route('/school/')
    def school():
    
        return 'school message'
    

控制台输出：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221358688-938015856.png)  

浏览器输出：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221411387-2114824225.png)  

### 3.4 自定义动态路由过滤器

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221425354-2049497622.jpg)  

#### 3.4.1 自定义动态路由过滤器之正则匹配

我们可以通过继承`werkzeug.routing 的BaseConverter`类从而自己定义一个动态路由过滤器的规则

    
    
    from flask import Flask,request
    from werkzeug.routing import BaseConverter
    
    
    app = Flask(__name__)
    app.debug =True
    
    class TelephoneConverter(BaseConverter):
        regex = '1[3857]\d{9}' #右下斜杠d
    
    
    app.url_map.converters['tel'] = TelephoneConverter
    
    @app.route('/student/<tel:telenum>/')
    def student_detail(telenum):
    
        return '学生的手机号码是{}'.format(telenum)
    
    if __name__ == '__main__':
        app.run()
    
    
    

注意：

  1. 自定义动态路由过滤器类，该类必须继承`werkzeug.routing` 的`BaseConverter`类

  2. 通过`regex`属性指定路由规则

  3. 讲自定义的类映射到`app.url_map.converters`中（其本质是一个字典）

`app.url_map.converters['tel'] = TelephoneConverter`

实现效果：  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221525627-1848936331.png)  

  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221534845-70594688.png)  

#### 3.4.2 自定义动态路由过滤器之处理动态路由

自定义一个类，该通过继承`werkzeug.routing 的BaseConverter`类不光可以实现正则匹配，我们介绍一下以下两个方法：

  * 在该类中实现 to_python 方法：

这个方法的返回值，将会传递给视图函数的形参。我们可以利用这个方法实现处理url中动态路由部分。

  * 在该类中实现 to_url 方法：

翻转url的时候也就是使用url_for函数的时候，我们传入指定的动态路由部分，触发to_url方法，这个方法的返回值，会拼接在非动态路由上，从而实现生成符合要求的url格式。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221600674-568579939.png)  

##### **实例：**

    
    
    from flask import Flask,request,url_for
    from werkzeug.routing import BaseConverter
    
    app = Flask(__name__)
    app.debug =True
    
    
    class ListConverter(BaseConverter):
        regex = '.*'     # 这个regex代表都匹配的意思，可以根据自己的需求制定url规则
        def to_python(self, value):
            '''这个函数用于拿到了路由里的动态参数赋值给value，
              可以在to_python进行操作动态参数，
              返回操作完的的结果给视图函数的形参'''
            return value.split('+')
    
        def to_url(self, value):
            '''这个函数用于和url_for连用，
               url_for通过指定给动态参数(以关键字实参的形式)赋值给value
               我们可以根据我们的需求操作url_for传进来的参数，
               然后返回一个理想的动态路由内容拼接在url上'''
            return '+'.join(value)
    
    
    app.url_map.converters['list'] = ListConverter
    
    @app.route('/student_list/<list:students>/')
    def student_list(students):
        print(url_for('student_list',students=['a','b'])) # 输出 /student_list/a+b/
    
        return '{}'.format(students)
    
    
    if __name__ == '__main__':
        app.run()
    

证明`to_python()`方法把访问时候动态路由部分被处理成列表了。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221628236-97482956.png)  

证明我们的 `to_url()` 方法把`url_for()`函数传入的动态路由部分由列表转换成拼接字符串了。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191009221642997-497647337.png)  

