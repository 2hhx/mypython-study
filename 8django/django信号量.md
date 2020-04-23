[信号量使用：https://www.cnblogs.com/liuqingzheng/articles/9803403.html](https://www.cnblogs.com/liuqingzheng/articles/9803403.html)

[信号量 使用：https://www.cnblogs.com/liwenzhou/p/9745331.html](https://www.cnblogs.com/liwenzhou/p/9745331.html)

# [Django中的信号](https://www.cnblogs.com/liwenzhou/p/9745331.html)



# 信号

Django 提供一个“信号分发器”，允许解耦的应用在框架的其它地方发生操作时会被通知到。 简单来说，信号允许特定的sender通知一组receiver某些操作已经发生。 这在多处代码和同一事件有关联的情况下很有用。

# 内置信号

## 模型信号

django.db.models.signals模块定义了模型系统发送的一组信号。

### pre_init

django.db.models.signals.pre_init
每当您实例化Django模型时，该信号都会在模型的__init__()方法的开头发送。

带有此信号的参数：

sender
刚创建了一个实例的模型类。
ARGS
传递给__init__()的位置参数列表：
kwargs
传递给__init__()的关键字参数的字典：

例如：

```
from app01 import models

p = models.Publisher(name='沙河出版社')
```

发送到pre_init处理程序的参数将是：

 

| 参数     | 值                                                       |
| -------- | -------------------------------------------------------- |
| `sender` | `Publisher`（类本身）                                    |
| `ARGS`   | `[]`（一个空列表，因为没有位置参数传递给`__init__()`。） |
| `kwargs` | `{'name'： “沙河出版社”}`                                |

 

### pre_save

django.db.models.signals.pre_save
这是在模型的save()方法的开头发送的。

带有此信号的参数：

sender
模型类。
instance
正在保存的实际实例。
raw
一个布尔值True如果模型按照显示的方式保存（即当加载固定装置时）。 不应该查询/修改数据库中的其他记录，因为数据库可能尚未处于一致状态。
using
正在使用的数据库别名。
update_fields
如果有字段被传递给Model.save()方法那么就是所传递的字段集，否则就是None。

### post_save

django.db.models.signals.post_save
像pre_save一样，但是在save()方法的末尾发送。

带有此信号的参数：

sender
模型类。
instance
正在保存的实际实例。
created
一个布尔值True如果创建了新记录。
raw
一个布尔值True如果模型按照显示的方式保存（即当加载固定装置时）。 不应该查询/修改数据库中的其他记录，因为数据库可能尚未处于一致状态。
using
正在使用的数据库别名。
update_fields
如果有字段被传递给Model.save()方法那么就是所传递的字段集，否则就是None。

### pre_delete

django.db.models.signals.pre_delete
在模型的delete()方法和queryset的delete()方法的开头发送。

带有此信号的参数：

sender
模型类。
instance
正在删除的实际实例。
using
正在使用的数据库别名。

### post_delete

django.db.models.signals.post_delete
像pre_delete一样，但是在模型的delete()方法和queryset的delete()方法的末尾发送。

带有此信号的参数：

sender
模型类。
instance
正在删除的实际实例。

请注意，该对象将不再位于数据库中，所以要非常小心使用此实例。

using
正在使用的数据库别名。

### m2m_changed

django.db.models.signals.m2m_changed
在模型实例上更改了ManyToManyField时发送。 严格来说，这不是一个模型信号，因为它是由ManyToManyField发送的，但由于它补充了pre_save / post_save和pre_delete / post_delete当跟踪模型的更改时，它包括在这里。

带有此信号的参数：

sender
描述ManyToManyField的中间模型类。 当定义多对多字段时，此类自动创建；您可以使用多对多字段上的through属性访问它。
instance
多对多关系更新的实例。 这可以是sender或ManyToManyField相关的类的一个实例。
action
指示在关系上完成的更新类型的字符串。 这可以是以下之一：

“pre_add”
在之前发送一个或多个对象被添加到关系中。
“post_add”
在之后发送一个或多个对象被添加到关系中。
“pre_remove”
在之前发送一个或多个对象从关系中删除。
“post_remove”
在之后发送一个或多个对象从关系中删除。
“pre_clear”
在之前发送关系被清除。
“post_clear”
之后发送关系被清除。
reverse
指示关系的哪一侧被更新（即，如果它是正在被修改的正向或反向关系）。
model
添加到，从关系中删除或从关系中清除的对象的类。
pk_set
对于pre_add，post_add，pre_remove和post_remove操作，这是一组主键值加入或从关系中删除。

对于pre_clear和post_clear操作，这是None。

using
正在使用的数据库别名。

### class_prepared

django.db.models.signals.class_prepared
每当模型类“准备”时发送 - 即，一旦模型已经被定义并在Django的模型系统中注册。 Django内部使用这个信号；它通常不会用于第三方应用程序。

由于此信号是在应用程序注册表群集进程期间发送的，并且在应用注册表完全填充后运行AppConfig.ready()，因此无法使用该方法连接接收器。 一种可能性是连接他们AppConfig.__init__()，注意不要导入模型或触发对应用程序注册表的调用。

使用此信号发送的参数：

sender
ready的model类。

## 管理信号

### pre_migrate

django.db.models.signals.pre_migrate
在开始安装应用程序之前，由migrate命令发送。 对于缺少models模块的应用，不会发送。

带有此信号的参数：

sender
应用程序即将迁移/同步的AppConfig实例。
APP_CONFIG
与sender相同。
verbosity
指示manage.py在屏幕上打印多少信息。 有关详细信息，请参阅--verbosity标志。

监听pre_migrate的函数应根据该参数的值调整输出到屏幕的内容。

interactive
如果interactive是True，则可以安全地提示用户在命令行中输入内容。 如果interactive为False，则侦听此信号的功能不应尝试提示任何内容。

例如，当interactive为True时，django.contrib.auth应用程序仅提示创建超级用户。

using
命令将在其上运行的数据库的别名。
plan
Django中的新功能1.10。
迁移计划将用于迁移运行。 虽然该计划不是公共API，但这在允许罕见的情况下需要知道计划。 一个计划是两个元组的列表，第一个项目是迁移类的实例，第二个项目显示迁移是否回滚（True）或应用（False

apps
Django中的新功能1.10。
包含迁移运行前的项目状态的Apps的实例。 应该使用它来代替全局apps注册表来检索要执行操作的模型。

### post_migrate

django.db.models.signals.post_migrate
在migrate（即使不进行迁移）和flush命令的末尾发送。 对于缺少models模块的应用，不会发送。

此信号的处理程序不得执行数据库模式更改，因为如果在migrate命令期间运行，则可能会导致flush命令失败。

带有此信号的参数：

sender
刚刚安装的应用程序的AppConfig实例。
APP_CONFIG
与sender相同。
verbosity
指示manage.py在屏幕上打印多少信息。 有关详细信息，请参阅--verbosity标志。

监听post_migrate的函数应根据此参数的值调整输出到屏幕的内容。

interactive
如果interactive是True，则可以安全地提示用户在命令行中输入内容。 如果interactive为False，则侦听此信号的功能不应尝试提示任何内容。

例如，当interactive为True时，django.contrib.auth应用程序仅提示创建超级用户。

using
用于同步的数据库别名。 默认为default数据库。
plan
Django中的新功能1.10。
用于迁移运行的迁移计划。 虽然该计划不是公共API，但这在允许罕见的情况下需要知道计划。 一个计划是两个元组的列表，第一个项目是迁移类的实例，第二个项目显示迁移是否回滚（True）或应用（False

apps
Django中的新功能1.10。
包含迁移运行后项目状态的Apps的实例。 应该使用它来代替全局apps注册表来检索要执行操作的模型。

## 请求/响应信号

处理请求时由核心框架发送的信号。

### request_started

django.core.signals. request_started
当Django开始处理HTTP请求时发送。

带有此信号的参数：

sender
处理程序类 - 例如django.core.handlers.wsgi.WsgiHandler - 处理该请求。
ENVIRON
environ字典提供给请求。

### request_finished

django.core.signals.request_finished
当Django完成向客户端传递HTTP响应时发送。

带有此信号的参数：

sender
处理程序类，如上。

### got_request_exception

django.core.signals. got_request_exception
当Django在处理传入的HTTP请求时遇到异常时，会发送此信号。

带有此信号的参数：

sender
处理程序类，如上。
request
HttpRequest对象。

## 测试信号

信号只在running tests时发送。

### setting_changed

django.test.signals.setting_changed
当通过django.test.TestCase.settings()上下文管理器或django.test.override_settings()装饰器/上下文管理器

实际发送两次：应用新值（“setup”）和恢复原始值（“拆除”）时。 使用enter参数来区分两者。

您还可以从django.core.signals导入此信号，以避免在非测试情况下从django.test导入。

带有此信号的参数：

sender
设置处理程序。
setting
设置的名称。
value
更改后的设置值。 对于最初不存在的设置，在“拆卸”阶段，value为None。
enter
一个布尔值True如果应用设置，False如果还原。

### template_rendered

django.test.signals.template_rendered
测试系统呈现模板时发送。 在Django服务器的正常操作期间不发出此信号 - 它仅在测试期间可用。

带有此信号的参数：

sender
被渲染的Template对象。
template
与发信人相同
context
模板呈现的Context。

## 数据库包装器

当数据库连接启动时，由数据库包装器发送的信号。

### connection_created

django.db.backends.signals.connection_created
当数据库包装器与数据库进行初始连接时发送。 如果您想将任何后续连接命令发送到SQL后端，这一点尤其有用。

带有此信号的参数：

sender
数据库包装器类 - 即django.db.backends.postgresql.DatabaseWrapper或django.db.backends.mysql.DatabaseWrapper等
connection
打开的数据库连接。 这可以在多数据库配置中使用，以区分来自不同数据库的连接信号。

## 使用信号

回到前面的面试图，如何在Django中实现每一次ORM保存到数据库的时候执行指定操作？

### Receiver函数

Receiver可以是任何Python函数或者方法：

```
def my_callback(sender, **kwargs):
    print(sender)
    print(kwargs)
    print("要保存了啊！")
    print('-' * 120)
```

### 监听信号

一旦某个指定信号触发，就执行我指定的receiver函数。

我们现在的需求是，模型层一执行保存的动作就做什么事。

所以应该是一旦触发 pre_save 信号就执行 my_callback，对于内置的信号Django框架会自动帮我们触发，我们只需要告诉它信号触发之后做什么事就可以了：

```
pre_save.connect(my_callback)
```

接下来，只要涉及到ORMC鞥面的save()操作，都会自动执行我定义的my_callback函数了。

例如：

```
a3 = Author.objects.create(name='测试信号-作者')
b3 = Book.objects.create(title='测试信号-书')
```

输出：



```
<class 'app02.models.Author'>
{'signal': <django.db.models.signals.ModelSignal object at 0x108e0d198>, 'instance': <Author: 测试信号-作者>, 'raw': False, 'using': 'default', 'update_fields': None}
要保存了啊！
------------------------------------------------------------------------------------------------------------------------
(0.001) SELECT @@SQL_AUTO_IS_NULL; args=None
(0.001) INSERT INTO `app02_author` (`name`) VALUES ('测试信号-作者'); args=['测试信号-作者']
<class 'app02.models.Book'>
{'signal': <django.db.models.signals.ModelSignal object at 0x108e0d198>, 'instance': <Book: 测试信号-书>, 'raw': False, 'using': 'default', 'update_fields': None}
要保存了啊！
------------------------------------------------------------------------------------------------------------------------
(0.001) INSERT INTO `app02_book` (`title`) VALUES ('测试信号-书'); args=['测试信号-书']
```



### 使用装饰器方式监听信号



```
# 使用装饰器方式
from django.db.models.signals import pre_save
from django.dispatch import receiver


@receiver(pre_save)
def my_callback(sender, **kwargs):
    print(sender)
    print(kwargs)
    print("要保存了啊！")
    print('-' * 120)
```



## 自定义信号

上面列出来的都是Django框架内置的信号，当然我们还可以自定义信号。

### 定义信号

所有信号都是 django.dispatch.Signal 的实例。 providing_args是一个列表，由信号将提供给监听者的参数名称组成。 理论上是这样，但是实际上并没有任何检查来保证向监听者提供了这些参数。

举个例子：

```
# 自定义信号
from django.dispatch import Signal

bath_done = Signal(providing_args=['amount', 'temperature'])
```

这里定义了一个洗澡水烧好了的信号，它接受两个参数：amount表示水量，temperature表示温度。

### 注册receiver



```
from django.dispatch import receiver

@receiver(bath_done)
def my_action(sender, **kwargs):
    print(sender)
    print(kwargs)
    print('脱衣服泡个澡吧！')
```



### 触发信号

斯嘉丽烧好了一浴缸40度的洗澡水，杜兰特要开喝了。

```
bath_done.send(sender='斯嘉丽', amount='一浴缸', temperature='40°')
```

 

