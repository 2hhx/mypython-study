[TOC]

# 编译python文件

# 一、编译python文件

为了提高加载模块的速度，强调强调强调：提高的是加载速度而绝非运行速度。python解释器会在\_\_pycache\_\_目录中下缓存每个模块编译后的版本，格式为：module.version.pyc。通常会包含python的版本号。例如，在CPython3.3版本下，spam.py模块会被缓存成\_\_pycache\_\_/spam.cpython-33.pyc。这种命名规范保证了编译后的结果多版本共存。

Python检查源文件的修改时间与编译的版本进行对比，如果过期就需要重新编译。这是完全自动的过程。并且编译的模块是平台独立的，所以相同的库可以在不同的架构的系统之间共享，即pyc使一种跨平台的字节码，类似于JAVA火.NET,是由python虚拟机来执行的，但是pyc的内容跟python的版本相关，不同的版本编译后的pyc文件不同，2.5编译的pyc文件不能到3.5上执行，并且pyc文件是可以反编译的，因而它的出现仅仅是用来提升模块的加载速度的，不是用来加密的。

```python
# python解释器在以下两种情况下不检测缓存

1. 如果是在命令行中被直接导入模块，则按照这种方式，每次导入都会重新编译，并且不会存储编译后的结果（python3.3以前的版本应该是这样）
    python -m spam.py

2. 如果源文件不存在，那么缓存的结果也不会被使用，如果想在没有源文件的情况下来使用编译后的结果，则编译后的结果必须在源目录下

sh-3.2  # ls
__pycache__ spam.py
sh-3.2  # rm -rf spam.py 
sh-3.2  # mv __pycache__/spam.cpython-36.pyc ./spam.pyc
sh-3.2  # python3 spam.pyc 
spam
 

# 提示：

1. 模块名区分大小写，foo.py与FOO.py代表的是两个模块
2. 你可以使用-O或者-OO转换python命令来减少编译模块的大小
    -O转换会帮你去掉assert语句
    -OO转换会帮你去掉assert语句和__doc__文档字符串
    由于一些程序可能依赖于assert语句或文档字符串，你应该在在确认需要
    的情况下使用这些选项。
3. 在速度上从.pyc文件中读指令来执行不会比从.py文件中读指令执行更快，只有在模块被加载时，.pyc文件才是更快的
4. 只有使用import语句是才将文件自动编译为.pyc文件，在命令行或标准输入中指定运行脚本则不会生成这类文件，因而我们可以使用compieall模块为一个目录中的所有模块创建.pyc文件

模块可以作为一个脚本（使用python -m compileall）编译Python源  
python -m compileall /module_directory 递归着编译
如果使用python -O -m compileall /module_directory -l则只一层

命令行里使用compile()函数时，自动使用python -O -m compileall
  
详见：https://docs.python.org/3/library/compileall.html#module-compileall
```



# 二、批量生成`.pyc`文件

```python
import compileall
compileall.compile_dir('$dir')
#$dir 为Python源代码所在的目录。
```

