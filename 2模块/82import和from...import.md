[TOC]

# import和from...import

一般使用jimport和form...import导入模块

下面以spam.py为列

```python
#spam.py
print('from the spam.py')
money = 1000
def read1():
    print('spam模块：',money)
def read2():
    print('spam模块')
    read1()
def change():
    global money
    money = 1
    

```





# 一、import模块名

```python

# python看成一个手机,pip是应用管家,time就是应用管家里的一个应用,要用它,import
import time
time.time()




```

用上面导入模块time()为列：

import 发生的3件事情

1. 在内存中生成一个叫做time的名称空间.
2. 运行time.py文件,然后把time.py文件内的名称空间放入time的名称空间内.
3.  把time的名称空间指向 import和from...impot.py(当前导入time模块的文件)  的名称空间中
4. 使用import time导入的时候使用方法只能 time.方法名()  ,不能直接方法名.

模块的重复导入会直接饮用之前创造好的结果，不会重复执行模块的文件，即重复导入会发生：spam=spam=模块名称空间的内存地址。



可以同时导入多个模块

```python
import time,os,requests
#建议使用下面
import time
import os
import requests

```

# 二、form模块名import具体的功能

```python
from time import gmtime
```

from...import...首次导入模块发生了3件事:

1. 以模块为准创造一个模块的名称空间
2. 执行模块对应的文件，将执行过程中产生的名字都丢到模块的名称空间
3. 在当前执行文件的名称空间中拿到一个名字，该名字直接指向模块中的某一个名字，意味着可以不用加任何前缀而直接使用

- 优点：不用加前缀，代码更加精简
- 缺点：容易与当前执行文件中名称空间中的名字冲突

导入文件内的所有功能：

```python
__all__ = ['money', 'read1']  # 只允许导入'money'和'read1'
form spam import *  # 导入模块内所有的功能，在函数调用的时候只能调用__all__内的函数和变量名，受到__all__的限制
```



# 三、import 和from...import的异同

1. 相同点
   1. 两者都会执行模块对应的文件，两者都会产生模块的名称空间。
   2. 两者调用功能时，都需要跑到定义时寻找作用域关系，与调用的位置无关。
2. 不同点
   1. import需要加前缀，form...import...不需要加前缀