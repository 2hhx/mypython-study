# 老男孩上海py11期代码发布直播课

## 昨日内容回顾

* 服务端如何给客户端推送消息

  **轮询**

  ```python
  """
  效率低，基本不用
  
  原理:
  	让客户端浏览器每隔一段时间主动朝服务端发送请求索要数据
  	
  缺点
  	消息延迟明显
  	消耗资源过大
  """
  ```

  **长轮询**

  ```python
  """
  兼容性非常好
  
  原理:
  	服务端给每个链接了的客户端维护一个队列，之后客户端通过ajax请求，去各自对应的队列中获取数据，没有则原地阻塞但是会设置超时时间和异常捕获机制，超时还没有让浏览器再次调用请求数据的函数(前端js中没有函数的递归一说，自己调用自己完全是可以的)
  	
  好处
  	消息基本零延迟
  	消耗资源较轮询要小得多
  """
  ```

  自己应用ajax和队列实现了长轮询完成一个简易版本的群聊功能

  ```html
  <!--前端如何书写关于websocket相关的代码-->
  <script>
  	var ws = new WebSocket('ws://127.0.0.1:8080/index/')
  </script>
  ```

  **websocket**

  ```python
  """
  http  			不加密传输
  https  			加密传输
  	短链接
  websocket  	加密传输
  	长链接
  
  
  它也是一个网络协议 并且基于该协议传输数据，数据是加密处理的
  """
  ```

  **原理剖析**

  ```python
  """
  1.握手环节:为了校验服务端是否支持websocket通信
  	链接服务器
  	浏览器会自动生成一个随机字符串 基于http协议将该字符串放在请求头中发送给服务端并且自己也保留一份
  	
  	服务端和浏览器都会对该随机字符串做以下处理
  	先与magic string做字符串拼接
  	之后再对结构进行加密处理sha1/base64
  	
  	服务端将处理好的结果基于http协议放在响应头里面返回给浏览器,浏览器会自动校验两者处理的结果是否一致 如果一致则建立链接，不一致则直接报错
  	
  2.收发数据
  	必备知识点
  		1.基于网络传输数据都是二进制格式
  		2.单位换算 8bit = 1bytes   二进制与十进制之间的换算
  	
  	先读取数据第二个字节的后七位(payload)
  	然后根据后七位大小做不同的处理
  	=127:基于当前位置继续往后读取8个字节(数据报10个字节)
  	=126:基于当前位置继续往后读取2个字节(数据报4个字节)
  	<=125:不再往后读取(数据报2个字节)
  	
  	上述操作完成后，继续往后读取固定长度4个字节的数据(masking-key)
  	依据masking-key与后面真实的加密数据进行位运算求出真实数据
  """
  ```

  **通过代码验证上述原理**

  无需掌握，因为操作太麻烦，我们直接使用封装好的模块，内部自动帮我们完成了上述的所有操作

* 以前知识点回顾

  1.ajax操作

  ```python
  $.ajax({
    url:'',
    type:'',
    data:{},
    dataType:"JSON",  # HttpResponse和JsonRepsonse有区别
    success:function(args){
      
    }
  })
  ```

  2.队列

  ```python
  """
  put()
  get()
  get(timeout=10) 
  """
  ```

  3.递归

  ```python
  # python中是有最大递归深度一说 官网说的是1000
  # python中没有尾递归优化
  """
  js中没有递归一说，函数自己调用自己完全是正常操作
  """
  ```

  4.modelform

  ```python
  """
  就是forms组件的加强版本
  """
  
  ```

* 基于之前的知识点实现简易版本的群聊功能

  自己参考昨日代码，最好自己动手实现一下

* 后端框架是否支持websocket

  ```python
  """
  并不是后端框架默认都支持websocket
  
  django
  	默认不支持
  	但是可以下载channles
  
  flask
  	默认不支持
  	但是可以下载geventwebsocket
  
  tornado
  	默认就是支持
  """
  
  ```

  **channles的安装**

  ```python
  """
  注意事项
  	1.不要安装最新版本的channles，建议按章2.3版本即可
  	2.python解释器建议使用3.6
  """
  pip3 install channels==2.3
  
  ```

# 今日内容

* django中如何实现websocket
* 基于django channels模块实现简易版本的群聊
* gojs插件
* paramiko模块
* gitpython模块

### django使用websocket

django默认只支持http协议

如果你想让django即支持http协议又支持websocket协议，则需要做以下配置

### 前期配置

**1.配置文件中注册channels应用**

```python
INSTALLED_APPS = [
    # 1 注册channles应用
    'channels'
]

```

**2.配置文件配置参数**

```python
# 2 配置启动需要的参数
ASGI_APPLICATION = 's11_day02.routing.application'
# ASGI_APPLICATION = '项目名同名的文件夹名.内部的py文件名(默认就叫routing).routing文件内的变量名(默认就叫application)'

```

**3.固定配置**

```python
# 在项目名同名的文件夹下创建routing.py文件并在该文件内提前写好以下代码
from channels.routing import ProtocolTypeRouter,URLRouter
from django.conf.urls import url
from app01 import consumers

application =ProtocolTypeRouter({
    'websocket':URLRouter([
        # websocket请求路由与视图函数的对应关系
      	url(r'^chat/$',consumers.ChatConsumer)
    ])
})

```

**总结：**配置完成后django由原来默认的wsgiref替换成asgi启动(asgi内部是基于达芙妮)

上述配置完成后 ，django就会同时支持http协议和websocket协议

```python
"""
原先基于http协议的路由与视图函数对应关系还是跟之前一样urls.py、views.py

针对websocket协议的路由与视图函数对应关系则需要在routing.py和consumers.py(在对应的应用下创建即可)
"""

```

#### 业务逻辑代码

```python
# consumers.py
from channels.generic.websocket import WebsocketConsumer
from channels.exceptions import StopConsumer



class ChatConsumer(WebsocketConsumer):
    def websocket_connect(self, message):
        """
        客户端请求链接之后自动触发
        :param message: 消息数据
        """
        pass
    
    def websocket_receive(self, message):
        """
        客户端浏览器发送消息来的时候自动触发
        :param message: 消息数据
        """
        pass
    
    def websocket_disconnect(self, message):
        """
        客户端断开链接之后自动触发
        :param message:  
        """
        pass

```

#### 基于channels完成聊天室

```python
"""
http协议
	index	>>> index函数
	访问：浏览器发送请求即可

websocket协议
	chat >>> ChatConsumer类(3个方法)
	访问:借助于new WebSocket对象
"""

```

```python
from channels.generic.websocket import WebsocketConsumer
from channels.exceptions import StopConsumer


consumer_object_list = []


class ChatConsumer(WebsocketConsumer):
    def websocket_connect(self, message):
        """
        客户端请求链接之后自动触发
        :param message: 消息数据
        """
        # print('请求链接')
        self.accept()  # 建立链接

        # 链接成功之后就将当前链接对象往列表中存一份
        consumer_object_list.append(self)


    def websocket_receive(self, message):
        """
        客户端浏览器发送消息来的时候自动触发
        :param message: 消息数据  {'type': 'websocket.receive', 'text': '你好啊 美女'}
        """
        # print(message)
        text = message.get('text')
        # 给客户端回复消息
        # self.send(text_data=text)
        # 我们要给所有的链接对象回复消息
        # 实现群发的简易版本
        for obj in consumer_object_list:
            obj.send(text_data=text)


    def websocket_disconnect(self, message):
        """
        客户端断开链接之后自动触发
        :param message:
        """
        # 客户端断开链接之后 应该将当前客户端对象从列表中移除
        consumer_object_list.remove(self)
        raise StopConsumer()  # 主动报异常 无需做处理 内部自动捕获

```

```html
<h1>聊天室</h1>
<div>
    <input type="text" id="text" placeholder="请输入">
    <input type="button" value="发送" onclick="sendMsg()">
    <input type="button" value="断开链接" onclick="close()">
</div>

<h1>聊天纪录</h1>
<div id="content">

</div>

<script>
    var ws = new WebSocket('ws://127.0.0.1:8000/chat/');
    // 1 发送消息 ws.send()
    // 2 握手成功之后 自动触发  ws.onopen
    // 3 服务端发送消息过来 自动触发  ws.onmessage
    // 4 断开链接 ws.close()

    // 0 握手成功之后自动触发
    ws.onopen = function () {
        alert('建立成功')
    };

    // 1 给服务端发送消息
    function sendMsg() {
        ws.send($('#text').val());
    }

    // 2 接受服务端发送过来的消息
    ws.onmessage = function (event) {  // event是对象
        var dataValue = event.data;  // 获取服务端的数据
        // 将数据动态渲染到页面上
        var pEle = $('<p>');
        pEle.text(dataValue);
        $('#content').append(pEle)
    };

    // 3 断开链接
    function close() {
        ws.close()
    }
</script>

```

**总结**

```python
"""
后端三个方法
	websocket_connect
	websocket_receive
	websocket_disconnect

前端四个方法
	// 1 发送消息 ws.send()
  // 2 握手成功之后 自动触发  ws.onopen
  // 3 服务端发送消息过来 自动触发  ws.onmessage
  // 4 断开链接 ws.close()
"""

```

注意我们上面的代码实现的群聊功能并不是真正的合理，后续如果想要真正实现群聊功能，官方提供了一个方法**channel-layers**模块，这个我们后面写项目的时候再讲



### gojs插件

是一个前端插件，可以通过代码动态的生成流程图，各自展示图

参考网址:<https://gojs.net/latest/index.html>

如果你想使用，需要先下载对应的文件

我们能用的到的其实就三个文件

```python
"""
gojs.js  			需要导入的js文件
go-debug.js   会帮你打印错误日志
	上面两个文件就类似于一个是压缩的一个是没有压缩的
	
Figures.js
	gojs.js内部只带了基本的图形 如果你想用更多的如下则需要导入该文件
"""

```

**基本使用**

先用div占据一块区域，之后在该区域内生成对应的图标及各种流程图

```html
<div id="myDiagramDiv" style="width:500px; height:350px; background-color: #DAE4E4;"></div>
<script src="go.js"></script>

<script>
  var $ = go.GraphObject.make;
  // 第一步：创建图表
  var myDiagram = $(go.Diagram, "myDiagramDiv"); // 创建图表，用于在页面上画图
  // 第二步：创建一个节点，内容为jason
  var node = $(go.Node, $(go.TextBlock, {text: "jason"}));
  // 第三步：将节点添加到图表中
  myDiagram.add(node)
</script>

```

#### 重要概念

* TextBlock  文本
* Shape  图形
* Node  图形与文本结合
* Link  箭头

**TextBlock**

```html
<div id="myDiagramDiv" style="width:500px; height:350px; background-color: #DAE4E4;"></div>
<script src="go.js"></script>
<script>
    var $ = go.GraphObject.make;
    // 第一步：创建图表
    var myDiagram = $(go.Diagram, "myDiagramDiv"); // 创建图表，用于在页面上画图
    var node1 = $(go.Node, $(go.TextBlock, {text: "jason"}));
    myDiagram.add(node1);
    var node2 = $(go.Node, $(go.TextBlock, {text: "jason", stroke: 'red'}));
    myDiagram.add(node2);
    var node3 = $(go.Node, $(go.TextBlock, {text: "jason", background: 'yellow'}));
    myDiagram.add(node3);
</script>

```

**Shape**

```html
<div id="myDiagramDiv" style="width:500px; height:350px; background-color: #DAE4E4;"></div>
<script src="go.js"></script>
<script src="Figures.js"></script>
<script>
    var $ = go.GraphObject.make;
    // 第一步：创建图表
    var myDiagram = $(go.Diagram, "myDiagramDiv"); // 创建图表，用于在页面上画图

    var node1 = $(go.Node,
        $(go.Shape, {figure: "Ellipse", width: 40, height: 40})
    );
     myDiagram.add(node1);

     var node2 = $(go.Node,
        $(go.Shape, {figure: "RoundedRectangle", width: 40, height: 40, fill: 'green',stroke:'red'})
    );
    myDiagram.add(node2);

    var node3 = $(go.Node,
        $(go.Shape, {figure: "Rectangle", width: 40, height: 40, fill: null})
    );
    myDiagram.add(node3);

    var node5 = $(go.Node,
        $(go.Shape, {figure: "Club", width: 40, height: 40, fill: 'red'})
    );
    myDiagram.add(node5);
</script>

```

**Node**

```html
<div id="myDiagramDiv" style="width:500px; height:350px; background-color: #DAE4E4;"></div>
<script src="go.js"></script>
<script src="Figures.js"></script>
<script>
    var $ = go.GraphObject.make;
    // 第一步：创建图表
    var myDiagram = $(go.Diagram, "myDiagramDiv"); // 创建图表，用于在页面上画图

    var node1 = $(go.Node,
         "Vertical",  // 垂直方向
        {
            background: 'yellow',
            padding: 8
        },
        $(go.Shape, {figure: "Ellipse", width: 40, height: 40,fill:null}),
        $(go.TextBlock, {text: "jason"})
    );
    myDiagram.add(node1);

    var node2 = $(go.Node,
        "Horizontal",  // 水平方向
        {
            background: 'white',
            padding: 5
        },
        $(go.Shape, {figure: "RoundedRectangle", width: 40, height: 40}),
        $(go.TextBlock, {text: "jason"})
    );
    myDiagram.add(node2);

    var node3 = $(go.Node,
        "Auto",  // 居中
        $(go.Shape, {figure: "Ellipse", width: 80, height: 80, background: 'green', fill: 'red'}),
        $(go.TextBlock, {text: "jason"})
    );
    myDiagram.add(node3);

</script>

```

**Link**

```html
<div id="myDiagramDiv" style="width:500px; min-height:450px; background-color: #DAE4E4;"></div>
    <script src="go.js"></script>
    <script>
        var $ = go.GraphObject.make;

        var myDiagram = $(go.Diagram, "myDiagramDiv",
            {layout: $(go.TreeLayout, {angle: 0})}
        ); // 创建图表，用于在页面上画图

        var startNode = $(go.Node, "Auto",
            $(go.Shape, {figure: "Ellipse", width: 40, height: 40, fill: '#79C900', stroke: '#79C900'}),
            $(go.TextBlock, {text: '开始', stroke: 'white'})
        );
        myDiagram.add(startNode);

        var downloadNode = $(go.Node, "Auto",
            $(go.Shape, {figure: "RoundedRectangle", height: 40, fill: 'red', stroke: '#79C900'}),
            $(go.TextBlock, {text: '下载代码', stroke: 'white'})
        );
        myDiagram.add(downloadNode);

        var startToDownloadLink = $(go.Link,
            {fromNode: startNode, toNode: downloadNode},
            $(go.Shape, {strokeWidth: 1}),
            $(go.Shape, {toArrow: "OpenTriangle", fill: null, strokeWidth: 1})
        );
        myDiagram.add(startToDownloadLink);
    </script>

```

上述代码只需要看懂即可，无需掌握

**数据绑定的方式**

```html
<div id="diagramDiv" style="width:100%; min-height:450px; background-color: #DAE4E4;"></div>

    <script src="go.js"></script>
    <script>
        var $ = go.GraphObject.make;
        var diagram = $(go.Diagram, "diagramDiv",{
            layout: $(go.TreeLayout, {
                angle: 0,
                nodeSpacing: 20,
                layerSpacing: 70
            })
        });

        // 先创建一个模版
        diagram.nodeTemplate = $(go.Node, "Auto",
            $(go.Shape, {
                figure: "RoundedRectangle",
                fill: 'yellow',
                stroke: 'yellow'
            }, new go.Binding("figure", "figure"), new go.Binding("fill", "color"), new go.Binding("stroke", "color")),
            $(go.TextBlock, {margin: 8}, new go.Binding("text", "text"))
        );

        // 先创建一个模版
        diagram.linkTemplate = $(go.Link,
            {routing: go.Link.Orthogonal},
            $(go.Shape, {stroke: 'yellow'}, new go.Binding('stroke', 'link_color')),
            $(go.Shape, {toArrow: "OpenTriangle", stroke: 'yellow'}, new go.Binding('stroke', 'link_color'))
        );

        // 数据格式是列表套字典 也就意味着可以从后端构造数据
        var nodeDataArray = [
            {key: "start", text: '开始', figure: 'Ellipse', color: "lightgreen"},
            {key: "download", parent: 'start', text: '下载代码', color: "lightgreen", link_text: '执行中...'},
            {key: "compile", parent: 'download', text: '本地编译', color: "lightgreen"},
            {key: "zip", parent: 'compile', text: '打包', color: "red", link_color: 'red'},
            {key: "c1", text: '服务器1', parent: "zip"},
            {key: "c11", text: '服务重启', parent: "c1"},
            {key: "c2", text: '服务器2', parent: "zip"},
            {key: "c21", text: '服务重启', parent: "c2"},
            {key: "c3", text: '服务器3', parent: "zip"},
            {key: "c31", text: '服务重启', parent: "c3"}
        ];
        diagram.model = new go.TreeModel(nodeDataArray);

        // 动态控制节点颜色变化  先找到节点之后改变
        var node = diagram.model.findNodeDataForKey("zip");
        diagram.model.setDataProperty(node, "color", "lightgreen");
    </script>

```

**如何去除自带的水印**

修改go.js/go-debug.js中的源码

1.查找一个字符串:7eba17a4ca3b1a8346，注释所在行

```js
/*a.kr=b.V[Ra("7eba17a4ca3b1a8346")][Ra("78a118b7")](b.V,Jk,4,4);*/

```

2.添加新的代码

```js
a.kr=function(){return true};

```

### paramiko模块

通过ssh链接服务器并执行想要的命令，类似于XShell

ansible(远程批量管理服务器)底层源码其实就是paramiko模块实现的

**安装**

```python
pip3 install paramiko

```

**使用**

前提须知:paramiko模块即支持用户名密码的方式也支持公钥私钥的方式操作服务器

```python
# 执行命令

# 创建链接对象
ssh = paramiko.SSHClient()
# 允许链接不在know_hosts文件中的主机
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
# 链接服务器
ssh.connect(hostname='172.16.219.170',port=22,username='root',password='jason123')

# 执行命令
stdin, stdout, stderr = ssh.exec_command('ip a')
"""
stdin  是用来输入额外的参数的   -y
stdout 命令的返回结果
stderr 错误的结果
"""
# 获取命令执行的结果
res = stdout.read()
print(res.decode('utf-8'))

# 关闭链接
ssh.close()



import paramiko

# 读取本地私钥
private_key = paramiko.RSAKey.from_private_key_file('a.txt')

# 创建SSH对象
ssh = paramiko.SSHClient()
# 允许连接不在know_hosts文件中的主机
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
# 连接服务器
ssh.connect(hostname='172.16.219.170', port=22, username='root', pkey=private_key)

# 执行命令
stdin, stdout, stderr = ssh.exec_command('ls /')
# 获取命令结果
result = stdout.read()
print(result.decode('utf-8'))
# 关闭连接
ssh.close()


# 上传下载文件

# 用户名和密码的方式
import paramiko

# 用户名和密码
transport = paramiko.Transport(('172.16.219.170', 22))
transport.connect(username='root', password='jason123')

sftp = paramiko.SFTPClient.from_transport(transport)

# 上传文件
# sftp.put("a.txt", '/data/tmp.txt')  # 注意上传文件到远程某个文件下 文件必须存在

# 下载文件
sftp.get('/data/tmp.txt', 'hahaha.txt')  # 将远程文件下载到本地并重新命令
transport.close()



# 公钥和私钥
import paramiko
private_key = paramiko.RSAKey.from_private_key_file('a.txt')
transport = paramiko.Transport(('172.16.219.170', 22))
transport.connect(username='root', pkey=private_key)
sftp = paramiko.SFTPClient.from_transport(transport)
# 将location.py 上传至服务器 /tmp/test.py
sftp.put('manage.py', '/data/test.py')

# 将remove_path 下载到本地 local_path
sftp.get('remove_path', 'local_path')
transport.close()

```

### SSHProxy类的封装

我想链接服务器执行三条命令，并且上传一个文件内容

大部分都会操作几次就链接几次服务器，效率较低，代码冗余

我们想实现一个类里面包含了执行命令和上传下载文件的操作

```python
# 类的代码无需掌握 只需要会拷贝使用即可
import paramiko


class SSHProxy(object):
    # 这里的参数 你可以再加公钥私钥的形式
    def __init__(self, hostname, port, username, password):
        self.hostname = hostname
        self.port = port
        self.username = username
        self.password = password
        self.transport = None

    def open(self):  # 给对象赋值一个上传下载文件对象连接
        self.transport = paramiko.Transport((self.hostname, self.port))
        self.transport.connect(username=self.username, password=self.password)

    def command(self, cmd):  # 正常执行命令的连接  至此对象内容就既有执行命令的连接又有上传下载链接
        ssh = paramiko.SSHClient()
        ssh._transport = self.transport

        stdin, stdout, stderr = ssh.exec_command(cmd)
        result = stdout.read()
        return result

    def upload(self, local_path, remote_path):
        sftp = paramiko.SFTPClient.from_transport(self.transport)
        sftp.put(local_path, remote_path)
        sftp.close()

    def close(self):
        self.transport.close()


    def __enter__(self):  # 对象执行with上下文会自动触发
        self.open()
        return self  # 这里发挥上面with语法内的as后面拿到的就是什么 
    
    
    def __exit__(self, exc_type, exc_val, exc_tb):  # with执行结束自动触发
        self.close()
        
"""

上述的封装操作在使用的使用 必须按照下面的顺序
obj = SSHProxy(...)

obj.open()  # 产生的对象必须要先执行open方法

obj.command('ls /')
obj.command('cat /data/tmp.txt')
obj.upload(...)
obj.upload(...)

obj.close()


文件操作
f = open()
f.close()
嫌上述操作麻烦 利用with上下文做处理了
with open() as f:
    pass
    
as后面的值由__enter__方法返回值决定 返回什么就是什么


# 一旦对象被执行with会自动触发对象内部的__enter__方法  with结束之后还会自动触发__exit__方法

obj = SSHProxy(1,2,3,4)
with obj as f:
    pass
    

封装之后按照下面的方式使用即可
with SSHProxy(....) as obj:
    obj.command()
    obj.command()
    obj.upload()
    obj.upload()
    obj.command()
"""

```

### 面试题

```python
class Foo(object):
    def __enter__(self):
        print('他进来了')
        return 123

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('他就这么出去了')

obj = Foo()
with obj as f:
    print(f)
    
# 面试题
"""
请在Context类中添加代码完成该类的实现
"""
class Context:
  pass
with Context() as ctx:
  ctx.do_something()

# 答案
class Context:
  def __enter__(self):
    	return self
   
 	def __exit__(self,*args,**kwargs):
    	pass
  
  def do_something(self):
    	pass

```























