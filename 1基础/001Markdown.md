[TOC]

# 一、前言

markdown语法简单，还是非常容易的，一些地方容易出错，下面一些基本标签中容易出错的地方，都讲的很清楚，希望能给大家一些帮助。

注释：[TOC       可以自动生成目录]

## 1.1、标题的写法



一级标题前一个#加空格，如：

#一级标题

# 一级标题

二级标题前俩个##加空格：

##二级标题

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

最小是六级标题哦

## 1.2、加粗

加粗的字体是在**  字体   **中，如：

```markdown
**粗了**
```

**粗了**



## 1.3、倾斜

要是想写倾斜的字体，一定记住是这样写的,*   字体*：

```markdown
*泄露了*
```

*泄露了*

## 1.4、 高亮

```markdown
==我高亮了==
```



==我高亮了==

## 1.5、 上标

```markdown
2^2^
```



2^2^

## 1.6、下标

```markdown
H~2~0
```

H~2~0

# 2、代码引用

## 2.1代码引用（>式）

```mark
>hello chen!
>>HELLO
>>>hello
```

> hello chen!

> > HELLO

> > > hello

## 2.2 代码引用（```式)

```markdown
​```python
print('hello chen')
​```
```

```python
print('hello chen')
```

## 2.3 代码引入

```markdown
`print('hello chen')
```

`print('hello chen')`

## 2.3 链接

### 2.3.1、链接显示

```mark
<http://wwwhttps://home.cnblogs.com/u/chenziqing>
```

<http://wwwhttps://home.cnblogs.com/u/chenziqing>

### 2.3.2 链接描述显示

```mark
[chen博客](http://wwwhttps://home.cnblogs.com/u/chenziqing)
```

[chen博客](http://wwwhttps://home.cnblogs.com/u/chenziqing)

## 2.4 插入图片

```markdown
![img](地址)
```

![img](C:\Users\admin\Pictures\Saved Pictures\1812714.jpg)





## 2.5 列表

### 2.5.1 有序列表

```mark
1.one
2.one
3.one
```

1. one

2. 2

3. 3

   ### 2.5.2 无序列表

   ```mark
   
   * 1
   * 2
   * 3
   ```

   * 1

   * 2

   * 3

     ## 2.6 分割线
     
     ```mark
     ---
     ```
     
     ---







## 2.7、markdown如何建设表格

在markdown中学习建设表格

格式：表格而且第二行必须得有，并且第二行的冒号代表对齐格式，分别为居中；右对齐；左对齐）：

| name  | age  | sex  |
| :---: | :--: | :--: |
| hallo |  21  |  女  |
| tony  |  20  |  男  |

形式如下：

name | age |sex

:-: |:-|-: 

hallo|21|女

tony|20|nan

运行成表格要进行查看中间是否有空行，点击查看源代码，删除多行。既可以生成表格。

## 2.8 数学公式

```markdown
$\sum_{i=1}^{10}f(i)\,\,\text{thanks}$
```

```mark
$$
\sum_{i=1}^{10}f(i)\,\,\text{thanks}
$$
```


$$
\sum_{i=1}^{10}f(i)\,\,\text{thanks}
$$

# 三总结

今天是第一次学习，关于python的内柔，也认识到简单语言markdown，上面讲述的都是基本markdown标签，不多，但是可以给大家学习。一起加油。