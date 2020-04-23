# Python函数介绍及用法

Python中map()、filter()、reduce()这三个都是应用于序列的内置函数。 

## Python map()函数

```
1.map()是 Python 内置的高阶函数，它接收一个函数 f 和一个 list，并通过把函数 f 依次作用在 list 的每个元素上，得到一个新的 list 并返回。
2.注意：map()函数不改变原有的 list，而是返回一个新的 list。
3.利用map()函数，可以把一个 list 转换为另一个 list，只需要传入转换函数。
4.由于list包含的元素可以是任何类型，因此，map() 不仅仅可以处理只包含数值的 list，事实上它可以处理包含任意类型的 list，只要传入的函数f可以处理这种数据类型。
```


格式：

```
1 map(func, seq1[, seq2,…]) 
```

  

第一个参数接受一个函数名，后面的参数接受一个或多个可迭代的序列，返回的是一个集合。  
Python函数编程中的map()函数是将func作用于seq中的每一个元素，并将所有的调用的结果作为一个list返回。如果func为None，作用同zip()。

1、当seq只有一个时，将函数func作用于这个seq的每个元素上，并得到一个新的seq。  
让我们来看一下只有一个seq的时候，map()函数是如何工作的。  
![work](http://img.blog.csdn.net/20150720225527063)  
从上图可以看出，函数func函数会作用于seq中的每个元素，得到func(seq[n])组成的列表。下面举得例子来帮助我们更好的理解这个工作过程。

​    

```
#使用lambda

>>> print map(lambda x: x % 2, range(7))

[0, 1, 0, 1, 0, 1, 0]
```

![3](http://img.blog.csdn.net/20150719231037347)

​    

```
#使用列表解析

>>> print [x % 2 for x in range(7)]

[0, 1, 0, 1, 0, 1, 0]
```

![4](http://img.blog.csdn.net/20150719231139216)  
一个seq时，可以使用filter()函数代替，那什么情况不能代替呢？

2、当seq多于一个时，map可以并行（注意是并行）地对每个seq执行如下图所示的过程：  
![2](http://img.blog.csdn.net/20150719230516191)  
从图可以看出，每个seq的同一位置的元素同时传入一个多元的func函数之后，得到一个返回值，并将这个返回值存放在一个列表中。下面我们看一个有多个seq的例子：

​    

```
>>> print map(lambda x , y : x ** y, [2,4,6],[3,2,1])

[8, 16, 6]
```



![5](http://img.blog.csdn.net/20150719231256646)  
如果上面我们不使用map函数，就只能使用for循环，依次对每个位置的元素调用该函数去执行。还可以使返回值是一个元组。如：

​    

```
>>> print map(lambda x , y : (x ** y, x + y), [2,4,6],[3,2,1])

[(8, 5), (16, 6), (6, 7)]
```

![7](http://img.blog.csdn.net/20150719231343980)  
当func函数时None时，这就同zip()函数了，并且zip()开始取代这个了，目的是将多个列表相同位置的元素归并到一个元组。如：

​    

```
>>> print map(None, [2,4,6],[3,2,1])

[(2, 3), (4, 2), (6, 1)]
```

需要注意的是：  
map无法处理seq长度不一致、对应位置操作数类型不一致的情况，这两种情况都会报类型错误。如下图：  
![8](http://img.blog.csdn.net/20150719231427487)

3、使用map()函数可以实现将其他类型的数转换成list，但是这种转换也是有类型限制的，具体什么类型限制，在以后的学习中慢慢摸索吧。这里给出几个能转换的例子：

​    

```
***将元组转换成list***
>>> map(int, (1,2,3))
[1, 2, 3]
***将字符串转换成list***
>>> map(int, '1234')
[1, 2, 3, 4]
***提取字典的key，并将结果存放在一个list中***
>>> map(int, {1:2,2:3,3:4})
[1, 2, 3]
***字符串转换成元组，并将结果以列表的形式返回***
>>> map(tuple, 'agdf')
[('a',), ('g',), ('d',), ('f',)]
#将小写转成大写
def u_to_l (s):
  return s.upper()
print map(u_to_l,'asdfd')
```

## python filter()函数

描述：

* filter()函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。
* 接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判断，返回True或False，将返回True的元素放到新列表中。

语法：

> filter(function, iterable)

参数：

* function：判断函数
* iterable：可迭代对象

返回值：

* 列表

实例：

​    

```
# 过滤列表中所有奇数
```

​    

```
def is_odd(n):

    return n % 2 == 1
```

​    

```
newlist = list(filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]))

print(newlist)
```

 输出结果：

> [1, 3, 5, 7, 9]

```
# 过滤出1~100中平方根是整数的数：
```

​    

```
import math

def is_sqr(x):

    return math.sqrt(x) % 1 == 0
```

​    

```
newlist = list(filter(is_sqrt, range(1, 101)))

print(newlist) 
```

 输出结果：

> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]



当出现这种错误时，是因为没将filter函数转换成list。

![](https://images2017.cnblogs.com/blog/1235684/201711/1235684-20171103103435591-2008900996.jpg)

## python之reduce函数

reduce函数对参数迭代器中的元素进行类累积 
格式为：reduce(func,iter,init)  

func为函数，iter为序列，init为固定初始值，无初始值时从序列的第一个参数开始

```
from functools import reduce
res=reduce(lambda x,y:x+y,['b','c','d','e'],'a')
print(res)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190621140731209.png)

```
from functools import reduce
res=reduce(lambda x,y:x*y,[1,2,3,4])
print(res)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/201906211405465.png)

```python
def add(x,y): # 两数相加
    return x+y
print(reduce(add,[1,2,3,4,5]))# 计算列表和：1+2+3+4+5
print(reduce(lambda x,y:x*y,[1,2,3,4]))# 使用 lambda 匿名函数
```

