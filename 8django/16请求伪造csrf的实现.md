[TOC]



# 正规的网站的操作

1. 正规网站在返回给用户含有post请求的页面 都附带了一个随机的字符串
2. 然后下一次用户在提交post请求的时候 会先校验该随机字符串是否存在并判断是否一致
3. 详情参考解决钓鱼网站的策略

# 跨站请求伪造（csrf）      钓鱼网站

​        就类似于你搭建了一个跟银行一模一样的web页面
​        用户在你的网站转账的时候输入用户名 密码 对方账户
​        银行里面的钱确实少了 但是发现收款人变了

# 最简单的原理

1. 你写的form表单中 用户的用户名  密码都会真实的提交给银行后台
2. 但是收款人的账户却不是用户填的 你暴露给用户的是一个没有name属性的input框
3. 你自己提前写好了一个隐藏的带有name和value的input框



### 真正的网站

前端

```html
<h1>真正的网站</h1>

<form action="" method="post">
    <p>username:<input type="text" name="username"></p>
    <p>target_username:<input type="text" name="target_username"> </p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
```

后端

```python
def transfer(request):#转账
    if request.method=="POST":
        username = request.POST.get('username')
        target_username = request.POST.get('target_username')
        money = request.POST.get('money')
        print('%s给%s转了%s钱'%(username,target_username,money))
    return render(request,'formm.html')
```

### 钓鱼网站

前端

```html
<h1>钓鱼的网站</h1>
<form action="http://127.0.0.1:8000/transfer/" method="post">
    <p>username:<input type="text" name="username"></p>
    <input type="text" name="target_username" value="sky" style="display: none">

    <p>target_username:<input type="text"></p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
```

后端

```python
def transfer(request):
    return render(request,'taansfer.html')
```



# 解决钓鱼网站的策略

1.  只要是用户想要提交post请求的页面 我在返回给用户的时候就提前设置好一个随机字符串
2.  当用户提交post请求的时候  我会自动先取查找是否有该随机字符串 
3. 如果有 正常提交
4. 如果没有  直接报403 

## form表单方法1

{% csrf_token %} 给出一个随机字符串，用来进行校验



```python
<h1>真正的网站</h1>

<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_username:<input type="text" name="target_username"> </p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191030225347079-1001569993.png)

## ajax方法1

第一种  自己再页面上先通过{% csrf_token %}获取到随机字符串  然后利用标签查找 

`data{'username':'jason','csrfmiddlewaretoken':$('[name="csrfmiddlewaretoken"]').val()},`            

```
<h1>真正的网站</h1>

<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_username:<input type="text" name="target_username"></p>
    <p>money:<input type="text" name="money"></p>
</form>
<button id="b1">发ajax</button>
<script>
    $('#b1').click(function () {
        $.ajax({
            url: "",
            type: "post",
            data: {"username": "sky", "csrfmiddlewaretoken": $('[name="csrfmiddlewaretoken"]').val()},
            success: function (data) {
                alert(data)

            }
        })

    })
</script>
```



## ajax方法2

第二种` data:{'username':'jason','csrfmiddlewaretoken':'{{ csrf_token }}'},`

```
<h1>真正的网站</h1>

<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_username:<input type="text" name="target_username"></p>
    <p>money:<input type="text" name="money"></p>
</form>
<button id="b1">发ajax</button>
<script>
    $('#b1').click(function () {
        $.ajax({
            url: "",
            type: "post",
            data: {"username": "sky", "csrfmiddlewaretoken": "{{csrf_token }}"},
            success: function (data) {
                alert(data)

            }
        })

    })
</script>
```



## ajax方法3

前端，导入下面的js

```html
<h1>真正的网站</h1>

<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_username:<input type="text" name="target_username"></p>
    <p>money:<input type="text" name="money"></p>
</form>
<button id="b1">发ajax</button>
<script src="/static/setpe.js"></script>
<script>
    $('#b1').click(function () {
        $.ajax({
            url: "",
            type: "post",
            data: {"username": "sky"},
            success: function (data) {
                alert(data)

            }
        })

    })
</script>
```





第三种
            拷贝下面js文件,新建文件夹static,然后静态文件配置，然后导入前端文件

js文件

```js
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





将上面的django的静态文件中，在html页面上通过导入该文件即可自动帮我们解决ajax提交post数据时校验csrf_token的问题，(导入该配置文件之前，需要先导入jQuery，因为这个配置文件内的内容是基于jQuery来实现的)

更多细节详见：[Djagno官方文档中关于CSRF的内容](https://docs.djangoproject.com/en/1.11/ref/csrf/)

# 校验指定的请求

csrf_exempt  只有两种装饰的方式

```
from django.views.decorators.csrf import csrf_exempt, csrf_protect
#csrf_exempt 不校验
# csrf_protect 校验
```



## 第一种
```
from django.views.decorators.csrf import csrf_exempt, csrf_protect
from django.utils.decorators import method_decorator
@csrf_exempt
def exem(request):
    return HttpResponse('exempt')

@csrf_protect
def pro(request):
    return HttpResponse('pro')
```

# CBV装饰器

除了csrf_exempt之外 所有的其他装饰器 在CBV上面都有三种方式

### 方式1

命名`@method_decorator(csrf_exempt,name='dispatch')`

```
from django.views.decorators.csrf import csrf_exempt, csrf_protect
from django.utils.decorators import method_decorator
from django.views import View
# 第一种
# @method_decorator(csrf_exempt,name='dispatch')
class MyCsrf(View):
    @method_decorator(csrf_protect)
    def dispatch(self, request, *args, **kwargs):
        return super().dispatch(request,*args,**kwargs)
    def get(self,request):
        return HttpResponse('hahaha')

    @method_decorator(csrf_protect)
    def post(self,request):
        return HttpResponse('post')
```

### 方式2

`@method_decorator(csrf_protect)`

```

    
from django.views.decorators.csrf import csrf_exempt, csrf_protect
from django.utils.decorators import method_decorator
from django.views import View

class MyCsrf(View):
    @method_decorator(csrf_protect)
    def dispatch(self, request, *args, **kwargs):
        return super().dispatch(request,*args,**kwargs)
    def get(self,request):
        return HttpResponse('hahaha')

    @method_decorator(csrf_protect)
    def post(self,request):
        return HttpResponse('post')
```

