

# 01-cookie

## 1.1 什么是cookie？

`cookie`技术产生源于`HTTP`协议在互联网上的急速发展，在浏览器发展初期，为了适应用户的需求，技术上推出了各种保持web浏览状态的手段，为什么要保持web浏览器的状态呢？

一般`web`通信是基于`HTTP`的，`HTTP`是无状态的协议，也就是说，在一次请求响应结束后，服务器不会留下任何有关于对方状态信息，所以需要保持web浏览器的状态。

比如：对于有些web应用来说，客户端的某些信息必须被记住。比如用户登录过后跳转页面依然要保持登录的状态，进行其他的业务访问，而当这个登录过的用户再次访问web服务器的时候，web服务器并不知道这个用户已经登录过了，所以无法进行其他需要权限的业务访问。所以`cookie`技术的出现就是为了解决这个问题。

`cookie`的具体实现过程：当一个用户访问`web`服务器后，`web`服务器会获取用户的状态并且返回一些数据（cookie）给浏览器，浏览器会自动储存这些数据（cookie），当用户再次访问web服务器，浏览器会把cookie放到请求报文中发送给web服务器，web服务器就会获取到了用户的状态。基于这次用户的状态方便用户进行其他业务的访问，并且web服务器可以设置浏览器保存cookie的时间，cookie是有域名的概念，只有访问同一个域名的时候才会把之前相同域名返回的cookie携带给该web服务器。

_附注：1993年，网景公司雇员`Lou Montulli`为了提升用户体验，进一步实现了个人化网络。发明了今天广泛使用的`Cookie`。_

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155133265-1996976369..jpg)

**关键词** ：

  * `web`通讯一般基于`HTTP`协议，`HTTP`是无状态协议。
  * `Cookie`技术是用来保持`web`访问状态，`Cookie`技术通过在请求和响应报文中添加`Cookie`数据来保存客户端的状态信息
  * 服务器可以设置`cookie`的有效期，浏览器会自动清除过期的`cookie`。
  * `cookie`有域名的概念，只有访问同一个域名，才会把之前相同域名返回的cookie携带给该服务器。

## 1.2 如何在flask中使用cookie？

### 1.2.1 设置cookie

设置`cookie`的时候是由我们`web`服务器设置，也就是在`Flask`项目中生成`cookie`，经由响应报文返回给浏览器保存`cookie`，下次浏览器再访问`web`服务器的时会在请求报文中把`cookie`携带过来，所以`cookie`产生的起点是在web服务器中，也就是我们的Flask项目中。

在Flask中如果想要在响应中添加一个cookie，最方便的做法是使用内置的`Response`类提供的`set_cookie()`方法。

 **表-2.2.1.1 set_cookie()方法的参数**

属性 | 说明  
---|---  
key | `cookie`的键（名称）  
value | `cookie`的值  
max_age | `cookie`被保存的时间数，单位为秒。  
expires | 具体的过期时间，一个`datetime`对象或UNIX时间戳  
path | 限制`cookie`只在给定的路径可用，默认为整个域名下路径都可用  
domain | 设置`cookie`可用的域名，默认是当前域名，子域名需要利用通配符`domain=.当前域名`  
secure | 如果设为`True`，只有通过`HTTPS`才可以用  
httponly | 如果设为`True`，进制客户端`JavaScript`获取`cookie`  

#### 1.2.1.2 实例：设置cookie

项目目录


​    
    │  app.py
    │
    ├─static    # 文件夹
    └─templates # 文件夹

app.py


​    
    from flask import Flask, Response
    
    app = Flask(__name__)


​    
    @app.route('/')
    def hello_world():
        resp = Response('设置cookie给浏览器')
        resp.set_cookie('user_name', 'mark')
    
        return resp
    
    if __name__ == '__main__':
        app.run()


**解读 app.py** ：

**(1)** 首先导入`Flask`内置的`Response`类，用于在响应报文中设置`cookie`


​    
    from flask import Flask,request, Response

**(2)** 在视图函数实例化`Response`类并传入返回的内容，`Response`类实例化出的对象调用`set_cookie()`方
法，set_cookie内的第一个参数是设置cookie的`key`，第二个参数是用来设置cookie的`value`，然后返回该对象，就会携带着设置好的`cookie`返回给浏览器保存。


​    
    @app.route('/')
    def hello_world():
        resp = Response('设置cookie给浏览器')
        resp.set_cookie('user_name', 'mark')
    
        return resp

#### **1.2.1.3 在浏览器中查看cookie的三种方式（以Chrome浏览器为例）**

**基于2.2.1.2实例**

**第一种** : 右键检查----->Network---->找到访问的域名---->找到Response Headers---->Set-Cookie

![1550736406457](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155133532-1177892422..png)

**第二种** ：点击url输入框左边的信息icon，然后找到响应的域名，展开查看cookie。

![1550736586670](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155133751-1539354373..png)

![1550736643161](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155133940-1885846214..png)

**第三种** ：设置---->高级---->内容设置---->Cookie---->查看所有cookie设置----->根据域名搜索对应的cookie信息

![1550740317286](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155134138-1836502968..png)

![1550740351876](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155134340-896718454..png)

![1550740399039](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155134529-315038539..png)

![1550740431743](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155134741-2012530184..png)

![1550740461224](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155134936-1531826233..png)

![1550740533491](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155135123-1069381446..png)

![1550740558622](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155135301-180033248..png)

### 1.2.2 设置cookie的有效期

**注意：Flask服务器默认设置cookie有效期为关闭浏览器后cookie失效** 。

#### 1.2.2.1 基于max_age参数设置cookie有效期

再设置`cookie`的调用`set_cookie()`时候传入关键字实参 `max_age= 值`，这个`值`代表多少秒后过期。

_注意：max_age参数设置过期时间不兼容IE8一下的浏览器_


​    
    ...
    @app.route('/')
    def hello_world():
        resp = Response('设置cookie给浏览器')
        resp.set_cookie('user_name', 'mark',max_age=60)
        
        return resp
    ...

![1550743936736](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155135784-444930551..png)

#### 1.2.2.2 基于expires参数设置cookie有效期

再设置`cookie`的调用`set_cookie()`时候传入关键字实参 `expires=
值`，这个`值`代具体的过期时间，一个`datetime`对象或UNIX时间戳。

_使用expires参数，就必须会用格林尼治时间（也就是相对北京时间少8个小时，因为浏览器会默认把服务器传来的时间值当做标准格林尼治时间，并根据当地的时区做调整_
。


​    
    @app.route('/expires_demo/')
    def expires_demo():
        resp = Response('设置cookie给浏览器, cookie设置过期时间为一个月后')
        expires = datetime.now()+timedelta(days=30, hours=16)
        resp.set_cookie('user_name', 'mark', expires=expires)
        return resp

![1550886456558](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155135938-1085978225..png)

### 1.2.3 在Flask中查询cookie

基于2.2.1.2 实例

查询`cookie`
是通过请求对象的`cookies`属性读取，读取的过程是使用设置`cookie`时的`key`来读取到设置`cookie`的`value`


​    
    ...
    @app.route('/get_cookie/')
    def get_cookie():
        user_name = request.cookies.get('user_name')
        if user_name == 'mark':
            return '{}的信息'.format(user_name)
    
        return 'cookie验证失败'
    ...


![1550741843996](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155136144-1923310819..png)

### 1.2.4删除cookie

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155136359-420093650..png)

基于2.2.1.2实例

删除`cookie`是通过`Flask`内置的`Response`类实例化出的对象调用`delete_cookie('key')`，删除的过程是使用设置`cookie`时的`key`来删除`cookie`信息。


​    
    @app.route('/del/')
    def del_cookie():
        resp = Response('删除cookie')
        resp.delete_cookie('user_name')
        return resp

# 02-session

## 2.1 什么是session？

session的基本概念：session又称之为安全的cookie，session是一个思路、是一个概念、一个服务器存储授权信息的解决方案，不同的服务器，不同的框架，不同的语言有不同的实现，session的目的和cookie完全一致，cookie在客户端和服务端处理的非常粗糙，cookie在浏览器保存的时候以及传输的过程均使用明文，导致了很多安全隐患问题，session的出现就是为了解决cookie存储数据不安全的问题。

_注意：session是一个思路一个概念，session的实现是基于cookie的，session并不像cookie是一项真实存在的技术，可以简单的理解为把粗糙的cookie在服务端通过加密，永久化等方式提高cookie的安全级别。_

## 2.2 实现session的两种思路

**第一种**

  1. 客户端携带用户信息请求服务端验证。
  2. 服务端验证成功后生成随机的session_id与用户信息建立映射后存储到数据库中（注意：数据库可以是任意永久化保存数据的机制，如redis、memcached、mysql、甚至是文件等等）。
  3. 服务端把刚刚生成的session_id作为cookie信息返回给客户端。
  4. 客户端收到以session_id为内容的cookie信息保存到本地。
  5. 客户端再次请求的时候会携带以session_id为内容的cookie去访问服务端，服务端取出session_id去数据库校验得到用户信息。

![1550917260707](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155136681-1046277379..png)

**第二种**

  1. 客户端携带用户信息请求服务端验证。
  2. 服务端收到用户信息验证成功后，服务端再把用户信息经过严格的加密加盐生成session信息。并且把刚刚生成的session信息作为cookie的内容返回给客户端。
  3. 客户端收到以session信息为内容的cookie保存到本地。
  4. 客户端再次请求的时候会携带以session信息为内容的cookie去访问服务端，服务端取出session信息经过解密得到用户的信息。

![1550918351048](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155136850-2054093429..png)

_注意：flask使用的就是第二种思路，利用加密解密的方式实现session，实现安全的cookie，服务端并不会做永久化的储存。_

## 2.3 如何在flask中实现session？

### 2.3.1 设置session

Flask提供了session对象用来将cookie加密储存，session通过秘钥对数据进行签名以加密数据。


​    
    from flask import Flask, session
    import os
    
    app = Flask(__name__)
    app.config['SECRET_KEY'] = os.urandom(24) # 配置session使用的秘钥
    
    @app.route('/')
    def set_session_info():
        session['username'] = 'mark' # 使用用户信息配置sesion信息作为cookie，并添加到响应体中
        
        return '设置session信息'

**解读**

通过app对象 通过`SECRET_KEY`配置session使用的加密秘钥


​    
    app.config['SECRET_KEY'] = os.urandom(24) # 配置session使用的秘钥

session对象像可以字典一样操作，内部是把字典的信息进行加密操作然后添加到相应体中作为cookie，响应的时候会自动返回给浏览器。


​    
     session['username'] = 'mark'
     session['userphone'] = '123456'  # 可以指定多条session信息，统一放到响应的cookie中返回给浏览器

![1550929913778](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155137020-1828638087..png)

### 2.3.2 设置session有效期

后端`Flask`跟浏览器交互默认情况下，session
cookie会在用户关闭浏览器时清除。通过将session.permanent属性设为True可以将session的有效期延长为31天，也可以通过操作`app`的配置`PERMANENT_SESSION_LIFETIME`来设置`session`过期时间。

**案例 3.3.2.1:开启指定session过期时间模式**


​    
    from flask import Flask, session
    import os
    
    app = Flask(__name__)
    app.config['SECRET_KEY'] = os.urandom(24)


​    
    @app.route('/')
    def set_session_info():
        session['username'] = 'mark'
        session['userphone'] = '123456'
        session.permanent = True # 开启设置有效期，默认为31天后过期
        return 'Hello World!'
    ...

![1550931605863](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155137209-326713179..png)

**案例 3.3.2.1:开启session指定过期时间模式后指定具体的过期时间**

基于案例3.3.2.1，通过设置`PERMANENT_SESSION_LIFETIME`指定具体的过期时间


​    
    from datetime import timedelta
    app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(hours=1) # 设置为1小时候过期

![1550932150978](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155137356-1128605837..png)

### 2.3.3 获取session

在Flask中获取设置的session信息通过session对象获取，session对象是继承了字典类，所以获取的时候是字典的取值方式。其内部会把浏览器传过来的session信息解密。


​    
    @app.route('/get_session/')
    def get_session():
        username = session.get('username')
        userphone = session.get('userphone')
        if username or userphone:
            return "{},{}".format(username, userphone)
        return "session为空"


![1550930691065](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155137512-1371028199..png)

### 2.3.4 删除session

`session`对象调用`pop()`可以根据具体的`session`的key清除掉指定的session信息。

session对象调用`clear()`可以清除此次请求的浏览器关于本域名的所有session信息


​    
    @app.route('/del_session/')
    def del_session():
        session.pop('username')
        # session.clear()
        return '删除成功'



# 3.1 flask模板上下文处理器

`app`对象调用`context_processor`作为模板上下文处理器，视图函数在每一次调用render_template('')的时候都会为模板传入`@app.context_processor`装饰器所装饰函数的返回值，该返回值作为模板变量，但是返回值一定要为字典，如果不想返回任何值，可以返回空字典，否则会报错，返回值可以设置为模板经常要使用的变量，减少了代码的冗余，提高了代码的可维护性。

**实例 4.1**


​    
    ...
    
    @app.route('/')
    def hello_world():
        context_dict = {
            "username": "马克"
        }
        
        return render_template('index.html', **context_dict)


​    
    @app.route('/detail/')
    def detail():
        context_dict = {
            "username": "马克"
        }
        
        return render_template('detail.html', **context_dict)


​    
    ...


**实例4.2**

实例4.2
利用模板上下文处理器避免了一些代码的冗余，利用该处理器，可以为视图函数每一次返回模板的时候传入设置好的变量，`实例4.2`实现的效果同`实例4.1`完全一致。


​    
    ...
    
    @app.route('/')
    def hello_world():
        return render_template('index.html')
    
    @app.route('/detail/')
    def detail():
        return render_template('detail.html')
    
    @app.context_processor
    def context_processor():
        return {"username":"马克"}
    
    ...

_适用场景：比如登录网站后用户信息始终显示在页面的右上角，我们可以利用模板上下文处理器，做到每次返回模板的时候都为其传入用户信息，减少了代码的冗余，提高了代码的可维护性。_

# 4 闪现



### 4.1 在模板中获取闪现信息

Flask
提供了一个非常简单的方法来使用闪现系统向用户反馈信息。闪现系统使得在一个请求结束的时候记录一个信息，`然后在且仅仅在下一个请求中访问这个数据`，强调flask闪现是基于`flask`内置的`session`的，利用浏览器的`session`缓存闪现信息。所以必须设置`secret_key`。

#### 4.1.1 简单的在模板中实现获取闪现信息

**实例：**

server.py


​    
    from flask import Flask, flash, redirect, render_template, \
         request, url_for
    
    app = Flask(__name__)
    app.secret_key = 'some_secret'
    
    @app.route('/')
    def index():
        return render_template('index.html')
    
    @app.route('/login', methods=['GET', 'POST'])
    def login():
        error = None
        if request.method == 'POST':
            if request.form['username'] != 'admin' or \
                    request.form['password'] != '123':
                error = '登录失败'
            else:
                flash('恭喜您登录成功')
                return redirect(url_for('index'))
        return render_template('login.html', error=error)
    
    if __name__ == "__main__":
        app.run()

**_注意：这个`flash()` 就可以实现在下一次请求时候，将括号内的信息做一个缓存。不要忘记设置secret_key_**

这里是 index.html 模板:


​    
    {% with messages = get_flashed_messages() %}  # 获取所有的闪现信息返回一个列表
      {% if messages %}
        <ul class=flashes>
        {% for message in messages %}
          <li>{{ message }}</li>
        {% endfor %}
        </ul>
      {% endif %}
    {% endwith %}
    
    <h1>主页</h1>
      <p>跳转到登录页面<a href="{{ url_for('login') }}">登录?</a>

**_注意：{% with messages = get_flashed_messages() %} # 获取所有的闪现信息返回一个列表_**

这里是login.html 模板


​    
    <h1>登录页面</h1>
    {% if error %}
    <p class=error><strong>Error:</strong> {{ error }}
    {% endif %}
        <form action="" method=post>
        用户名:
        <input type=text name=username>
        密码:
        <input type=password name=password>
        <p><input type=submit value=Login></p>
    </form>

####
![1552036299625](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155138240-12907379..png)

**简单的在模板中实现获取闪现信息小结：**


​    
    设置闪现内容：flash('恭喜您登录成功')
    模板取出闪现内容：{% with messages = get_flashed_messages() %} 



#### 4.1.2 模板中的分类闪现

当闪现一个消息时，是可以提供一个分类的。未指定分类时默认的分类为 `'message'` 。
可以使用分类来提供给用户更好的反馈，可以给用户更精准的提示信息体验。

要使用一个自定义的分类，只要使用 `flash()` 函数的第二个参数:


​    
    flash('恭喜您登录成功',"status")
    flash('您的账户名为admin',"username")

在使用`get_flashed_messages()`时候需要传入`with_categories=true`便可以渲染出来类别


​    
    {% with messages = get_flashed_messages(with_categories=true) %}
      {% if messages %}
        <ul class=flashes>
        {% for category, message in messages %}
          <li class="{{ category }}">{{ category }}：{{ message }}</li>
        {% endfor %}
        </ul>
      {% endif %}
    {% endwith %}


![1552041915121](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155139453-249225863..png)

**模板中的分类闪现小结：**


​    
    分类设置闪现内容：flash('恭喜您登录成功',"status")
                flash('您的账户名为admin',"username")
    模板取值：   {% with messages = get_flashed_messages(with_categories=true) %}
                {% if messages %}
                <ul class=flashes>
                {% for category, message in messages %}
                ...




####

#### 4.1.3 模板中过滤闪现消息

同样要使用一个自定义的分类，只要使用 `flash()` 函数的第二个参数:


​    
    flash('恭喜您登录成功',"status")
    flash('您的账户名为admin',"username")

在使用`get_flashed_messages()`时候需要传入`category_filter=["username"]`便可根据类别取出闪现信息。中括号内可以传入的值就是类别，可以传入多个。


​    
    {% with messages = get_flashed_messages(category_filter=["username"]) %}
    {% if messages %}
      <ul>
        {%- for message in messages %}
        <li>{{ message }}</li>
        {% endfor -%}
      </ul>
    </div>
    {% endif %}
    {% endwith %}

![1552041362576](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155140542-1156156426..png)

**小结：**


​    
    分类设置闪现内容：flash('恭喜您登录成功',"status")
                flash('您的账户名为admin',"username")
    模板取值：  % with messages = get_flashed_messages(category_filter=["username"]) %}
                {% if messages %}
                  <ul>
                    {%- for message in messages %}

### 4.2 在视图中获取闪现信息



#### 4.2.1 简单的在是视图中获取闪现信息


​    
    -设置: flash('xxx')
    -取值：get_flashed_message() # 注意这个不同于模板取值，这个是从flask中导入的
    -注意：在视图中获取闪现信息不必非得是两次连续的请求，只要保证是第一次取相应的闪现信息，就可以取得到。

**实例：**


​    
    from flask import Flask, request, flash, get_flashed_messages
    import os
    
    app = Flask(__name__)
    app.secret_key = os.urandom(4)
    app.debug = True
    
    @app.route('/login/')
    def login():
        if request.args.get('name') == 'rocky':
            return 'ok'
        flash('第一条闪现信息：用户名不是rocky填写的是{}'.format(request.args.get('name')))
        # flash('第二条闪现信息：用户名不是rocky填写的是{}'.format(request.args.get('name')))
        return 'error,设置了闪现'
    @app.route('/get_flash/')
    def get_flash():
        #get_flashed_messages()是一个列表列表可以取出闪现信息，该条闪现信息只要被取出就会删除掉。
        return '闪现的信息是{}'.format(get_flashed_messages())
    
    @app.route('/demo/')
    def demo():
        return 'demo'
    
    if __name__ == '__main__':
        app.run()

**（1）** 会触发设置闪现内容

![1552048742450](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155142726-1681318289..png)

**（2）** 取出闪现内容

![1552052193446](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155143713-1931514309..png)

**（3）** 再次取出闪现内容，发现闪现内容取出一次后就为空了

![1552052104889](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155143887-2108757345..png)

**小结：**

  * get_flashed_messages()是一个列表，该列表可以取出闪现信息，该条闪现信息只要被取出就会删除掉。



#### 4.2.2 在视图中实现分类获取闪现信息。


​    
    -设置：flash('用户名错误', "username_error")
          flash('用户密码错误', "password_error") # 第二个参数为闪现信息的分类。
    
    -取所有闪现信息的类别和闪现内容：get_flashed_messages(with_categories=True)
        
    -针对分类过滤取值：get_flashed_messages(category_filter=['username_error']) 
                # 中括号内可以写多个分类。
        
    -注意：如果flash()没有传入第二个参数进行分类，默认分类是 'message'


**实例1**


​    
    @app.route('/login/')
    def login():
        if request.args.get('name') == 'rocky':
            return 'ok'
        flash('用户名错误', category="username_error")
        flash('用户密码错误', "password_error")
        return 'error,设置了闪现'
    @app.route('/get_flash/')
    def get_flash():
        return '闪现的信息是{}'.format(get_flashed_messages(with_categories=True))

把所有的闪现类别和闪现信息返回。

![1552051297253](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155144215-1937103247..png)

**实例2**


​    
    @app.route('/login/')
    def login():
        if request.args.get('name') == 'rocky':
            return 'ok'
        flash('用户名错误', category="username_error")
        flash('用户密码错误', "password_error")
        return 'error,设置了闪现'
    @app.route('/get_flash/')
    def get_flash():
        return '闪现的信息是{}'.format(get_flashed_messages(category_filter=['username_error']))

返回页面只显示了 `"username_error"`的分类内容。

![1552050949373](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155144365-2146703891..png)

