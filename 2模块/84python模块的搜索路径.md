[TOC]

# 84python模块的搜索路径

# 一、模块搜索路径的顺序

模块其实就是一个文件，如果要执行文件，首先就需要找到模块的路径(某个文件夹)，如果模块的文件路径和执行文件不在同一个文件的目录下，我们就需要指定文件的路径。

模块的搜索路径指的是在导入模块时需要检索的文件夹。

导入模块时查找模块的顺序是：

1. 先从内存中已经导入的模块中寻找
2. 内置的模块
3. 环境变量sys.path中查找

```python
import sys
print(sys.path)



#输出：
['F:\\python学习\\测试\\df', 'F:\\python学习', 'D:\\pythonIDE\\PyCharm 2019.1.3\\helpers\\pycharm_display', 'D:\\Python\\python37.zip', 'D:\\Python\\DLLs', 'D:\\Python\\lib', 'D:\\Python', 'D:\\Python\\lib\\site-packages', 'D:\\pythonIDE\\PyCharm 2019.1.3\\helpers\\pycharm_matplotlib_backend']
```



==强调：sys.path的第一个值是当前执行文件的所在的文件夹==

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190818143417025-206595861.jpg)

# 1.1验证先从内存中找

如果我们在运行run.py文件的时候，快速删除mmm.py文件，我们会发现文件会继续运行，而且不会报错，因为mmm.py已经被导入内存，如果我们在一次运行run.py时会报错，因为mmm.py已经被删除了。



```pytho
#m2.py

def f1():
    print('111')
f1()
```



```python
import time
import m2
time.sleep(1)
m2.f1()
```

# 1.2验证先从内置中找

```python
#time.py
print('111')
```

```python
import time
print(time)

#输出：<module 'time' (built-in)>

```

# 1.3验证从sys.path环境变量中找

```python

4. 环境变量中  (主要记住未来项目的执行文件一定要弄一个环境变量)
import sys
print(sys.path)  # 环境变量,模块就是在这里找
sys.path.append(r'F:\python学习\0016模块基础\04 模块的搜索路径')
# del sys.path[1]
print(sys.path)

import testt
testt.f1()

```

# 总结

模块的搜索路径是：内存 --> 内置 --> 自定制 --> 环境变量