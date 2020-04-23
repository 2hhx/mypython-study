## 1、IPython简介

`ipython`是一个`python`的交互式`shell`，比默认的`python shell`好用得多，支持变量自动补全，自动缩进，支持`bash
shell`命令，内置了许多很有用的功能和函数。学习`ipython`将会让我们以一种更高的效率来使用`python`。同时它也是利用Python进行科学计算和交互可视化的一个最佳的平台。

IPython提供了两个主要的组件：

    
    
    1.一个强大的python交互式shell 
    2.供Jupyter notebooks使用的一个Jupyter内核（IPython notebook）

IPython的主要功能如下：

    
    
    1.运行ipython控制台 
    2.使用ipython作为系统shell 
    3.使用历史输入(history) 
    4.Tab补全 
    5.使用%run命令运行脚本 
    6.使用%timeit命令快速测量时间 
    7.使用%pdb命令快速debug 
    8.使用pylab进行交互计算 
    9.使用IPython Notebook

## 2、安装IPython

ipython支持Python2.7版本或者3.3以上的版本

    
    
    pip install ipython

以上这条命令可以自动安装IPython以及它的各种依赖包，但是如果我们想在notebook中操作ipython的话，我们还需要安装jupyter：

    
    
    pip install jupyter

## 3、使用IPython的两种方式

Python支持所有python的标准输入输出，也就是我们在IDLE中或者Python
shell中能用的，在IPython中都能够使用，唯一的不同之处使ipython会使用`In [x]`和`Out [x]`表示输入输出，并表示出相应的序号。
**In和Out是两个保存历史信息的变量**

> 交互式

直接打开命令行或者终端，输入ipython,即可进入ipython环境  
![](https://img2018.cnblogs.com/blog/1825659/202001/1825659-20200113220339323-1699074179.png)

> Jupyter notebook

Jupiter notebook就类似于ipython的编辑器，他是一个文本工具，它是在你电脑本地开了一个服务端，将它运行在浏览器上。

windows，mac通用启动命令：jupyter notebook  
![](https://img2018.cnblogs.com/blog/1825659/202001/1825659-20200113220502789-682074053.png)

## 4、IPython基础功能

ipython快捷键

    
    
    - Ctrl-P    或上箭头键 后向搜索命令历史中以当前输入的文本开头的命令
    - Ctrl-N   或下箭头键 前向搜索命令历史中以当前输入的文本开头的命令
    - Ctrl-R   按行读取的反向历史搜索（部分匹配）
    - Ctrl-Shift-v   从剪贴板粘贴文本
    - Ctrl-C   中止当前正在执行的代码
    - Ctrl-A   将光标移动到行首
    - Ctrl-E   将光标移动到行尾
    - Ctrl-K   删除从光标开始至行尾的文本
    - Ctrl-U   清除当前行的所有文本译注12
    - Ctrl-F   将光标向前移动一个字符
    - Ctrl-b   将光标向后移动一个字符
    - Ctrl-L   清屏

## 5、IPython高级功能

一些常用的高级功能比如：

  * TAB键自动完成
  * ？：内省、命名空间搜索
  * ！：执行系统命令

以及一系列魔术命令

### 5.1、魔术命令：以%开始的命令

    
    
    %run:执行文件代码
    “”“
    类似于Cpython中在命令行中 python+文件路径
    ”“”
    %paste:执行剪贴板代码
    
    %timeit:评估运行时间 # 补充一个：%%time
    
    %pdb:自动调试

IPython常用的魔术命令：

方法 | 描述  
---|---  
%quickref | 显示IPython的快速参考  
%magic | 显示所有魔术命令的详细文档  
%debug | 从最新的异常跟踪的底部进入交互式调试器  
%hist | 打印命令的输入（可选输出）历史  
%pdb | 在异常发生后自动进入调试器  
%paste | 执行剪贴板中的Python代码  
%cpaste | 打开一个特殊提示符以便手工粘贴待执行的Python代码  
%reset | 删除interactive命名空间中的全部变量/名称  
%page OBJECT | 通过分页器打印输出OBJECT  
%run script.py | 在IPython中执行一个Python脚本文件  
%prun statement | 通过cProfile执行statement，并打印分析器的输出结果  
%time statement | 报告statement的执行时间  
%timeit statement | 多次执行statement以计算系综平均执行时间。对那些执行时 间非常小的代码很有用  
%who、%who_ls、%whos | 显示interactive命名空间中定义的变量，信息级别/冗余度可变  
%xdel variable | 删除variable，并尝试清除其在IPython中的对象上的一切引用

