# flask请求上下文

![1554344346506](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155552798-25915086..png)

在分析上下问之前，要做好一个心理准备，因为设计到的代码会很多，需要不懂的要跟着文档自己去翻阅源码。

首先把涉及到的主要的类或者设计到的py页面展示如下图。下面我会以对应类或者页面去讲解flask源码

![1553480943160](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155553388-420550334..png)

之前我们已经论述过了，每次请求过来都会触发`app()`，所以会触发`FLask`类的`__call__`方法，`__call__`方法会触发`Flask`类的`wsgi_app()`方法。然后所有的请求的整个生命周期都在整个`wsgi_app()`里面了。

根据上图类和序号来完成我们的分析流程。

#### **1 首先分析请求上下文对象(ctx)创立**

  * 1.0 FLask 类中的`wsgi_app()`中的 ctx = self.request_context(environ）

  * 1.1 RequestContext类中的 `__init__`

    * 实例化出请求上下文对象ctx

    * 并且关注：
        
                if request is None:
            request = app.request_class(environ)
        self.request = request

  * 1.2 Request类中的 `__init__`

    * 该类的 `__init__`方法实例化出reqeust对象

这三部完成了初始化一个用户请求相关的数据，也就是请求上下文对象。

1.0中的ctx就是RequestContext对象，请求上下文对象ctx中初始化所有请求所有内容，并且其内部封装着Request对象，Request对象把请求过来的信息格式化并且储存起来。

#### **2 把请求对象(ctx)添加到local中（入栈）**

  * 2.0 FLask 类中的`wsgi_app()`中的 ctx.push()

  * 2.1 RequestContext 类中的 push() 下

    * 只关注_request_ctx_stack.push(self)
  * 2.2 LocalStack类中的 push()方法

    * 只关注 self._local.stack = rv = [] ，触发2.3执行。

    * 在实现了2.3的基础上，关注本方法中的 rv.append(obj) , rv就是2.3中stack的value值，此obj就是ctx对象 ，相当于为Local类中的storage里面的`当前线程或携程唯一标识`里的`stack`对应的`value`值，添加了球队上下文对象ctx，这个对象里面包含了所有请求过来的信息。

{

​ 线程或携程唯一标识:{

​ stack:[请求上下文对象ctx]。

​ }，

​ }

  * 2.3 Local类中的 `__setattr__`方法实现了创建了

    * storage = {

​ 线程或携程唯一标识:{

​ stack: [ ]

​ }，

​ }

#### **3 找到视图函数并且使用导入request对象**

  * 3.0 FLask 类中的`wsgi_app()`中 `response = self.full_dispatch_request()`的找到视图函数并执行

  * 3.1 找到了视图函数并且执行`request.method`方法。
    
        @app.route('/')
    def index():
        v = request.method
        return  v

  * 3.2 须知：`request = LocalProxy(partial(_lookup_req_object, 'request'))` 用于在视图函数里导入的request对象

    * 偏函数：partial(_lookup_req_object, 'request') _不懂可以翻阅之前的文章_
  * 3.3 触发了`LocalProxy`类 中的 `__getattr__`

    * 关注：return getattr(self._get_current_object(), name) # name是‘method’，去Request类中查询‘method’属性，
  * 3.4 触发了`LocalProxy`类 中的 `_get_current_object()`

    * 关注 `return self.__local()` #返回了Request对象

在LocalProxy类实例化的时候使得`self.__local`的值就是实例化时传入偏函数。所以会返回偏函数运行结果。

  * 3.5 触发了`globals.py` 里的 `_lookup_req_object()`运行。

    * 关注 top = _request_ctx_stack.top # 触发3.6执行
    * return getattr(top, name) # name = ‘request’，所以返回了Request对象
  * 3.6 触发了`LocalStack`类中的`top()`方法：

    * 关注 return self._local.stack[-1] # 返回了请求上下文ctx对象。
  * 3.7 触发了Local类中的`__getattr__（）`方法

    * 关注`return self.__storage__[self.__ident_func__()][name]` #返回了当前线程或携程的stack对应的value值，可以理解为返回了 `[ctx对象]`

#### 5 请求结束时从Local中移除上下文对象（出栈）

​ 经过了添加请求上下文到`Local`的`storage`中，以及视图函数的运行返回相应对象，我们现在进行把请求上下文对象从storage中移除。

  * 4.0 FLask 类中的`wsgi_app()`中 ctx.auto_pop()

  * 4.1 触发了 RequestContext类中的 auto_pop()

    * 关注 self.pop()
  * 4.2 触发了 RequestContext类中的 pop() 方法

    * rv = _request_ctx_stack.pop()
  * 4.3 触发了 LocalStack类中的pop()的pop方法
    
        elif len(stack) == 1: # 证明push过一次 添加过了一次对象
        release_local(self._local) # 在这里pop掉该线程。release_local pop掉的是一个字典
        return stack[-1]

  * 4.4 触发了 Local类中的`__release_local__()` 方法

    *         self.__storage__.pop(self.__ident_func__(), None) #在Local对象中删除掉了当前线程或者携程的请求上下文对象，

![1554344389012](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155553556-1208142947..png)

#### 总结：

  * 其实操作flask的请求上下文就是操作Local中的字典`__storage__`

    1. 通过REquestContext类首先实例化ctx请求上下文对象，其内部包含请求对象

    2. 入栈，通过请求上下文对象的类的push()方法触发了LocalStack类的push() 方法，从而添加到Local类中的字典里。

    3. 观察导入的request源码 ，通过观察LocalProxy的源码，最后触发了LocalStack的top()方得到上下文对象，再的到请求对象，从而实现reuqest的功能。

    4. 出站，和入栈原理相同通过请求上下文对象的类的方法，触发了LocalStack的的pop()方法从而从字典中删除掉当前线程或当前携程的请求信息。

![1553484834232](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155554088-541734375..png)

​

