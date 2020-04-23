[TOC]

# 前端css

# 1 css介绍







css（Cascading Style Sheet,层叠样式表）定义如何显示HTML元素。

当浏览器读到一个样式表的时候，它就会按照这个样式表来对文档进行格式化。



# 2 css语法

## 2.1css实例

每个css样式都是由俩个部分组成：选择器和声明。声明又包括属性和属性值。每个声明之后用分号结束。



![](https://images2017.cnblogs.com/blog/867021/201712/867021-20171215115756808-909989248.png)

## 2.2css注释

```html
/*注释内容*/
```

# 3 css的几种引入方式

## 3.1行内样式(内联样式表)

行里样式就是在标记的style属性中设定css样式，不推荐大规模使用。

```html
<p style="color:red">
    hello world
</p>
```

## 3.2内部样式

嵌入式是将css样式集中写在&lt;head&gt;&lt;/head&gt;标签对的&lt;style&gt;&lt;style&gt;标签对中，格式如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        p{
            color: red;
        }
    </style>
</head>
<body>
<p> 窝窝头 一块钱 4 个 嘿嘿</p>
</body>
</html>
```

## 3.3外部样式

外部样式就是将css写在一个单独的文件中，然后在页面进行引入即可，推荐使用此方法。

```html
<link rel="stylesheet" type="text/css" href="my.css"/>
```



# 4 css选择器

## 4.1基本选择器

### 4.1.1元素选择器

```html
<style>
	p{color:red;}    
</style>
```

### 4.1.2  ID选择器

```html
<style>
    
    #da{color:red;}
</style>
```

### 4.1.3类选择器

```html
<style>
    .ca{color:red;}
</style>
```

注意：

样式类名不要用数字开头（有的浏览器不认识）。

标签中的class属性如果有多个，用空格分开。

### 4.1.4通用选择器 

```html
<style>
    *{	
	padding: 0;
	margin: 0;
}
</style>
```

## 4.2组合选择器

### 4.2.1后代选择器

```html
/*li内部的a标签设置字体颜色*/
<style>
    li a{color:red;}

</style>


```



### 4.2.2儿子选择器

```html
/*选择父级div下的所有的p标签*/
<style>
    div>p{font-family:"Arial Black",arial-black,cursive,"微软雅黑";}
</style>


```

### 4.2.3 毗邻选择器

```html
/*选择紧挨着div元素的p元素*/
<style>
    div+p{color:red;}
</style>


```

### 4.2.4弟弟选择器

```html
/*id后边所有的兄弟p标签*/
<style>
    #il~p{color:red;}
</style>


```

## 4.3 属性选择器

```html
<style>
    /*用于选取带有指定属性的元素。*/
	p[title] { color: red; } 
    /*用于选取带有指定属性和值的元素。*/ 		       p[title="213"] { color: green; }
 
</style>


```

不常用的属性选择器

```html
<style>
    /*找到所有title属性以hello开头的元素*/
	[title^="hello"] {color: red;}

	/*找到所有title属性以hello结尾的元素*/
	[title$="hello"] {color: yellow;}

	/*找到所有title属性中包含（字符串包含）hello的元素*/
    [title*="hello"] {color: red;}

	/*找到所有title属性(有多个值或值以空格分割)中有一个值为hello的元素：*/
	[title~="hello"] {color: green;}
</style>


```

## 4.4分组和嵌套

### 4.4.1分组

当多个元素的样式相同的时候，我们没有必要重复地为每个元素都设置样式，我们可以通过在多个选择器之间使用逗号分隔的分组选择器来统一设置元素样式。

比如：

```html
<style>
    
    div,p{color:red;}
</style>


```

### 4.4.2嵌套

多种选择器可以混合起来使用，比如在c1类内部的所有的p标签都设置字体颜色为红色。

```html
<style>
    .cl p{color:red;}
</style>


```

## 4.5伪类选择器

```html
<style>
    /*未访问链接*/
    a:link{color:red;}
    /*鼠标移动到链接上的时候*/
    a:hover{color:blue;}
    /*当鼠标点击不松手的时候*/
    a:active{color:yellow;}
    /*访问后*/
    a:visited{color:black;}
    
    /*input输入框获取焦点的时的样式*/
    input:focus{outline:none;background-color:red;}
</style>


```

## 4.6伪元素选择器

**first-letter**

常用的给标签内的首字母设置特殊样式

```html
<style>
    p:first-letter{font-size:48px;color:red;}</style>


```



**berfore**

```html
<style>
	/*在每个<p>元素之前插入内容*/
    p:before{content:"*",color:red;}
</style>


```

**after**

```html
<style>
    /*在每个<p>元素之后插入内容*/
    p:after{content:"嘿嘿"，color:red;}
</style>



```

==before和after多用于清除浮动==

## 4.7选择器的优先级

**css继承**

继承是CSS的一个主要特征，它是依赖于祖先-后代的关系的。继承是一种机制，它允许样式不仅可以应用于某个特定的元素，还可以应用于它的后代。例如一个body定义了的字体颜色值也会应用到段落的文本中。

```html
<style>
    body { color: red; }
</style>


```



此时页面上所有标签都会继承body的字体颜色。然而CSS继承性的权重是非常低的，是比普通元素的权重还要低的0。

我们只要给对应的标签设置字体颜色就可覆盖掉它继承的样式。

```html
<style>
    p { color: green; }
</style>


```



此外，继承是CSS重要的一部分，我们甚至不用去考虑它为什么能够这样，但CSS继承也是有限制的。有一些属性不能被继承，如：border, margin, padding, background等。

**选择器的优先级**

我们上面学了很多的选择器，也就是说在一个HTML页面中有很多种方式找到一个元素并且为其设置样式，那浏览器根据什么来决定应该应用哪个样式呢？

其实是按照不同选择器的权重来决定的，具体的选择器权重计算方式如下图：

[![img](https://images2018.cnblogs.com/blog/867021/201803/867021-20180305155201408-1680872107.png)

除此之外还可以通过添加 !important方式来强制让样式生效，但并不推荐使用。因为如果过多的使用!important会使样式文件混乱不易维护。

万不得已可以使用!important









# 5  css属性相关

## 5.1宽和高

width属性可以为元素设置宽度。

height属性可以为元素设置高度。

块级标签才能设置宽度，内联标签的宽度由内容来决定。

## 5.2字体属性

### 文字字体

font-family可以把多个字体名称作为一个“回退”系统来保存。如果浏览器不支持第一个字体，则会尝试下一个。浏览器会使用它可识别的第一个值。

```html
<style>
    body{ font-family: "Microsoft Yahei", "微软雅黑", "Arial", sans-serif }
</style>


```

### 字体大小

```html
<style>p{font-size:14px;}</style>


```

==如果设置陈inherit表示继承父元素的字体大小数值。==

### 字体的粗细（字重）

==font-weight==用来设置字体的粗细。

| 值      | 描述                                           |
| ------- | ---------------------------------------------- |
| normal  | 标准的粗细                                     |
| bold    | 粗体                                           |
| bolder  | 更粗                                           |
| lighter | 更细                                           |
| 100-900 | 设置具体的粗细，400等同于normal，700等同于bold |
| inherit | 继承父元素字体的粗细，是默认值                 |

### 文本颜色



颜色属性被用来设置文字的颜色。

颜色是通过CSS最经常的指定：

- 十六进制值 - 如: **＃**FF0000
- 一个RGB值 - 如: RGB(255,0,0)
- 颜色的名称 - 如:  red

还有rgba(255,0,0,0.3)，第四个值为alpha, 指定了色彩的透明度/不透明度，它的范围为0.0到1.0之间。

## 5.3文字属性



### 文字对齐

text-align 属性规定元素中的文本的水平对齐方式。

|   值    |      描述       |
| :-----: | :-------------: |
|  left   | 左边对齐 默认值 |
|  right  |     右对齐      |
| center  |    居中对齐     |
| justify |    两端对齐     |

### 文字装饰

text-decoration 属性用来给文字添加特殊效果。

|      值      |                 描述                  |
| :----------: | :-----------------------------------: |
|     none     |        默认。定义标准的文本。         |
|  underline   |         定义文本下的一条线。          |
|   overline   |         定义文本上的一条线。          |
| line-through |       定义穿过文本下的一条线。        |
|   inherit    | 继承父元素的text-decoration属性的值。 |

常用的为去掉a标签默认的自划线：

```html
a { text-decoration: none; }


```



 

### 首行缩进

```html
将段落的第一行缩进 32像素：

p { text-indent: 32px; }

 去除li标签的样式

list-style: none;


```



### 文字之间的距离

~~~html
将文字的间距调整为5像素：

p {

```
letter-spacing: 5px;
```

}

 


~~~



## 5.4 背景属性





```html
<style>
    /*背景颜色*/
	background-color: red;
    /*背景图片*/ 
    background-image: url('1.jpg');
/*
背景重复
repeat(默认):背景图片平铺排满整个网页
repeat-x：背景图片只在水平方向上平铺
repeat-y：背景图片只在垂直方向上平铺
no-repeat：背景图片不平铺
*/ background-repeat: no-repeat; 
    /*背景位置*/ 
    background-position: left top;
/*background-position: 200px 200px;*/
    
    /*支持合写*/
    background:#336699 url('1.png') no-repeat left top;
</style>


```





 

使用背景图片的一个常见案例就是很多网站会把很多小图标放在一张图片上，然后根据位置去显示图片。减少频繁的图片请求。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>滚动背景图示例</title>
    <style>
        * {
            margin: 0;
        }
        .box {
            width: 100%;
            height: 500px;
            background: url("http://gss0.baidu.com/94o3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D450%2C600/sign=e9952f4a6f09c93d07a706f3aa0dd4ea/4a36acaf2edda3cc5c5bdd6409e93901213f9232.jpg")  center center;
            background-attachment: fixed;
        }
        .d1 {
            height: 500px;
            background-color: tomato;
        }
        .d2 {
            height: 500px;
            background-color: steelblue;
        }
        .d3 {
            height: 500px;
            background-color: mediumorchid;
        }
    </style>
</head>
<body>
    <div class="d1"></div
</html>


```



## 5.5边框



边框属性 

- border-width
- border-style
- border-color

```html
#i1 { border-width: 2px; border-style: solid; border-color: red; }

 

通常使用简写方式：

#i1 { border: 2px solid red; }


```



 

边框样式

|   值   |      描述      |
| :----: | :------------: |
|  none  |    无边框。    |
| dotted | 点状虚线边框。 |
| dashed | 矩形虚线边框。 |
| solid  |   实线边框。   |

 

除了可以统一设置边框外还可以单独为某一个边框设置样式，如下所示：

```html
#i1 { border-top-style:dotted; border-top-color: red; border-right-style:solid; border-bottom-style:dotted; border-left-style:none; }


```



 

## 5.6border-radius



用这个属性能实现圆角边框的效果。

将border-radius设置为长或高的一半即可得到一个圆形。



设置一角border-bottom-left-radius: 300px;

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .disc{
            width: 500px;
            height: 500px;
            border: 2px red solid;
            border-radius: 50%;
        }

        .disc2{
             width: 500px;
            height: 500px;
            border: 2px red solid;
            border-bottom-left-radius: 300px;
        }
    </style>
</head>
<body>

<div class="disc"></div>

<div class="disc2"></div>
</body>
</html>


```



## 5.7display属性



用于控制HTML元素的显示效果。

| 值                     | 意义                                                         |
| ---------------------- | ------------------------------------------------------------ |
| display:"none"         | HTML文档中元素存在，但是在浏览器中不显示。一般用于配合JavaScript代码使用。 |
| display:"block"        | 默认占满整个页面宽度，如果设置了指定宽度，则会用margin填充剩下的部分。 |
| display:"inline"       | 按行内元素显示，此时再设置元素的width、height、margin-top、margin-bottom和float属性都不会有什么影响。 |
| display:"inline-block" | 使元素同时具有行内元素和块级元素的特点。                     |

 

**display:"none"与visibility:hidden的区别：**

visibility:hidden: 可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间。也就是说，该元素虽然被隐藏了，但仍然会影响布局。

display:none: 可以隐藏某个元素，且隐藏的元素不会占用任何空间。也就是说，该元素不但被隐藏了，而且该元素原本占用的空间也会从页面布局中消失。

## 5.8  CSS盒子模型



- **margin**:            用于控制元素与元素之间的距离；margin的最基本用途就是控制元素周围空间的间隔，从视觉角度上达到相互隔开的目的。
- **padding**:           用于控制内容与边框之间的距离；   
- **Border**(边框):     围绕在内边距和内容外的边框。
- **Content**(内容):   盒子的内容，显示文本和图像。

看图吧：

![img](https://images2018.cnblogs.com/blog/867021/201803/867021-20180305155247808-885981996.png)

## 5.9 margin外边距



```html
.margin-test { margin-top:5px; margin-right:10px; margin-bottom:15px; margin-left:20px; }

 

推荐使用简写：

.margin-test { margin: 5px 10px 15px 20px; }

 

顺序：上右下左

常见居中：

.mycenter { margin: 0 auto; }



```



 

## 5.10padding内填充



```html
.padding-test { padding-top: 5px; padding-right: 10px; padding-bottom: 15px; padding-left: 20px; }

 

推荐使用简写：

.padding-test { padding: 5px 10px 15px 20px; }



```



 

顺序：上右下左

补充padding的常用简写方式：

- 提供一个，用于四边；
- 提供两个，第一个用于上－下，第二个用于左－右；
- 如果提供三个，第一个用于上，第二个用于左－右，第三个用于下；
- 提供四个参数值，将按上－右－下－左的顺序作用于四边；

## 5.11 float



在 CSS 中，任何元素都可以浮动。

浮动元素会生成一个块级框，而不论它本身是何种元素。

关于浮动的两个特点：

- 浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。
- 由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。

### 三种取值

left：向左浮动

right：向右浮动

none：默认值，不浮动

## 5.12clear



clear属性规定元素的哪一侧不允许其他浮动元素。

| 值      | 描述                                  |
| ------- | ------------------------------------- |
| left    | 在左侧不允许浮动元素。                |
| right   | 在右侧不允许浮动元素。                |
| both    | 在左右两侧均不允许浮动元素。          |
| none    | 默认值。允许浮动元素出现在两侧。      |
| inherit | 规定应该从父元素继承 clear 属性的值。 |

注意：clear属性只会对**自身**起作用，而不会影响其他元素。

### 清除浮动

清除浮动的副作用（父标签塌陷问题）

主要有三种方式：

- 固定高度
- 伪元素清除法
- overflow:hidden

伪元素清除法（使用较多）：

.clearfix:after { content: ""; display: block; clear: both; }

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .a, .b {
            width: 250px;
            height: 250px;
        }

        .a {
            background-color: red;
            float: left;
        }

        .b {
            background-color: gold;
            float: right;
        }

        .p {
            border: gold solid 3px;
            /*height: 300px;*/
            overflow: hidden;

        }

        /*.clear {*/
        /*    clear: both;*/
        /*}*/
        /**/
        /*.clear:after{*/
        /*    content: "";*/
        /*    display: block;*/
        /*    clear: both;*/
        /*}*/


    </style>
</head>
<body>
<div class="p clear">
    <div class="a">123</div>
    <div class="b"></div>
    <div class="clear"></div>

</div>

</body>
</html>



```



## 5.13  overflow溢出属性



| 值      | 描述                                                     |
| ------- | -------------------------------------------------------- |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
| scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit | 规定应该从父元素继承 overflow 属性的值。                 |

 

- overflow（水平和垂直均设置）
- overflow-x（设置水平方向）
- overflow-y（设置垂直方向）



## 5.14  定位（position）



### static

static 默认值，无定位，不能当作绝对定位的参照物，并且设置标签对象的left、top等值是不起作用的的。

### relative（相对定位）

相对定位是相对于该元素在文档流中的原始位置，即以自己原始位置为参照物。有趣的是，即使设定了元素的相对定位以及偏移值，元素还占有着原来的位置，即占据文档流空间。对象遵循正常文档流，但将依据top，right，bottom，left等属性在正常文档流中偏移位置。而其层叠通过z-index属性定义。

注意：position：relative的一个主要用法：方便绝对定位元素找到参照物。

### absolute（绝对定位）

定义：设置为绝对定位的元素框从文档流完全删除，并相对于最近的已定位祖先元素定位，如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块（即body元素）。元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样。元素定位后生成一个块级框，而不论原来它在正常流中生成何种类型的框。

重点：如果父级设置了position属性，例如position:relative;，那么子元素就会以父级的左上角为原始点进行定位。这样能很好的解决自适应网站的标签偏离问题，即父级为自适应的，那我子元素就设置position:absolute;父元素设置position:relative;，然后Top、Right、Bottom、Left用百分比宽度表示。

另外，对象脱离正常文档流，使用top，right，bottom，left等属性进行绝对定位。而其层叠通过z-index属性定义。

### fixed（固定）

fixed：对象脱离正常文档流，使用top，right，bottom，left等属性以窗口为参考点进行定位，当出现滚动条时，对象不会随着滚动。而其层叠通过z-index属性 定义。 注意点： 一个元素若设置了 position:absolute | fixed; 则该元素就不能设置float。这 是一个常识性的知识点，因为这是两个不同的流，一个是浮动流，另一个是“定位流”。但是 relative 却可以。因为它原本所占的空间仍然占据文档流。

在理论上，被设置为fixed的元素会被定位于浏览器窗口的一个指定坐标，不论窗口是否滚动，它都会固定在这个位置。

### 是否脱离文档流

上述例子可知：

**脱离文档流：**

　　绝对定位

　　固定定位

**不脱离文档流：**

　　相对定位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>画圆</title>
    <style>
        body {
            height: 2000px;
        }

        .relative {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            position: relative;
            background: red;
            left: 100px;
            top: 100px;
        }

        .absolute {
            width: 100px;
            height: 100px;
            border: 2px solid black;
            position: absolute;
            background: #ffb515;
            left: 50px;
            top: 50px;
        }

        .fixed {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            position: fixed;
            background: #2f0099;
            left: 800px;
            bottom: 20px;
        }
    </style>
</head>
<body>
<div class="relative">
    <div class="absolute">

    </div>
</div>


<div class="fixed">

</div>
</body>
</html>



```



## 5.15  z-index



```html
#i2 { z-index: 999; }



```



 

设置对象的层叠顺序。

1. z-index 值表示谁压着谁，数值大的压盖住数值小的，
2. 只有定位了的元素，才能有z-index,也就是说，不管相对定位，绝对定位，固定定位，都可以使用z-index，而浮动元素不能使用z-index
3. z-index值没有单位，就是一个正整数，默认的z-index值为0如果大家都没有z-index值，或者z-index值一样，那么谁写在HTML后面，谁在上面压着别人，定位了元素，永远压住没有定位的元素。
4. 从父现象：父亲怂了，儿子再牛逼也没用

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>自定义模拟框</title>
    <style>
        .top{
            width: 500px;
            height: 500px;
            background: #2f0099;
            position: fixed;
            z-index: 100;
            opacity: 0.5;
        }
        .bottom{
            width: 400px;
            height: 400px;
            background: #ff5b28;
            position: fixed;
            z-index: 101;
            opacity: 0;
        }
    </style>
</head>
<body>
<div class="bottom"></div>
<div class="top"></div>


</body>
</html>



```





## 5.16    opacity



用来定义透明效果。取值范围是0~1，0是完全透明，1是完全不透明。

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        .bottom{
            width: 400px;
            height: 400px;
            background: #ff5b28;
            position: fixed;
            z-index: 101;
            opacity: 0;
        }
    </style>
</head>
<body>
<div class="bottom"></div>


</body>
</html>



```

















# 综合

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>umblog</title>
</head>
<style>
    * {
        margin: 0;
        padding: 0;
    }

    ul li a {
        text-decoration: none;
        color: #A3A3A3;
    }

    .left {
        width: 20%;
        height: 100%;
        background-color: #4E4E4E;
        position: fixed;
    }

    .left .photo {
        width: 130px;
        height: 130px;
        border-radius: 50%;
        overflow: hidden;
        margin: 20px auto;
        border: 3px white solid;

    }

    img {
        max-width: 130px;
        min-height: 130px;
    }

    .name {
        color: #A3A3A3;
        text-align: center;
        margin: 10px;
    }

    .blog {
        margin-top: 100px;
        text-align: center;
        /*margin: 2px;*/
        margin-bottom: 100px;
    }

    a:hover {
        color: azure;
    }
    .right{
        width: 80%;
        /*height: 10000px;*/
        background-color: #FFFFFF;
        float: right;
    }
    .right .title{
        width: 500px;
        height: 200px;
        background-color: #FFFFFF;
        margin: 20px;
        box-shadow: 3px 3px 3px black;
    }
    .right .title .name{
        /*display: block;*/
        color: black;
        font-size: 40px;
        font-weight: bolder;
        padding: 20px;
    }
    .right .title .let{
        display: block;
        width: 10px;
        height: 60px;
        background-color: red;
        /*margin-right: 20px;*/
        float: left;

    }
    .right .title .date{
        color: black;
        font-weight: bolder;
        padding-left: 200px;

    }
    .want{
        margin:20px 20px 20px 50px ;
    }
    .language{
        padding: 30px;
    }
</style>
<body>
<div class="left">

    <div class="photo">
        <img src="1.jpg" alt="">
    </div>

    <div class="name">海洋的博客</div>
    <div class="name">我是最帅的那个人</div>
    <div class="blog">
        <ul>
            <li><a href="">关于我</a></li>
            <li><a href="">微博</a></li>
            <li><a href="">微信公众号</a></li>
        </ul>

    </div>
    <div class="blog">
        <ul>
            <li><a href="">#Python</a></li>
            <li><a href="">#PHP</a></li>
            <li><a href="">#go</a></li>
        </ul>

    </div>

</div>
<div class="right">
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>
    <div class="title">
        <span class="let"></span>
        <span class="name">梅花朵躲开</span>
        <span class="date">2019.10.12</span>
        <br>
        <div class="want">这个就是你想要的书</div>
        <div><hr></div>
        <span class="language">#Python</span><span>#Java</span>
    </div>

</div>
</body>
</html>
```









