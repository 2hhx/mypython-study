[TOC]



# choice参数

**主要是一种对应关系，存储到数据库中，而且用choice的时候，所有的情况都能够列举出来**

```python
choice参数
	可以用做
        用户性别
        用户学历
        用户工作状态
        客户来源
        ...
class Userinfo(models.Model):
    name = models.CharField(max_length=32)
    password = models.IntegerField(default=123)
    choice = (
        (1,'male'),
        (2,'female'),
        (3,'other')
    )
    gender = models.IntegerField(choices=choice)
        """
        1.如果我存的是上面元组中数字会怎么样
        2.如果我存的数字不在元组范围内又会怎样
        3.数字没有对应关系 是可以存的,如果存的数字不在范围内  拿到的还是数字本身
        """
  
```

前端获取

```python
{{ user_obj.get_gender_display }}
```



后端获取

```python
      
        
from app01 import models
ser_obj = models.Userinfo.objects.filter(pk=4).first()
        print(user_obj.username)
        print(user_obj.gender)
        # 针对choices字段 如果你想要获取数字所对应的中文 你不能直接点字段
        # 固定句式   数据对象.get_字段名_display()  当没有对应关系的时候 该句式获取到的还是数字
        print(user_obj.get_gender_display())
```

可以这样使用

```python
record_choices = (('checked', "已签到"),
                      ('vacate', "请假"),
                      ('late', "迟到"),
                      ('noshow', "缺勤"),
                      ('leave_early', "早退"),
                      )
        record = models.CharField("上课纪录", choices=record_choices, default="checked", 
    
    
        
        
        score_choices = ((100, 'A+'),
                     (90, 'A'),
                     (85, 'B+'),
                     (80, 'B'),
                     (70, 'B-'),
                     (60, 'C+'),
                     (50, 'C'),
                     (40, 'C-'),
                     (0, ' D'),
                     (-1, 'N/A'),
                     (-100, 'COPY'),
                     (-1000, 'FAIL'),
                     )
        score = models.IntegerField("本节成绩", choices=score_choices, default=-1)
```

# MTV与MVC模型

​        django号称是MTV框架,其实他还是MVC框架
​        MTV：
​            M：models
​            T: templates
​            V: views
​        MVC：
​            M：models
​            V: views
​            C: contronner(路由匹配)

# ajax

异步提交
            同步异步：描述的任务的提交方式
                同步:提交任务之后 原地等待任务的返回结果 期间不干任何事儿
                异步:提交任务之后 不愿地等待 直接执行下一行代码 任务的返回通过回调机制

阻塞非阻塞：程序的运行状态
            程序运行的三状态图    



​    ![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191028195236689-1808839816.png)

局部刷新一个页面 不是整体刷新 而是页面的某个地方局部刷新

Ajax是一门js的技术  基于原生js开发的，但是用原生的js写代码过于繁琐
    我们在学的时候 只学如何用jQuery实现ajax

​    AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。
（这一特点给用户的感受是在不知不觉中完成请求和响应过程）





## **示例**,计算器(简单加法)

**页面输入两个整数，通过AJAX传输到后端计算出结果并返回。**



前端

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--jquery-->
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
</head>
<body>
<hr>
<input type="text" id="t1">
+
<input type="text" id="t2">
=
<input type="text" id="t3">
<hr>

<p>
    <button id="d1" class="btn btn-success btn-sm">计算</button>

</p>



$('#b1').on('click', function () {
        // 朝后端提交post数据
        $.ajax({
            // 1.到底朝后端哪个地址发数据
            url: '',  // 专门用来控制朝后端提交数据的地址  不写默认就是朝当前地址提交
            // 2.到底发送什么请求
            type: 'post',  // 专门制定ajax发送的请求方式
            // 3.发送的数据到底是什么
            data: {'t1': $('#t1').val(), 't2': $('#t2').val()},
            data: {'username': 'jason', 'password': '123'},
            // 4.异步提交的任务 需要通过回调函数来处理
            success: function (data) {  // data形参指代的就是异步提交的返回结果
                // 通过DOM操作将内容 渲染到标签内容上
                $('#t3').val(data)
                alert(data)
            }
        })
    })
</body>
</html>
```

后端

```
def add(request):
    # print(request.is_ajax())#判断是否未ajax请求
    if request.is_ajax():
        # if request.method =='POST':
        #     pass
        if request.method=='POST':
            print(request.POST)
        t1 = request.POST.get('t1')
        t2 = request.POST.get('t2')
        res = int(t1)+int(t2)
        return HttpResponse(res)
    return render(request,'add.html')
```

# AJAX准备知识：JSON

**补充**：当我们得到一个json字符串的时候，为了能够更好的观看可以使用网址[json格式化](http://www.bejson.com/)

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191028170211829-1880175473.png)

## 什么是 JSON ？

- JSON 指的是 JavaScript 对象表示法（JavaScript Object Notation）
- JSON 是轻量级的文本数据交换格式
- JSON 独立于语言 *
- JSON 具有自我描述性，更易理解

\* JSON 使用 JavaScript 语法来描述数据对象，但是 JSON 仍然独立于语言和平台。JSON 解析器和 JSON 库支持许多不同的编程语言。

 啥都别多说了，上图吧！

![img](https://images2018.cnblogs.com/blog/1342004/201806/1342004-20180627141602223-599696124.png)

合格的json对象(json只认双引的字符串格式)：

```
["one", "two", "three"]
{ "one": 1, "two": 2, "three": 3 }
{"names": ["张三", "李四"] }
[ { "name": "张三"}, {"name": "李四"} ]　
```

不合格的json对象：



```
{ name: "张三", 'age': 32 }  // 属性名必须使用双引号
[32, 64, 128, 0xFFF] // 不能使用十六进制值
{ "name": "张三", "age": undefined }  // 不能使用undefined
{ "name": "张三",
  "birthday": new Date('Fri, 26 Aug 2011 07:13:10 GMT'),
  "getName":  function() {return this.name;}  // 不能使用函数和日期对象
}
```



## stringify与parse方法

JavaScript中关于JSON对象和字符串转换的两个方法：

JSON.parse(): 用于将一个 JSON 字符串转换为 JavaScript 对象(json只认双引的字符串格式)

```
JSON.parse('{"name":"Howker"}');
JSON.parse('{name:"Stack"}') ;   // 错误
JSON.parse('[18,undefined]') ;   // 错误
```

JSON.stringify(): 用于将 JavaScript 值转换为 JSON 字符串。

```
JSON.stringify({"name":"Tonny"})
```

# 编码格式种类总结

1. 前后端传输数据的编码格式
           编码格式种类
               1.urlencoded
               2.formdata
               3.application/json

   ## form表单

   ```
       form表单
           form表单默认的编码格式是urlencoded
               urlencoded编码格式的数据特点
                   username=jason&password=123&age=18
               # django后端针对符合urlencoded数据格式 会自动解析 并给你封装到request.POST中
               
               # 你可以通过制定enctype参数来修改form表单提交数据的编码格式
               # form表单传输文件的时候  编码格式就必须有默认的改为formdata
               """
               即可以传普通的键值对也可以上传文件
               
               django后端针对只要是符合urlencoded格式的数据都会自动解析放到request.POST
               针对文件数据 会解析并放到request.FILES
               """
   ```

   ## ajax 编码

   ```
       ajax  默认的数据编码格式也是urlencoded
           也就意味着ajax发送post请求django后端默认也是通过request.POST获取数据
           
       ajax发送json格式数据
           如何查看前端提交数据的编码格式？
               在请求头中有一个content-Type参数
           """
           前后端交互数据的时候 一定要做到数据个编码格式的一致性
           """
           1.需要手动指定编码格式
               contentType:'application/json'
           2.一定要确保数据也是符合json格式的
               data:JSON.stringify({'username':'jason'})
           
           # django后端针对json格式的数据 是不会做任何处理的  会原封不动的放在request.body中
           你可以手动去处理获取数据
               
               1.将bytes类型转成json格式字符串
               2.利用json模块json.loads反序列化出来
   ```



# ajax传json格式的数据

django后端针对json格式的数据，不会自动帮助分析，会原封不动的给封装到request.body中，这个时候需要手动去取出数据。

**注意**

1.指定contentType参数                contentType:'application/json',
 2.要将你发送的数据 确保是json格式的           data:JSON.stringify({'username':'jason','password':'123'})



后端

```python
import json
def add(request):
   if request.is_ajax():
    if request.method=='POST':
        print(request.POST)
        print(request.FILES)
        print(request.body)#b'{"t1":"chen","t2":"456"}'

        print(type(request.body))#<class 'bytes'>
        json_by = request.body
        json_str = str(json_by,encoding='utf8')
        json_dict = json.loads(json_str)
        print(json_dict,type(json_dict))#后端获取字典
#{'t1': 'chen', 't2': '456'} <class 'dict'>
    return render(request,'add.html')

```

前端

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--jquery-->
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
</head>
<body>
<hr>
<form action="" method="post" enctype="">
    <input type="text" id="t1" name="username">
    <input type="text" id="t2" name="password">
    <input type="file" name="myfile">

</form>
    <button id="d1" class="btn btn-success btn-sm">计算</button>
<hr>



<script>
    $('#d1').on('click', function () {
        $.ajax({
            url: '',
            type: 'post',
            contentType: 'application/json',#指定参数为json类型
            data:JSON.stringify({'t1': $('#t1').val(), 't2': $('#t2').val(),}),#要将你发送的数据 确保是json格式的
            success: function (data) {
                alert(data)
            }
        })

    })

</script>
</body>
</html>
```



# ajax传文件

需要注意

```python
AJAX传文件
        需要利用内置对象 Formdata
        该对象既可以传普通的键值 也可以传文件
        
        # 获取input获取用户上传文件的文件的内容
        // 1.先通过jquery查找到该标签
        // 2.将jquery对象转换成原生的js对象
        // 3.利用原生js对象的方法 直接获取文件内容
        $('#t3')[0].files[0]

ajax传文件需要注意的事项
        1.利用formdata对象 能够简单的快速传输数据 (普通键值 + 文件)
        2.有几个参数
            data:formdata对象
            contentType:false
            processData:false
```

```python
contentType前后端传输数据编码格式
    
        form表单 默认的提交数据的编码格式是urlencoded
            urlencoded
                username=admin&password=123这种就是符合urlencoded数据格式
                
                django后端针对username=admin&password=123的urlencoded数据格式会自动解析
                将结果打包给request.POST 用户只需要从request.POST即可获取对应信息
               
            formdata
                django后端针对formdata格式类型数据 也会自动解析
                但是不会方法request.POST中而是给你放到了request.FILES中
            
        ajax  ajax默认的提交数据的编码格式也是urlencoded
            username=jason&password=123
            
        总结:django后端针对不同的编码格式数据 会有不同的处理机制以及不同的获取该数据的方法
```

前端

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
</head>
<body>
<input type="text" name="username" id="t1">
<input type="text" name="password" id="t2">
<input type="file" name="myfile" id="t3">
<button id="b1">提交</button>

<script>
    $('#b1').click(function () {
        // 1.先生成一个formdata对象
        var myFormData = new FormData();
        // 2.朝对象中添加普通的键值
        myFormData.append('username',$("#t1").val());
        myFormData.append('password',$("#t2").val());
        // 3.朝对象中添加文件数据
        // 1.先通过jquery查找到该标签
        // 2.将jquery对象转换成原生的js对象
        // 3.利用原生js对象的方法 直接获取文件内容
        myFormData.append('myfile',$('#t3')[0].files[0]);
        $.ajax({
            url:'',
            type:'post',
            data:myFormData,  // 直接丢对象
            // ajax传文件 一定要指定两个关键性的参数
            contentType:false,  // 不用任何编码 因为formdata对象自带编码 django能够识别该对象
            processData:false,  // 告诉浏览器不要处理我的数据 直接发就行
            success:function (data) {
                alert(data)
            }
        })
    })
</script>


</body>
</html>
```

后端

```python
def upload(request):
    if request.is_ajax():
        if request.method == 'POST':
            print(request.POST)
            print(request.FILES)
            file_obj = request.FILES.get('myfile')
            with open('1.jpg', 'wb') as f:
                for line in file_obj:  # file_obj你可以直接看成文件句柄f
                    f.write(line)

            return HttpResponse('收到了')
    return render(request,'upload.html')
```

# 序列化

## Django内置的serializers

将用户表的数据 查询出来 返回给前端
    给前端的是一个大字典 字典里面的数据的一个个的字段,作为接口，并且可以被其他的语言进行识别

### 数据在前端展示的三种方式

数据准备

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191028171416957-1100580018.png)

## 比较1

后端

```python
def index(request):
    user_obj_list = models.Userinfo.objects.all()
    return render(request,'index.html',locals())
```

前端

```python
{% for user_obj in user_obj_list %}
    {{ user_obj.name }}
    {{ user_obj.password }}
    {{ user_obj.get_gender_display }}

{% endfor %}
```

## 比较2

后端

```python
from django.core import serializers
def index(request):
    user_obj_list = models.Userinfo.objects.all()
    # user_name = user_obj_list.first().name
    # password = user_obj_list.first().password
    # user_list = []
    # for user_obj in user_obj_list:
    #     user_list.append({
    #         'username':user_obj.name,
    #         'password':user_obj.password,
    #         'gender':user_obj.get_gender_display()
    #     })
    res = serializers.serialize('json',user_obj_list)
    print(res)
    return render(request,'index.html',locals())
    
```



前端

```python
{% for user in user_list %}
    {{ user.username }}
    {{ user.password }}
    {{ user.gender }}

{% endfor %}
```



## 真正的序列化3

后端

```python
from django.core import serializers
def index(request):
    user_obj_list = models.Userinfo.objects.all()

    res1 = serializers.serialize('json',user_obj_list)
    res = eval(res1)#Z强制转换
    # return HttpResponse(res1)
    return render(request,'index.html',locals())
```

前端

```python
{% for foo in res %}
    {{ foo.pk }}
    {{ foo.fields.name }}
    {{ foo.fields.password }}
    {{ foo.fields.gender }}
    {% if foo.fields.gender == 1 %}
        男
    {% elif foo.fields.gender == 2 %}
        女
    {% elif foo.fields.gender == 3 %}
        其他
    {% endif %}
    <br>


{% endfor %}
```

# SweetAlert插件示例

![img](https://images2018.cnblogs.com/blog/867021/201804/867021-20180407235320541-386911677.gif)

点击下载[Bootstrap-sweetalert项目](https://github.com/lipis/bootstrap-sweetalert)。



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    {% load static %}
    <link rel="stylesheet" href="{% static 'dist/sweetalert.css' %}">
    <script src="{% static 'dist/sweetalert.min.js' %}"></script>
    <style>
        div.sweet-alert h2 {
            padding-top: 10px;
        }
    </style>
</head>
<body>
<div class="container-fluid">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <h2>数据展示</h2>
            <table class="table table-hover table-striped table-bordered">
                <thead>
                    <tr>
                        <th>序号</th>
                        <th>用户名</th>
                        <th>密码</th>
                        <th>性别</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>
                    {% for user_obj in user_queryset %}
                        <tr>
                            <td>{{ forloop.counter }}</td>
                            <td>{{ user_obj.username }}</td>
                            <td>{{ user_obj.password }}</td>
                            <td>{{ user_obj.get_gender_display }}</td>
                            <td>
                                <a href="#" class="btn btn-primary btn-sm">编辑</a>
                                <a href="#" class="btn btn-danger btn-sm cancel" delete_id="{{ user_obj.pk }}">删除</a>
                            </td>
                        </tr>
                    {% endfor %}
                </tbody>


            </table>
        </div>
    </div>
</div>


<script>
    $('.cancel').click(function () {
        var $btn = $(this);
        swal({
          title: "你确定要删吗?",
          text: "你要是删了,你就准备好跑路吧!",
          type: "warning",
          showCancelButton: true,
          confirmButtonClass: "btn-danger",
          confirmButtonText: "对,老子就要删!",
          cancelButtonText: "算了,算了!",
          closeOnConfirm: false,
            showLoaderOnConfirm: true
        },
        function(){
            $.ajax({
                url:'',
                type:'post',
                data:{'delete_id':$btn.attr('delete_id')},
                success:function (data) {
                    if (data.code==1000){
                        swal(data.msg, "你可以回去收拾行李跑路了.", "success");
                        // 1.直接刷新页面
                        {#window.location.reload()#}
                        // 2.通过DOM操作 实时删除
                        $btn.parent().parent().remove()
                    }else{
                        swal("发生了未知错误!", "我也不知道哪里错了.", "info");
                    }
                }
            });

        });
    })
</script>


</body>
</html>
```



后端

```python
"""
当你是用ajax做前后端 交互的时候 
你可以考虑返回给前端一个大字典
"""
import time
from django.http import JsonResponse
def sweetajax(request):
    if request.method == 'POST':
        back_dic = {"code":1000,'msg':''}
        delete_id = request.POST.get('delete_id')
        models.Userinfo.objects.filter(pk=delete_id).delete()
        back_dic['msg'] = '后端传来的:真的被我删了'
        time.sleep(3)
        return JsonResponse(back_dic)
    user_queryset = models.Userinfo.objects.all()
    return render(request,'sa.html',locals())

```



上面这个二次确认的动态框样式，你也可以直接应用到你的项目中

提醒事项：

1.上述的样式类部分渲染的样式来自于bootstrap中，所有建议在使用上述样式时，将bootstrap的js和css也导入了，这样的情况下，页面效果就不会有任何问题

2.弹出的上述模态框中，可能字体会被图标掩盖一部分，可通过调整字体的上外边距来解决

# 和XML的比较

JSON 格式于2001年由 Douglas Crockford 提出，目的就是取代繁琐笨重的 XML 格式。

JSON 格式有两个显著的优点：书写简单，一目了然；符合 JavaScript 原生语法，可以由解释引擎直接处理，不用另外添加解析代码。所以，JSON迅速被接受，已经成为各大网站交换数据的标准格式，并被写入ECMAScript 5，成为标准的一部分。

XML和JSON都使用结构化方法来标记数据，下面来做一个简单的比较。

用XML表示中国部分省市数据如下：

```
<?xml version="1.0" encoding="utf-8"?>
<country>
    <name>中国</name>
    <province>
        <name>黑龙江</name>
        <cities>
            <city>哈尔滨</city>
            <city>大庆</city>
        </cities>
    </province>
    <province>
        <name>广东</name>
        <cities>
            <city>广州</city>
            <city>深圳</city>
            <city>珠海</city>
        </cities>
    </province>
    <province>
        <name>台湾</name>
        <cities>
            <city>台北</city>
            <city>高雄</city>
        </cities>
    </province>
    <province>
        <name>新疆</name>
        <cities>
            <city>乌鲁木齐</city>
        </cities>
    </province>
</country>
```



用JSON表示如下：





```
{
    "name": "中国",
    "province": [{
        "name": "黑龙江",
        "cities": {
            "city": ["哈尔滨", "大庆"]
        }
    }, {
        "name": "广东",
        "cities": {
            "city": ["广州", "深圳", "珠海"]
        }
    }, {
        "name": "台湾",
        "cities": {
            "city": ["台北", "高雄"]
        }
    }, {
        "name": "新疆",
        "cities": {
            "city": ["乌鲁木齐"]
        }
    }]
}
```



由上面的两端代码可以看出，JSON 简单的语法格式和清晰的层次结构明显要比 XML 容易阅读，并且在数据交换方面，由于 JSON 所使用的字符要比 XML 少得多，可以大大得节约传输数据所占用得带宽。

# AJAX简介

AJAX（Asynchronous Javascript And XML）翻译成中文就是“异步的Javascript和XML”。即使用Javascript语言与服务器进行异步交互，传输的数据为XML（当然，传输的数据不只是XML）。

AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。（这一特点给用户的感受是在不知不觉中完成请求和响应过程）

AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。

- 同步交互：客户端发出一个请求后，需要等待服务器响应结束后，才能发出第二个请求；
- 异步交互：客户端发出一个请求后，无需等待服务器响应结束，就可以发出第二个请求。

## **AJAX****常见应用情景**

搜索引擎根据用户输入的关键字，自动提示检索关键字。

还有一个很重要的应用场景就是注册时候的用户名的查重。

其实这里就使用了AJAX技术！当文件框发生了输入变化时，使用AJAX技术向服务器发送一个请求，然后服务器会把查询到的结果响应给浏览器，最后再把后端返回的结果展示出来。

- 整个过程中页面没有刷新，只是刷新页面中的局部位置而已！
- 当请求发出后，浏览器还可以进行其他操作，无需等待服务器的响应！

![img](https://images2018.cnblogs.com/blog/1342004/201806/1342004-20180627142808460-58688385.png)

当输入用户名后，把光标移动到其他表单项上时，浏览器会使用AJAX技术向服务器发出请求，服务器会查询名为lemontree7777777的用户是否存在，最终服务器返回true表示名为lemontree7777777的用户已经存在了，浏览器在得到结果后显示“用户名已被注册！”。

- 整个过程中页面没有刷新，只是局部刷新了；
- 在请求发出后，浏览器不用等待服务器响应结果就可以进行其他操作；

## **AJAX****的优缺点**

#### 优点：

- AJAX使用JavaScript技术向服务器发送异步请求；
- AJAX请求无须刷新整个页面；
- 因为服务器响应内容不再是整个页面，而是页面中的部分内容，所以AJAX性能高； 
- 两个关键点:1.局部刷新，2.异步请求

# jQuery实现的AJAX

最基本的jQuery发送AJAX请求示例：





```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <style>
        .hide {
            display: none;
        }
    </style>
</head>
<body>
<p><input type="text" class="user"><span class="hide" style="color: red">用户名已存在</span></p>

<script src="/static/jquery-3.3.1.min.js"></script>
{#下面这一项是基于jQuery的基础上自动给我们的每一个ajax绑定一个请求头信息，类似于form表单提交post数据必须要有的csrf_token一样#}
{#否则我的Django中间件里面的校验csrf_token那一项会认为你这个请求不是合法的，阻止你的请求#}
<script src="/static/setup_Ajax.js"></script>
<script>
    //给input框绑定一个失去焦点的事件
    $('.user').blur(function () {
        //$.ajax为固定用法，表示启用ajax
        $.ajax({
            //url后面跟的是你这个ajax提交数据的路径，向谁提交，不写就是向当前路径提交
            url:'',
            //type为标定你这个ajax请求的方法
            type:'POST',
            //data后面跟的就是你提交给后端的数据
            data:{'username':$(this).val()},
            //success为回调函数，参数data即后端给你返回的数据
            success:function (data) {
                ret=JSON.parse(data);
                if (ret['flag']){
                    $('p>span').removeClass('hide');
                }
            }
        })
    });
</script>
</body>
</html>
```



## views.py：





```
def index(request):
    if request.method=='POST':
        ret={'flag':False}
        username=request.POST.get('username')
        if username=='JBY':
            ret['flag']=True
            import json
            return HttpResponse(json.dumps(ret))
    return render(request,'index.html')
```



## $.ajax参数

data参数中的键值对，如果值值不为字符串，需要将其转换成字符串类型。



```
$("#b1").on("click", function () {
    $.ajax({
      url:"/ajax_add/",
      type:"GET",
      data:{"i1":$("#i1").val(),"i2":$("#i2").val(),"hehe": JSON.stringify([1, 2, 3])},
      success:function (data) {
        $("#i3").val(data);
      }
    })
  })
```



# JS实现AJAX(了解)





```
var b2 = document.getElementById("b2");
  b2.onclick = function () {
    // 原生JS
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.open("POST", "/ajax_test/", true);
    xmlHttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    xmlHttp.send("username=q1mi&password=123456");
    xmlHttp.onreadystatechange = function () {
      if (xmlHttp.readyState === 4 && xmlHttp.status === 200) {
        alert(xmlHttp.responseText);
      }
    };
  };
```



# AJAX请求如何设置csrf_token

不论是ajax还是谁，只要是向我Django提交post请求的数据，都必须校验csrf_token来防伪跨站请求，那么如何在我的ajax中弄这个csrf_token呢，我又不像form表单那样可以在表单内部通过一句{% csrf_token %}就搞定了......

### 方式1

通过获取隐藏的input标签中的csrfmiddlewaretoken值，放置在data中发送。



```
$.ajax({
  url: "/cookie_ajax/",
  type: "POST",
  data: {
    "username": "Tonny",
    "password": 123456,
    "csrfmiddlewaretoken": $("[name = 'csrfmiddlewaretoken']").val()  // 使用JQuery取出csrfmiddlewaretoken的值，拼接到data中
  },
  success: function (data) {
    console.log(data);
  }
})
```



### 方式2

通过获取返回的cookie中的字符串 放置在请求头中发送。

注意：需要引入一个jquery.cookie.js插件。





```
$.ajax({
  url: "/cookie_ajax/",
  type: "POST",
  headers: {"X-CSRFToken": $.cookie('csrftoken')},  // 从Cookie取csrf_token，并设置ajax请求头
  data: {"username": "Q1mi", "password": 123456},
  success: function (data) {
    console.log(data);
  }
})
```



### 方式3

或者用自己写一个getCookie方法：

```
function getCookie(name) {
    var cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++) {
            var cookie = jQuery.trim(cookies[i]);
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}
var csrftoken = getCookie('csrftoken');
```



每一次都这么写太麻烦了，可以使用$.ajaxSetup()方法为ajax请求统一设置。



```
function csrfSafeMethod(method) {
  // these HTTP methods do not require CSRF protection
  return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}

$.ajaxSetup({
  beforeSend: function (xhr, settings) {
    if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
      xhr.setRequestHeader("X-CSRFToken", csrftoken);
    }
  }
});
```

将下面的文件配置到你的Django项目的静态文件中，在html页面上通过导入该文件即可自动帮我们解决ajax提交post数据时校验csrf_token的问题，(导入该配置文件之前，需要先导入jQuery，因为这个配置文件内的内容是基于jQuery来实现的)

更多细节详见：[Djagno官方文档中关于CSRF的内容](https://docs.djangoproject.com/en/1.11/ref/csrf/)

## 练习（用户名是否已被注册）

### 功能介绍

在注册表单中，当用户填写了用户名后，把光标移开后，会自动向服务器发送异步请求。服务器返回这个用户名是否已经被注册过。

### 案例分析

- 页面中给出注册表单；
- 在username input标签中绑定onblur事件处理函数。
- 当input标签失去焦点后获取 username表单字段的值，向服务端发送AJAX请求；
- django的视图函数中处理该请求，获取username值，判断该用户在数据库中是否被注册，如果被注册了就返回“该用户已被注册”，否则响应“该用户名可以注册”。

答案就在前面的示例中，看你能不能找到了......
