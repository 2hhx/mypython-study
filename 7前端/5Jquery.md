[TOC]



# 前端之Jquery



# jQuery

## jQuery介绍

1. jQuery是一个轻量级的、兼容多浏览器的JavaScript库。
2. jQuery使用户能够更方便地处理HTML Document、Events、实现动画效果、方便地进行Ajax交互，能够极大地简化JavaScript编程。它的宗旨就是：“Write less, do more.“

## jQuery的优势

1. 一款轻量级的JS框架。jQuery核心js文件才几十kb，不会影响页面加载速度。
2. 丰富的DOM选择器,jQuery的选择器用起来很方便，比如要找到某个DOM对象的相邻元素，JS可能要写好几行代码，而jQuery一行代码就搞定了，再比如要将一个表格的隔行变色，jQuery也是一行代码搞定。
3. 链式表达式。jQuery的链式操作可以把多个操作写在一行代码里，更加简洁。
4. 事件、样式、动画支持。jQuery还简化了js操作css的代码，并且代码的可读性也比js要强。
5. Ajax操作支持。jQuery简化了AJAX操作，后端只需返回一个JSON格式的字符串就能完成与前端的通信。
6. 跨浏览器兼容。jQuery基本兼容了现在主流的浏览器，不用再为浏览器的兼容问题而伤透脑筋。
7. 插件扩展开发。jQuery有着丰富的第三方的插件，例如：树形菜单、日期控件、图片切换插件、弹出窗口等等基本前端页面上的组件都有对应插件，并且用jQuery插件做出来的效果很炫，并且可以根据自己需要去改写和封装插件，简单实用。

## jQuery内容：

1. 选择器
2. 筛选器
3. 样式操作
4. 文本操作
5. 属性操作
6. 文档处理
7. 事件
8. 动画效果
9. 插件
10. each、data、Ajax

下载链接：[jQuery官网](https://jquery.com/)

中文文档：[jQuery AP中文文档](http://jquery.cuishifeng.cn/)

## jQuery版本

- 1.x：兼容IE678,使用最为广泛的，官方只做BUG维护，功能不再新增。因此一般项目来说，使用1.x版本就可以了，最终版本：1.12.4 (2016年5月20日)
- 2.x：不兼容IE678，很少有人使用，官方只做BUG维护，功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x，最终版本：2.2.4 (2016年5月20日)
- 3.x：不兼容IE678，只支持最新的浏览器。需要注意的是很多老的jQuery插件不支持3.x版。目前该版本是官方主要更新维护的版本。

*维护IE678是一件让人头疼的事情，一般我们都会额外加载一个CSS和JS单独处理。值得庆幸的是使用这些浏览器的人也逐步减少，PC端用户已经逐步被移动端用户所取代，如果没有特殊要求的话，一般都会选择放弃对678的支持。*

## jQuery对象

**jQuery对象**就是通过jQuery包装DOM对象后产生的对象。**jQuery对象**是 jQuery独有的。如果一个对象是 **jQuery对象**，那么它就可以使用**jQuery**里的方法：例如$(“#i1”).html()。

`$("#i1").html()`的意思是：获取id值为 `i1`的元素的html代码。其中 `html()`是jQuery里的方法。

相当于： `document.getElementById("i1").innerHTML;`

虽然 `jQuery对象`是包装 `DOM对象`后产生的，但是 `jQuery对象`无法使用 `DOM对象`的任何方法，同理 `DOM对象`也没不能使用 `jQuery`里的方法。

一个约定，我们在声明一个jQuery对象变量的时候在变量名前面加上$：

```js
var $variable = jQuery对像
var variable = DOM对象
$variable[0]//jQuery对象转成DOM对象
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016213933143-481374763.png)

拿上面那个例子举例，jQuery对象和DOM对象的使用：

```js
$("#i1").html();//jQuery对象可以使用jQuery的方法
$("#i1")[0].innerHTML;// DOM对象使用DOM的方法
```



![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016214011948-974800237.png)

## jQuery基础语法

```
$(selector).action()
```

## 例子

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>jquery</title>
    <!--jquery-->
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>

        div{height: 100px}
    </style>
</head>
<body>
<div class="a" id="a1">第一个div

    <div class="b" id="b1" name="sky">第一个div下的div1</div>
    <span class="c" id="c1">第一个div下的span</span>
</div>
<br>
<br>
<br>
<div class="d" id="d1">第二个div
    <input type="text" id="i1">
    <input type="checkbox" id="i2" checked>嘿嘿
    <input type="text" id="i3">
    <input type="radio" id="i4" checked>男
</div>
<select name="" id="s1">
        <option value="" id="o1">上海</option>
        <option value="" id="o2">北京</option>
        <option value="" id="o3" selected>南京</option>
</select>
<div class="e" id="e1" name="ocean">第三个div</div>
<div class="f" id="f1">第四个div</div>

</body>
</html>
```



## 查找标签

### 基本选择器

**id选择器：**

```
$("#id")
```

**标签选择器：**

```
$("tagName")
```

**class选择器：**

```
$(".className")
```

**配合使用：**

```
$("div.c1")  // 找到有c1 class类的div标签
```

**所有元素选择器：**

```
$("*")
```

**组合选择器：**

```
$("#id, .className, tagName")

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016162301528-646639493.png)

### **层级选择器：**

*x和y可以为任意选择器*

```
$("x y");// x的所有后代y（子子孙孙）
$("x > y");// x的所有儿子y（儿子）
$("x + y")// 找到所有紧挨在x后面的y
$("x ~ y")// x之后所有的兄弟y

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016162739481-1622929006.png)

### **基本筛选器：**

```
:first // 第一个
:last // 最后一个
:eq(index)// 索引等于index的那个元素
:even // 匹配所有索引值为偶数的元素，从 0 开始计数
:odd // 匹配所有索引值为奇数的元素，从 0 开始计数
:gt(index)// 匹配所有大于给定索引值的元素
:lt(index)// 匹配所有小于给定索引值的元素
:not(元素选择器)// 移除所有满足not条件的标签
:has(元素选择器)// 选取所有包含一个或多个标签在其内的标签(指的是从后代元素找)，并且只能作用与标签。

```



**例子：**

```
$("div:has(h1)")// 找到所有后代中有h1标签的div标签
$("div:has(.c1)")// 找到所有后代中有c1样式类的div标签
$("li:not(.c1)")// 找到所有不包含c1样式类的li标签
$("li:not(:has(a))")// 找到所有后代中不含a标签的li标签

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016173120838-517122418.png)

练习：

自定义模态框，使用jQuery实现弹出和隐藏功能。



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>自定义模态框</title>
    <!--jquery-->
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>
        .cover {
            position: fixed;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            background-color: peachpuff;
            z-index: 99;
        }

        .moda {
            width: 600px;
            height: 400px;
            background-color: yellow;
            position: fixed;
            left: 50%;
            top: 50%;
            margin-left: -300px;
            margin-top: -200px;
            z-index: 100;
        }

        .hide {
            display: none;
        }
    </style>
</head>
<body>
<input type="button" value="弹" id="i0">
<div class="cover hide">

</div>
<div class="moda hide">
    <label for="i1">姓名</label>
    <input type="text" id="i1">
    <label for="i2"></label>
    <input type="text" id="i2">
    <input type="button" id="i3" value="关闭">
</div>
<script>
    var tButton = $("#i0")[0];
    tButton.onclick = function () {
        var coverEle = $(".cover")[0];
        var modalEge = $(".moda")[0];

        $(coverEle).removeClass("hide");
        $(modalEge).removeClass("hide");
    };
    var cButton = $("#i3")[0];
    cButton.onclick = function () {
        var coverEle = $(".cover")[0];
        var modalEge = $(".moda")[0];

        $(coverEle).addClass("hide");
        $(modalEge).addClass("hide");
    }
</script>
</body>
</html>
```



### **属性选择器：**

```
[attribute]
[attribute=value]// 属性等于
[attribute!=value]// 属性不等于

```

**例子：**

```
// 示例
<input type="text">
<input type="password">
<input type="checkbox">
$("input[type='checkbox']");// 取到checkbox类型的input标签
$("input[type!='text']");// 取到类型不是text的input标签

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016173442790-1342868910.png)

### **表单筛选器**：

```
:text
:password:file
:radio
:checkbox
:submit
:reset
:button

```



**例子：**

```
$(":checkbox")  // 找到所有的checkbox

```

表单对象属性:

```
:enabled
:disabled
:checked
:selected

```



 找到被选中的option：



```
<select id="s1">
  <option value="beijing">北京市</option>
  <option value="shanghai">上海市</option>
  <option selected value="guangzhou">广州市</option>
  <option value="shenzhen">深圳市</option>
</select>

$(":selected")  // 找到所有被选中的option

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016173833020-720354253.png)

## 筛选器方法

下一个元素：

```js
$("#id").next()
$("#id").nextAll()
$("#id").nextUntil("#i2")
从某一个元素到下面某一个元素之间有哪几个元素

```



上一个元素：

```
$("#id").prev()
$("#id").prevAll()
$("#id").prevUntil("#i2")
从某一个元素到上面面某一个元素之间有哪几个元素

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016180411480-1096413118.png)

父亲元素：

```
$("#id").parent()
$("#id").parents()  // 查找当前元素的所有的父辈元素$("#id").parentsUntil() // 查找当前元素的所有的父辈元素，直到遇到匹配的那个元素为止。

```

儿子和兄弟元素：

```
$("#id").children();// 儿子们
$("#id").siblings();// 兄弟们

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016180732003-64320556.png)

查找

搜索所有与指定表达式匹配的元素。这个函数是找出正在处理的元素的后代元素的好方法。

```
$("div").find("span")

```

等价于$("div span")

筛选

筛选出与指定表达式匹配的元素集合。这个方法用于缩小匹配的范围。用逗号分隔多个表达式。

```
$("div").filter(".c1")  // 从结果集中过滤出有c1样式类的

```

等价于 $("div.c1")

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016181039934-1417465445.png)

补充：

```
.first() // 获取匹配的第一个元素
.last() // 获取匹配的最后一个元素
.not() // 从匹配元素的集合中删除与指定表达式匹配的元素
.has() // 保留包含特定后代的元素，去掉那些不含有指定后代的元素。
.eq() // 索引值等于指定值的元素

```

### 示例：左侧菜单



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>左侧菜单示例</title>
    <!--jquery-->
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>
        .left{
            position: fixed;
            left: 0;
            top: 0;
            width: 20%;
            height: 100%;
            background-color: rgb(47,53,61);
        }
        .right{
            width: 80%;
            height: 100%;
        }
        .menu{
            color: white;
        }
        .title{
            text-align: center;
            padding: 10px 15px;
            border-bottom: 1px solid #ff5b28;
        }
        .items{
            background-color: palevioletred;
        }
        .item{
            padding: 5px 10px;
            border-bottom: 1px solid #ffc8bd;

        }
        .hide{
            display: none;
        }
    </style>
</head>
<body>
<div class="left">
    <div class="menu">
        <div class="item">
            <div class="title">菜单1</div>
            <div class="items">
                <div class="item">111</div>
                <div class="item">222</div>
                <div class="item">333</div>
            </div>
        </div>
        <div class="item">
            <div class="title">菜单2</div>
            <div class="items hide">
                <div class="item">111</div>
                <div class="item">222</div>
                <div class="item">333</div>
            </div>
        </div>
        <div class="item">
            <div class="title">菜单3</div>
            <div class="items hide">
                <div class="item">111</div>
                <div class="item">222</div>
                <div class="item">333</div>
            </div>
        </div>
    </div>
</div>
<div class="right"></div>
<script>
    $(".title").click(function () {//jQuery绑定的事件
        //隐藏所有的class里面的.items的标签
        // $(".items").addClass("hide");  //批量操作
        // $(this).next().removeClass("hide");
        // jQuery链式操作
        $(this).next().removeClass('hide').parent().siblings().find('.items').addClass('hide')


    })
</script>
</body>
</html>
```





## 操作标签

### 样式操作

样式类

```
addClass();// 添加指定的CSS类名。
removeClass();// 移除指定的CSS类名。
hasClass();// 判断样式存不存在
toggleClass();// 切换CSS类名，如果有就移除，如果没有就添加。

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016213108823-972101176.png)

CSS

```
css("color","red")//DOM操作：tag.style.color="red"

```



示例：

```
$("p").css("color", "red"); //将所有p标签的字体设置为红色

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016213305460-1072045852.png)

### 位置操作

```
offset()// 获取匹配元素在当前窗口的相对偏移或设置元素位置
position()// 获取匹配元素相对父元素的偏移
scrollTop()// 获取匹配元素相对滚动条顶部的偏移
scrollLeft()// 获取匹配元素相对滚动条左侧的偏移

```

`.offset()`方法允许我们检索一个元素相对于文档（document）的当前位置。

和 `.position()`的差别在于： `.position()`是相对于相对于父级元素的位移。

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016213714347-772074437.png)

### 位置相关示例之返回顶部示例：



```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>位置相关示例之返回顶部</title>
  <style>
    .c1 {
      width: 100px;
      height: 200px;
      background-color: red;
    }

    .c2 {
      height: 50px;
      width: 50px;

      position: fixed;
      bottom: 15px;
      right: 15px;
      background-color: #2b669a;
    }
    .hide {
      display: none;
    }
    .c3 {
      height: 10000px;
      width: 800px;
      border: 1px solid purple;
      background-color: purple;
    }
  </style>
</head>
<body>
<button id="b1" class="btn btn-default">点我</button>
<div class="c1"></div>
<div class="c3">1</div>
<button id="b2" class="btn btn-default c2 hide">返回顶部</button>
<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
<script>
  $("#b1").on("click", function () {
    $(".c1").offset({left: 200, top:200});
  });
  $(".c1").on("click", function () {
    $(".c1").offset({left: 8, top:33});
  });


  $(window).scroll(function () {
    if ($(window).scrollTop() > 50) {
      $("#b2").removeClass("hide");
    }else {
      $("#b2").addClass("hide");
    }
  });
  $("#b2").on("click", function () {
    $(window).scrollTop(0);
  })
</script>
</body>
</html>
```



### 尺寸：



```
height()
width()
innerHeight()
innerWidth()
outerHeight()
outerWidth()

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016215347826-377091230.png)

### 文本操作

HTML代码：

```
html()// 取得第一个匹配元素的html内容
html(val)// 设置所有匹配元素的html内容

```

文本值：

```
text()// 取得所有匹配元素的内容
text(val)// 设置所有匹配元素的内容

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016215806746-450847390.png)

值：

```
val()// 取得第一个匹配元素的当前值
val(val)// 设置所有匹配元素的值
val([val1, val2])// 设置多选的checkbox、多选select的值

```



例如：



```
<input type="checkbox" value="basketball" name="hobby">篮球
<input type="checkbox" value="football" name="hobby">足球

select multiple id="s1"
    option value="1"1/option
    option value="2"2/option
    option value="3"3/option
/select

```



设置值：

```
$("[name='hobby']").val(['basketball', 'football']);
$("#s1").val(["1", "2"])

```

示例：

获取被选中的checkbox或radio的值：

```
<label for="c1">女</label>
<input name="gender" id="c1" type="radio" value="0">
<label for="c2">男</label>
<input name="gender" id="c2" type="radio" value="1">

```

可以使用：

```
$("input[name='gender']:checked").val()

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016220107207-283202426.png)

### 文本操作之登录验证示例

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>文本操作之登录验证</title>
  <style>
    .error {
      color: red;
    }
  </style>
</head>
<body>

<form action="">
    <div>
        <label for="input-name">用户名</label>
        <input type="text" id="input-name" name="name">
        <span class="error"></span>
    </div>
    <div>
        <label for="input-password">密码</label>
        <input type="text" id="input-password" name="password">
        <span class="error"></span>
    </div>
    <div>
        <input type="button" id="btn" value="提交">
    </div>
</form>
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
<script>
    $("#btn").click(function () {
    var username = $("#input-name").val();
    var password = $("#input-password").val();

    if (username.length === 0) {
      $("#input-name").siblings(".error").text("用户名不能为空");
    }
    if (password.length === 0) {
      $("#input-password").siblings(".error").text("密码不能为空");
    }

  })
</script>
</body>
</html>

```

### 阻止事件示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>
        .hid{
            display: none;
        }
    </style>
</head>
<body>
<form>
    <input type="text" value="" id ="id"><span class="hid">不能为空</span>
    <br>
    <input type="submit" value="拜拜！" id="sub">
</form>
<script>
    $("#sub").on("click",function (e) {
           var data= $("#id").val();
           if(!data.trim()){
               $(".hid").removeClass("hid");
               //第一种阻止事件的方法
               // return false
                //第二种阻止事件的方法
               e.preventDefault()
           }
    })


</script>



</body>
</html>
```

### 属性操作

用于ID等或自定义属性：

```
attr(attrName)// 返回第一个匹配元素的属性值
attr(attrName, attrValue)// 为所有匹配元素设置一个属性值
attr({k1: v1, k2:v2})// 为所有匹配元素设置多个属性值
removeAttr()// 从每一个匹配的元素中删除一个属性

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016223025133-1099728983.png)

用于checkbox和radio

```
prop() // 获取属性
removeProp() // 移除属性

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191016223301726-334062051.png)

**注意：**

在1.x及2.x版本的jQuery中使用attr对checkbox进行赋值操作时会出bug，在3.x版本的jQuery中则没有这个问题。为了兼容性，我们在处理checkbox和radio的时候尽量使用特定的prop()，不要使用attr("checked", "checked")。



```
<input type="checkbox" value="1">
<input type="radio" value="2">
<script>
  $(":checkbox[value='1']").prop("checked", true);
  $(":radio[value='2']").prop("checked", true);
</script>

```



**prop和attr的区别：**

attr全称attribute(属性) 

prop全称property(属性)

虽然都是属性，但他们所指的属性并不相同，attr所指的属性是HTML标签属性，而prop所指的是DOM对象属性，可以认为attr是显式的，而prop是隐式的。

举个例子：

```
<input type="checkbox" id="i1" value="1">

```

针对上面的代码，

```
$("#i1").attr("checked")  // undefined
$("#i1").prop("checked")  // false

```

可以看到attr获取一个标签内没有的东西会得到undefined，而prop获取的是这个DOM对象的属性，因此checked为false。

如果换成下面的代码：

```
<input type="checkbox" checked id="i1" value="1">

```

再执行：

```
$("#i1").attr("checked")   // checked
$("#i1").prop("checked")  // true

```

这已经可以证明attr的局限性，它的作用范围只限于HTML标签内的属性，而prop获取的是这个DOM对象的属性，选中返回true，没选中返回false。

接下来再看一下针对自定义属性，attr和prop又有什么区别：

```
<input type="checkbox" checked id="i1" value="1" me="自定义属性">

```

执行以下代码：

```
$("#i1").attr("me")   // "自定义属性"
$("#i1").prop("me")  // undefined

```

可以看到prop不支持获取标签的自定义属性。

**总结一下：**

1. 对于标签上有的能看到的属性和自定义属性都用attr
2. 对于返回布尔值的比如checkbox、radio和option的是否被选中都用prop。



### 文档处理

添加到指定元素**内部**的后面

```
$(A).append(B)// 把B追加到A(在A内部添加B)
$(A).appendTo(B)// 把A追加到B(在B的内部添加A)
```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191017082903764-1787132591.png)

添加到指定元素**内部**的前面

```
$(A).prepend(B)// 把B前置到A(在A内部添加B并且置顶)
$(A).prependTo(B)// 把A前置到B(在B的内部添加A，并且置顶)

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191017083211956-1325459530.png)

添加到指定元素**外部**的后面

```
$(A).after(B)// 把B放到A的后面
$(A).insertAfter(B)// 把A放到B的后面

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191017083537867-713097498.png)

添加到指定元素**外部**的前面

```
$(A).before(B)// 把B放到A的前面
$(A).insertBefore(B)// 把A放到B的前面

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191017152010406-175528167.png)

移除和清空元素

```
remove()// 从DOM中删除所有匹配的元素。
empty()// 删除匹配的元素集合中所有的子节点。

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191017152131934-1936237636.png)

替换

```
replaceWith()#A被B替换
A.replaceAll(B)#A把B顶替

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191017153139967-203772027.png)

克隆

```
clone()// 参数

```

![](https://img2018.cnblogs.com/blog/1739658/201910/1739658-20191017153302074-926268375.png)

### 克隆示例：



```html
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
<button>来啊，来啊</button>
<script>
    $("button").click(function () {
        $(this).clone(true).insertAfter(this)
    })


</script>
</body>
</html>
```



## 事件

### 常用事件



```
click(function(){...})
hover(function(){...})
blur(function(){...})
focus(function(){...})
change(function(){...})
keyup(function(){...})

```

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [bind()](https://www.runoob.com/jquery/event-bind.html)      | 向元素添加事件处理程序                                       |
| [blur()](https://www.runoob.com/jquery/event-blur.html)      | 添加/触发失去焦点事件                                        |
| [hover()](https://www.runoob.com/jquery/event-hover.html)    | 添加两个事件处理程序到 hover 事件                            |
| [change()](https://www.runoob.com/jquery/event-change.html)  | 添加/触发 change 事件                                        |
| [click()](https://www.runoob.com/jquery/event-click.html)    | 添加/触发 click 事件                                         |
| [dblclick()](https://www.runoob.com/jquery/event-dblclick.html) | 添加/触发 double click 事件                                  |
| [focus()](https://www.runoob.com/jquery/event-focus.html)    | 添加/触发 focus 事件                                         |
| [keyup()](https://www.runoob.com/jquery/event-keyup.html)    | 添加/触发 keyup 事件                                         |
| [focusin()](https://www.runoob.com/jquery/event-focusin.html) | 添加事件处理程序到 focusin 事件                              |
| [focusout()](https://www.runoob.com/jquery/event-focusout.html) | 添加事件处理程序到 focusout 事件                             |
| [keydown()](https://www.runoob.com/jquery/event-keydown.html) | 添加/触发 keydown 事件                                       |
| [keypress()](https://www.runoob.com/jquery/event-keypress.html) | 添加/触发 keypress 事件                                      |
| [delegate()](https://www.runoob.com/jquery/event-delegate.html) | 向匹配元素的当前或未来的子元素添加处理程序                   |
| [live()](https://www.runoob.com/jquery/event-live.html)      | 在版本 1.9 中被移除。添加一个或多个事件处理程序到当前或未来的被选元素 |
| [load()](https://www.runoob.com/jquery/event-load.html)      | 在版本 1.8 中被废弃。添加一个事件处理程序到 load 事件        |
| [mousedown()](https://www.runoob.com/jquery/event-mousedown.html) | 添加/触发 mousedown 事件                                     |
| [mouseenter()](https://www.runoob.com/jquery/event-mouseenter.html) | 添加/触发 mouseenter 事件                                    |
| [mouseleave()](https://www.runoob.com/jquery/event-mouseleave.html) | 添加/触发 mouseleave 事件                                    |
| [mousemove()](https://www.runoob.com/jquery/event-mousemove.html) | 添加/触发 mousemove 事件                                     |
| [mouseout()](https://www.runoob.com/jquery/event-mouseout.html) | 添加/触发 mouseout 事件                                      |
| [mouseover()](https://www.runoob.com/jquery/event-mouseover.html) | 添加/触发 mouseover 事件                                     |
| [mouseup()](https://www.runoob.com/jquery/event-mouseup.html) | 添加/触发 mouseup 事件                                       |
| [off()](https://www.runoob.com/jquery/event-off.html)        | 移除通过 on() 方法添加的事件处理程序                         |
| [on()](https://www.runoob.com/jquery/event-on.html)          | 向元素添加事件处理程序                                       |
| [one()](https://www.runoob.com/jquery/event-one.html)        | 向被选元素添加一个或多个事件处理程序。该处理程序只能被每个元素触发一次 |
| [$.proxy()](https://www.runoob.com/jquery/event-proxy.html)  | 接受一个已有的函数，并返回一个带特定上下文的新的函数         |
| [ready()](https://www.runoob.com/jquery/event-ready.html)    | 规定当 DOM 完全加载时要执行的函数                            |
| [resize()](https://www.runoob.com/jquery/event-resize.html)  | 添加/触发 resize 事件                                        |
| [scroll()](https://www.runoob.com/jquery/event-scroll.html)  | 添加/触发 scroll 事件                                        |
| [select()](https://www.runoob.com/jquery/event-select.html)  | 添加/触发 select 事件                                        |
| [submit()](https://www.runoob.com/jquery/event-submit.html)  | 添加/触发 submit 事件                                        |
| [toggle()](https://www.runoob.com/jquery/event-toggle.html)  | 在版本 1.9 中被移除。添加 click 事件之间要切换的两个或多个函数 |
| [trigger()](https://www.runoob.com/jquery/event-trigger.html) | 触发绑定到被选元素的所有事件                                 |
| [triggerHandler()](https://www.runoob.com/jquery/event-triggerhandler.html) | 触发绑定到被选元素的指定事件上的所有函数                     |
| [unbind()](https://www.runoob.com/jquery/event-unbind.html)  | 从被选元素上移除添加的事件处理程序                           |
| [undelegate()](https://www.runoob.com/jquery/event-undelegate.html) | 从现在或未来的被选元素上移除事件处理程序                     |
| [unload()](https://www.runoob.com/jquery/event-unload.html)  | 在版本 1.8 中被废弃。添加事件处理程序到 unload 事件          |
| [contextmenu()](https://www.runoob.com/jquery/event-contextmenu.html) | 添加事件处理程序到 contextmenu 事件                          |
| [$.holdReady()](https://www.runoob.com/jquery/event-holdready.html) | 用于暂停或恢复.ready() 事件的执行                            |
| [die()](https://www.runoob.com/jquery/event-die.html)        | 在版本 1.9 中被移除。移除所有通过 live() 方法添加的事件处理程序 |
| [error()](https://www.runoob.com/jquery/event-error.html)    | 在版本 1.8 中被废弃。添加/触发 error 事件                    |
| [event.currentTarget](https://www.runoob.com/jquery/jq-event-currenttarget.html) | 在事件冒泡阶段内的当前 DOM 元素                              |
| [event.data](https://www.runoob.com/jquery/event-data.html)  | 包含当前执行的处理程序被绑定时传递到事件方法的可选数据       |
| [event.delegateTarget](https://www.runoob.com/jquery/event-delegatetarget.html) | 返回当前调用的 jQuery 事件处理程序所添加的元素               |
| [event.isDefaultPrevented()](https://www.runoob.com/jquery/event-isdefaultprevented.html) | 返回指定的 event 对象上是否调用了 event.preventDefault()     |
| [event.isImmediatePropagationStopped()](https://www.runoob.com/jquery/event-isimmediatepropagationstopped.html) | 返回指定的 event 对象上是否调用了 event.stopImmediatePropagation() |
| [event.isPropagationStopped()](https://www.runoob.com/jquery/event-ispropagationstopped.html) | 返回指定的 event 对象上是否调用了 event.stopPropagation()    |
| [event.namespace](https://www.runoob.com/jquery/event-namespace.html) | 返回当事件被触发时指定的命名空间                             |
| [event.pageX](https://www.runoob.com/jquery/event-pagex.html) | 返回相对于文档左边缘的鼠标位置                               |
| [event.pageY](https://www.runoob.com/jquery/event-pagey.html) | 返回相对于文档上边缘的鼠标位置                               |
| [event.preventDefault()](https://www.runoob.com/jquery/event-preventdefault.html) | 阻止事件的默认行为                                           |
| [event.relatedTarget](https://www.runoob.com/jquery/jq-event-relatedtarget.html) | 返回当鼠标移动时哪个元素进入或退出                           |
| [event.result](https://www.runoob.com/jquery/event-result.html) | 包含由被指定事件触发的事件处理程序返回的最后一个值           |
| [event.stopImmediatePropagation()](https://www.runoob.com/jquery/event-stopimmediatepropagation.html) | 阻止其他事件处理程序被调用                                   |
| [event.stopPropagation()](https://www.runoob.com/jquery/event-stoppropagation.html) | 阻止事件向上冒泡到 DOM 树，阻止任何父处理程序被事件通知      |
| [event.target](https://www.runoob.com/jquery/jq-event-target.html) | 返回哪个 DOM 元素触发事件                                    |
| [event.timeStamp](https://www.runoob.com/jquery/jq-event-timestamp.html) | 返回从 1970 年 1 月 1 日到事件被触发时的毫秒数               |
| [event.type](https://www.runoob.com/jquery/jq-event-type.html) | 返回哪种事件类型被触发                                       |
| [event.which](https://www.runoob.com/jquery/event-which.html) | 返回指定事件上哪个键盘键或鼠标按钮被按下                     |
| [event.metaKey](https://www.runoob.com/jquery/event_metakey.html) | 事件触发时 META 键是否被按下                                 |


### keydown和keyup事件组合示例：

```html
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
<table border="1">
    <thead>
    <tr>
        <th>#</th>
        <th>姓名</th>
        <th>操作</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td><input type="checkbox"></td>
        <td>Egon</td>
        <td>
            <select name="">
                <option value="1">上线</option>
                <option value="3">下线</option>
                <option value="4">停职</option>
            </select>
        </td>
    </tr>
    <tr>
        <td><input type="checkbox"></td>
        <td>Egon</td>
        <td>
            <select name="">
                <option value="1">上线</option>
                <option value="3">下线</option>
                <option value="4">停职</option>
            </select>
        </td>
    </tr>
    <tr>
        <td><input type="checkbox"></td>
        <td>Egon</td>
        <td>
            <select name="">
                <option value="1">上线</option>
                <option value="3">下线</option>
                <option value="4">停职</option>
            </select>
        </td>
    </tr>
    <tr>
        <td><input type="checkbox"></td>
        <td>Egon</td>
        <td>
            <select name="">
                <option value="1">上线</option>
                <option value="3">下线</option>
                <option value="4">停职</option>
            </select>
        </td>
    </tr>

    </tbody>
</table>
<input type="button" id="b1" value="全选">
<input type="button" id="b2" value="取消">
<input type="button" id="b3" value="反选">
<script>
    var flag = false;
    //shift键按下的时候
    (window).onkeydown(function (event) {
        console.log(event.keyCode);
        if (event.keyCode == 16) {
            flag = true;
        }
    });
    //shift按键被抬起的时候
    (window).onkeyup(function (event) {
        console.log(event.keyCode);
        if (event.keyCode == 16) {
            flag = false;
        }
    });
    //select标签的值发生变化的时候
    $("select").change(function (event) {
        // 如果shift按键被按下，就进入批量编辑模式
        // shift按键对应的code是16
        // 判断当前select这一行是否被选中
        console.log($(this).parent().siblings().first().find(":checkbox"));
        var isChecked = $(this).parent().siblings().first().find(":checkbox").prop("checked");
        console.log(isChecked);
        if (flag && isChecked) {
            // 进入批量编辑模式
            // 1. 取到当前select选中的值
            var value = $(this).val();
            // 2. 给其他被选中行的select设置成和我一样的值
            // 2.1 找到那些被选中行的select
            var $select = $("input:checked").parent().parent().find("select");
            // 2.2 给选中的select赋值
            $select.val(value);
        }
    });
</script>
</body>
</html>
```

### hover事件示例：

```html
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
<div style="width: 200px;height: 200px;background-color: red">来玩啊</div>

<script>
    $("div").hover(
        function () {
            alert("很开心为你服务");
        },
        function () {
            alert("大爷，慢走，下次再来")
        }
    )


</script>

</body>
</html>
```

### 实时监听input输入值变化示例：

```html
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
<p>当你在以上输入框中输入内容时，div 中会陷入输入键的数字码。</p>
请输入：<input type="text">

<div></div>
<span></span>
<script>
    $("input").on("input",function () {
        console.log($(this).val())

    });
    $("input").keydown(function (event) {
            $("div").html("按键key:"+event.which);
            $("span").html("按键key:"+event.keyCode)
    });


</script>

</body>
</html>
```

### 事件绑定on

1. `.on( events [, selector ],function(){})`

- events： 事件
- selector: 选择器（可选的）
- function: 事件处理函数

### 移除事件off

1. `.off( events [, selector ][,function(){}])`

`off()` 方法移除用 `.on()`绑定的事件处理程序。

- events： 事件
- selector: 选择器（可选的）
- function: 事件处理函数

### 阻止后续事件执行return示例

1. `return false; // 常见阻止表单提交等`
2. e.preventDefault();



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>阻止后续事件</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>
        .hid{
            display: none;
        }
    </style>
</head>
<body>
<form>
    <input type="text" value="" id ="id"><span class="hid">不能为空</span>
    <br>
    <input type="submit" value="拜拜！" id="sub">
</form>
<script>
    $("#sub").on("click",function (e) {
           var data= $("#id").val();
           if(!data.trim()){
               $(".hid").removeClass("hid");
               //第一种阻止事件的方法
               // return false
                //第二种阻止事件的方法
               e.preventDefault()
           }
    })


</script>



</body>
</html>
```



 

注意：

像click、keydown等DOM中定义的事件，我们都可以使用`.on()`方法来绑定事件，但是`hover`这种jQuery中定义的事件就不能用`.on()`方法来绑定了。

想使用事件委托的方式绑定hover事件处理函数，可以参照如下代码分两步绑定事件：



```
$('ul').on('mouseenter', 'li', function() {//绑定鼠标进入事件
    $(this).addClass('hover');
});
$('ul').on('mouseleave', 'li', function() {//绑定鼠标划出事件
    $(this).removeClass('hover');
});

```



### 阻止事件冒泡示例



```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>
        #a{
            width: 500px;
            height: 500px;
            background-color: red;
        }
         #b{
            width: 400px;
            height: 400px;
            background-color: green;
        }
        #c{
            width: 200px;
            height: 200px;
            background-color: black;
        }

    </style>
    <script>
        // $(document).ready(function(){
        //
        //     $("#a").on("click",function () {
        //         alert("第一")
        //     });
        //     $("#b").on("click",function (e) {
        //         alert("第er")
        //            e.stopPropagation()
        //     });
        //     $("#c").on("click",function (e) {
        //         alert("第san");
        //
        //     });
        //
        // })
         $(function(){

            $("#a").on("click",function () {
                alert("第一")
            });
            $("#b").on("click",function (e) {
                alert("第er")
                   e.stopPropagation()
            });
            $("#c").on("click",function (e) {
                alert("第san");

            });

        })

</script>
</head>
<body>
<div id="a">
    <div id="b">
        <div id="c"></div>
    </div>
</div>


</body>
</html>
```



### 页面载入

当DOM载入就绪可以查询及操纵时绑定一个要执行的函数。这是事件模块中最重要的一个函数，因为它可以极大地提高web应用程序的响应速度。

两种写法：

```
$(document).ready(function(){
// 在这里写你的JS代码...
})

```

简写：

```
$(function(){
// 你在这里写你的代码
})

```

### 与window.onload的区别

- window.onload()函数有覆盖现象，必须等待着图片资源加载完成之后才能调用
- jQuery的这个入口函数没有函数覆盖现象，文档加载完成之后就可以调用（建议使用此函数）

### 事件委托

事件委托是通过事件冒泡的原理，利用父标签去捕获子标签的事件。

示例：

表格中每一行的编辑和删除按钮都能触发相应的事件。

```
$("table").on("click", ".delete", function () {
  // 删除按钮绑定的事件
})

```

## 动画效果

```
// 基本
show([s,[e],[fn]])
hide([s,[e],[fn]])
toggle([s],[e],[fn])
// 滑动
slideDown([s],[e],[fn])
slideUp([s,[e],[fn]])
slideToggle([s],[e],[fn])
// 淡入淡出
fadeIn([s],[e],[fn])
fadeOut([s],[e],[fn])
fadeTo([[s],o,[e],[fn]])
fadeToggle([s,[e],[fn]])
// 自定义（了解即可）
animate(p,[s],[e],[fn])

```



### 自定义动画点赞示例：



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--jquery-->
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>
        div{
            position: relative;
            display: inline-block;
        }
        div>i{
            display: inline-block;
            color: red;
            position: absolute;
            right: -16px;
            top: -5px;
            opacity: 1;
        }
    </style>
</head>
<body>
<div id="d1">点赞</div>
<script>
    $("#d1").on("click",function () {
        var newI=document.createElement("i");
        newI.innerText="+1";
        $(this).append(newI);
        $(this).children("i").animate({opacity:0},1000)
    })
</script>
</body>
</html>
```

## 补充

### each

**jQuery.each(collection, callback(indexInArray, valueOfElement))：**

描述：一个通用的迭代函数，它可以用来无缝迭代对象和数组。数组和类似数组的对象通过一个长度属性（如一个函数的参数对象）来迭代数字索引，从0到length - 1。其他对象通过其属性名进行迭代。

```
li =[10,20,30,40]
$.each(li,function(i, v){
  console.log(i, v);//index是索引，ele是每次循环的具体元素。
})

```

输出：

```
010
120
230
340

```

**.each(function(index, Element))：**

描述：遍历一个jQuery对象，为每个匹配元素执行一个函数。

`.each()` 方法用来迭代jQuery对象中的每一个DOM元素。每次回调函数执行时，会传递当前循环次数作为参数(从0开始计数)。由于回调函数是在当前DOM元素为上下文的语境中触发的，所以关键字 `this` 总是指向这个元素。

```
// 为每一个li标签添加foo
$("li").each(function(){
  $(this).addClass("c1");
});

```

注意: jQuery的方法返回一个jQuery对象，遍历jQuery集合中的元素 - 被称为隐式*迭代*的过程。当这种情况发生时，它通常不需要显式地循环的 `.each()`方法：

也就是说，上面的例子没有必要使用each()方法，直接像下面这样写就可以了：

```
$("li").addClass("c1");  // 对所有标签做统一操作

```

**注意：**

在遍历过程中可以使用 `return false`提前结束each循环。

**终止each循环**

```
return false；

```

伏笔...

### .data()

在匹配的元素集合中的所有元素上存储任意相关数据或返回匹配的元素集合中的第一个元素的给定名称的数据存储的值。

**.data(key, value):**

描述：在匹配的元素上存储任意相关数据。

```
$("div").data("k",100);//给所有div标签都保存一个名为k，值为100

```

**.data(key):**

描述: 返回匹配的元素集合中的第一个元素的给定名称的数据存储的值—通过 `.data(name, value)`或 `HTML5 data-*`属性设置。

```
$("div").data("k");//返回第一个div标签中保存的"k"的值

```

.removeData(key):

描述：移除存放在元素上的数据，不加key参数表示移除所有保存的数据。

```
$("div").removeData("k");  //移除元素上存放k对应的数据

```

示例：

模态框编辑的数据回填表格

### 插件(了解即可)

jQuery.extend(object)

jQuery的命名空间下添加新的功能。多用于插件开发者向 jQuery 中添加新函数时使用。

示例：



```
<script>
jQuery.extend({
  min:function(a, b){return a < b ? a : b;},
  max:function(a, b){return a > b ? a : b;}
});
jQuery.min(2,3);// => 2
jQuery.max(4,5);// => 5
</script>

```



jQuery.fn.extend(object)

一个对象的内容合并到jQuery的原型，以提供新的jQuery实例方法。



```
<script>
  jQuery.fn.extend({
    check:function(){
      return this.each(function(){this.checked =true;});
    },
    uncheck:function(){
      return this.each(function(){this.checked =false;});
    }
  });
// jQuery对象可以使用新添加的check()方法了。
$("input[type='checkbox']").check();
</script>

```



单独写在文件中的扩展：



```
(function(jq){
  jq.extend({
    funcName:function(){
    ...
    },
  });
})(jQuery);

```



例子：

自定义的jQuery登录验证插件



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--jquery-->
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/css/bootstrap.min.css">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.3.1/js/bootstrap.min.js"></script>
    <style>
        .login-form{
            margin: 100px auto 0;
            max-width: 330px;
        }
        .login-form>div{
            margin: 15px 0;
        }
        .error{
            color: red;
        }
    </style>
</head>
<body>
<div>
    <form action="" class="login-form" novalidate>
        <div>
            <label for="username">姓名</label>
            <input type="text" id="username" name="name" required autocomplete="off">
            <span class="error"></span>
        </div>
        <div>
            <label for="password">密码</label>
            <input type="text" id="password" name="password" required autocomplete="off">
            <span class="error"></span>
        </div>
        <div>
            <label for="mobile">手机</label>
            <input type="text" id="mobile" name="mobile" required autocomplete="off">
            <span class="error"></span>
        </div>
        <div>
            <label for="where">来自</label>
            <input type="text" id="where" name="where" autocomplete="off">
            <span class="error"></span>
        </div>
        <div>
            <input type="submit" value="登陆">
        </div>

    </form>
</div>
<script>
    $.validate();
</script>
</body>
</html>
```





```js
"use strict";
(function ($) {
    function check() {
        // 定义一个标志位，表示验证通过还是验证不通过
        var flag = true;
        var errMsg;
        // 校验规则
        $("form input[type!=':submit']").each(function () {
            var labelName = $(this).prev().text();
            var inputName = $(this).attr("name");
            var inputValue = $(this).val();
            if ($(this).attr("required")) {
                // 如果是必填项
                if (inputValue.length === 0) {
                    // 值为空
                    errMsg = labelName + "不能为空";
                    $(this).next().text(errMsg);
                    flag = false;
                    return false;
                }
                // 如果是密码类型，我们就要判断密码的长度是否大于6位
                if (inputName === "password") {
                    // 除了上面判断为不为空还要判断密码长度是否大于6位
                    if (inputValue.length < 6) {
                        errMsg = labelName + "必须大于6位";
                        $(this).next().text(errMsg);
                        flag = false;
                        return false;
                    }
                }
                // 如果是手机类型，我们需要判断手机的格式是否正确
                if (inputName === "mobile") {
                    // 使用正则表达式校验inputValue是否为正确的手机号码
                    if (!/^1[345678]\d{9}$/.test(inputValue)) {
                        // 不是有效的手机号码格式
                        errMsg = labelName + "格式不正确";
                        $(this).next().text(errMsg);
                        flag = false;
                        return false;
                    }
                }
            }
        });
        return flag;
    }

    function clearError(arg) {
        // 清空之前的错误提示
        (arg).next().text("");
    };
    $.extend({
        validate: function () {
            ("form :submit").on("click", function () {
                return check();
            });
            ("form :input[type!='submit']").on("focus", function () {
                clearError(this);
            });
        }
    });
})(jQuery);

```



JS文件

 传参版插件：



```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>登录校验示例</title>
  <style>
    .login-form {
      margin: 100px auto 0;
      max-width: 330px;
    }
    .login-form > div {
      margin: 15px 0;
    }
    .error {
      color: red;
    }
  </style>
</head>
<body>



div
  form action="" class="login-form" novalidate

    <div>
      <label for="username">姓名</label>
      <input id="username" type="text" name="name" required autocomplete="off">
      <span class="error"></span>
    </div>
    <div>
      <label for="passwd">密码</label>
      <input id="passwd" type="password" name="password" required autocomplete="off">
      <span class="error"></span>
    </div>
    <div>
      <label for="mobile">手机</label>
      <input id="mobile" type="text" name="mobile" required autocomplete="off">
      <span class="error"></span>
    </div>
    <div>
      <label for="where">来自</label>
      <input id="where" type="text" name="where" autocomplete="off">
      <span class="error"></span>
    </div>
    <div>
      <input type="submit" value="登录">
    </div>
    

  /form
/div

script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"/script
script src="validate3.js"/script
script
  $.validate({"name":{"required": true}, "password": {"required": true, "minLength": 8}, "mobile": {"required": true}});
/script
/body
/html

```



HTML文件



```
"use strict";
(function ($) {
  function check(arg) {
    // 定义一个标志位，表示验证通过还是验证不通过
    var flag = true;
    var errMsg;
    // 校验规则
    $("form input[type!=':submit']").each(function () {
      var labelName = $(this).prev().text();
      var inputName = $(this).attr("name");
      var inputValue = $(this).val();
      if (arg[inputName].required) {
        // 如果是必填项
        if (inputValue.length === 0) {
          // 值为空
          errMsg = labelName + "不能为空";
          $(this).next().text(errMsg);
          flag = false;
          return false;
        }
        // 如果是密码类型，我们就要判断密码的长度是否大于6位
        if (inputName === "password") {
          // 除了上面判断为不为空还要判断密码长度是否大于6位
          if (inputValue.length < arg[inputName].minLength) {
            errMsg = labelName + "必须大于"+arg[inputName].minLength+"位";
            $(this).next().text(errMsg);
            flag = false;
            return false;
          }
        }
        // 如果是手机类型，我们需要判断手机的格式是否正确
        if (inputName === "mobile") {
          // 使用正则表达式校验inputValue是否为正确的手机号码
          if (!/^1[345678]\d{9}$/.test(inputValue)) {
            // 不是有效的手机号码格式
            errMsg = labelName + "格式不正确";
            $(this).next().text(errMsg);
            flag = false;
            return false;
          }
        }
      }
    });
    return flag;
  }

  function clearError(arg) {
    // 清空之前的错误提示
    (arg).next().text("");
  }
  // 上面都是我定义的工具函数
  .extend({
    validate: function (arg) {
      ("form :submit").on("click", function () {
      return check(arg);
    });
    ("form :input[type!='submit']").on("focus", function () {
      clearError(

```

