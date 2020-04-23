# [Python如何生成windows可执行的exe文件](https://segmentfault.com/a/1190000016087451)

[pyinstaller官网](http://www.pyinstaller.org/downloads.html)

pyinstaller -F thired.py

### 为什么要生成可执行文件

* 不需要安装对应的编程环境
* 可以将你的应用闭源
* 用户可以方便、快捷的直接使用

### 打包工具

* pyinstaller

#### 安装pyinstaller

如果你的网络稳定，通常直接使用下面的命令安装即可：

```
pip install pyinstaller
```

当然了，你也可以下载pyinstaller源码包，然后进入包目录执行下面的命令，同样可以安装（前提是需要安装setuptools）：

```
python setup.py install
```

安装过程如下图所示

![图片描述](https://segmentfault.com/img/bVbfFej?w=925&h=308)

#### 检查pyinstaller安装成功与否：

只需要执行如下命令其中一个即可：

```
pyinstaller --version
pyinstaller -v
```

如果出现如下界面，就说明是安装成功了

![图片描述](https://segmentfault.com/img/bVbfFd8?w=609&h=141)

### pyinstaller参数作用

* -F 表示生成单个可执行文件
* -D –onedir 创建一个目录，包含exe文件，但会依赖很多文件（默认选项）
* -w 表示去掉控制台窗口，这在GUI界面时非常有用。不过如果是命令行程序的话那就把这个选项删除吧
* -c –console, –nowindowed 使用控制台，无界面(默认)
* -p 表示你自己自定义需要加载的类路径，一般情况下用不到
* -i 表示可执行文件的图标
* 其他参数，可以通过`pyinstaller --help`查看



打包的app里并不包含任何源码，但将脚本的.pyc文件打包了。

基本语法：

pyinstaller options myscript.py
常用的可选参数如下：
--onefile 将结果打包成一个可执行文件
--onedir 将所有结果打包到一个文件夹中，该文件夹包括一个可执行文件和可执行文件执行时需要的依赖文件（默认）
--paths=DIR 设置导入路径
--distpath=DIR 设置将打包的结果文件放置的路径
--specpath=DIR 设置将spec文件放置的路径
--windowed 使用windows子系统执行，不会打开命令行（只对windows有效）
--nowindowed 使用控制台子系统执行（默认）（只对windows有效）
--icon=<FILE.ICO> 将file.ico添加为可执行文件的资源(只对windows有效）

如pyinstaller --paths="D:\Queena" guess_exe.py

示例: pyinstaller -F wxy_tool.py --distpath=wxy-mtt

pyinstaller -F thired.py



4.优化

因为这个打包会出现很多需要依赖的文件，那如果我只想要一个exe怎么办呢？那么这时候就需要加一个-F参数就行：pyinstaller -F xxx.py就行了。

更多参数：

-F, –onefile	打包一个单个文件，如果你的代码都写在一个.py文件的话，可以用这个，如果是多个.py文件就别用
-D, –onedir	打包多个文件，在dist中生成很多依赖文件，适合以框架形式编写工具代码，我个人比较推荐这样，代码易于维护
-K, –tk	在部署时包含 TCL/TK
-a, –ascii	不包含编码.在支持Unicode的python版本上默认包含所有的编码.
-d, –debug	产生debug版本的可执行文件
-w,–windowed,–noconsole	使用Windows子系统执行.当程序启动的时候不会打开命令行(只对Windows有效)
-c,–nowindowed,–console	
使用控制台子系统执行(默认)(只对Windows有效)

pyinstaller -c  xxxx.py

pyinstaller xxxx.py --console

-s,–strip	可执行文件和共享库将run through strip.注意Cygwin的strip往往使普通的win32 Dll无法使用.
-X, –upx	如果有UPX安装(执行Configure.py时检测),会压缩执行文件(Windows系统中的DLL也会)(参见note)
-o DIR, –out=DIR	指定spec文件的生成目录,如果没有指定,而且当前目录是PyInstaller的根目录,会自动创建一个用于输出(spec和生成的可执行文件)的目录.如果没有指定,而当前目录不是PyInstaller的根目录,则会输出到当前的目录下.
-p DIR, –path=DIR	设置导入路径(和使用PYTHONPATH效果相似).可以用路径分割符(Windows使用分号,Linux使用冒号)分割,指定多个目录.也可以使用多个-p参数来设置多个导入路径，让pyinstaller自己去找程序需要的资源
–icon=<FILE.ICO>	
将file.ico添加为可执行文件的资源(只对Windows系统有效)，改变程序的图标  pyinstaller -i  ico路径 xxxxx.py

–icon=<FILE.EXE,N>	将file.exe的第n个图标添加为可执行文件的资源(只对Windows系统有效)
-v FILE, –version=FILE	将verfile作为可执行文件的版本资源(只对Windows系统有效)
-n NAME, –name=NAME	可选的项目(产生的spec的)名字.如果省略,第一个脚本的主文件名将作为spec的名字
### 开始打包

进入python需要打包的脚本所在目录，然后执行下面的命令即可：

```
pyinstaller -F -i favicon.ico nhdz.py
```

执行过程如下图所示：

![图片描述](https://segmentfault.com/img/bVbfFeU?w=767&h=284)

### 打包结果

打包完成后，进入到当前目录下，会发现多了__pycache__、build、dist、nhdz.spec这四个文件夹或者文件，其中打包好的exe应用在dist目录下面，进入即可看到，可以把他拷贝到其他地方直接使用，如下图所示，是打包完成后的目录：

![图片描述](https://segmentfault.com/img/bVbfFex?w=740&h=186)

### 执行exe应用

因为是exe应用，是可执行文件了，所以直接双击运行即可，运行效果如下图所示：

![图片描述](https://segmentfault.com/img/bVbfFeF?w=668&h=253)

到这里，exe文件就已经生算是打包完成，并且可以运行了，如果你想在其他平台运行，只需要拷贝dist下面的文件即可

### ICO图标制作

前面需要用到ICO图标，大家可以网上搜索“ICO 在线生成”，可以直接点击[ICO图标制作](http://ico.duduxuexi.com/)在上面制作、然后保存也行





# 解决方案

Windows下利用pyinstaller打包Python3.6脚本
最近用python写了一个TensorFlow程序，基于谷歌的facenet来检测人脸，我写的是服务器端，包括一个tcp通讯协议，问题来了，如何将其打包成一个exe文件发布？

本人电脑：

Windows 10 系统；

Python 3.6.3（Anaconda 3.5.0.1安装）；

TensorFlow 1.4.0（GPU版本，1050Ti）

PyInstaller 3.3.1




0、入坑前的准备工作
网上最为流行的就是PyInstaller方法了，我决定使用这个方法将我的py文件打包成exe。首先，明确最新版的pyinstaller已经支持python3.6版本的打包工作，我们可以登录PyInstaller的官网看看下面的消息：



截止本人写这篇博客，最新版的PyInstaller是3.3.1：



使用pip安装步骤非常简单，就是一步：

pip install pyinstaller
然后使用也很简单，在windows下按Win+R进入命令行，输入cmd，然后进入你的py文件所在的文件夹：（我的程序放在了桌面的AeyeFaceDetection_python文件夹内）

cd desktop\AeyeFaceDetection_python
接着使用下面的命令生成exe文件：

pyinstaller -F main.py
用-F意味着可以生成单个可执行文件，如果是下面的方法：

pyinstaller -F -w main.py
则表示去掉控制台窗口，这在GUI界面时非常有用。不过如果是命令行就不要这样写。

现在我们假设已经按照-F方法生成成功，那么在我们的py文件所在的文件夹内可以看到两个新生成的文件夹，名字为build和dist，并且在我们要生成的py文件下有一个同名的spec文件，这个文件的作用在网上可以百度的到，我这里就不作叙述了。





1、第一个坑：requests版本问题
我按照官方教程和各位网友的操作，发现第一个问题就是这样的：

  File "d:\software\anaconda3.5.0.1\lib\site-packages\PyInstaller\utils\hooks\__init__.py", line 619, in collect_submodules
    repr(pkg_dir), package))
  File "d:\software\anaconda3.5.0.1\lib\site-packages\PyInstaller\utils\hooks\__init__.py", line 90, in exec_statement
    return __exec_python_cmd(cmd)
  File "d:\software\anaconda3.5.0.1\lib\site-packages\PyInstaller\utils\hooks\__init__.py", line 77, in __exec_python_cmd
    txt = exec_python(*cmd, env=pp_env)
  File "d:\software\anaconda3.5.0.1\lib\site-packages\PyInstaller\compat.py", line 588, in exec_python
    return exec_command(*cmdargs, **kwargs)
  File "d:\software\anaconda3.5.0.1\lib\site-packages\PyInstaller\compat.py", line 378, in exec_command
    out = out.decode(encoding)
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xce in position 125: invalid continuation byte
由于问题过于庞杂，不太好找原因，我就先写了小的测试脚本，命名为d.py，放在原文件夹下：

import time
print("zhangping is a man.")
time.sleep(5)
注意这里的延时是必要的，否则程序会一闪而过，就类似于C++中添加一个getchar()一样。

运行

pyinstaller -F d.py
发现可以成功，如下图：



就走我以为一切都顺利的时候，运行C:\Users\zhangping\Desktop\AeyeFaceDetection_python\dist\d.exe，发现程序崩溃：



这个问题在网上我搜了好久，包括增加环境变量等各种方法都尝试过了，都没有用，最后找到了这个：

https://www.zhihu.com/question/53717334

完美解决的方法如下：

（1）首先要查看自己的requests版本，高于2.10就自行卸载：



（2）卸载：

pip uninstall requests
（3）重新安装requests2.10.0版本：

pip install requests==2.10.0
再次打包就可以运行d.exe了。

然而打包main.py还是报错。

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xce in position 125: invalid continuation byte

2、第二个坑：UnicodeDecodeError: 'utf-8' 问题
网上好多方法均不可行，因为我的路径下没有任何中文，（虽然我的人脸数据库中的库中人名是中文），但网上很多的这种解决方案是不适合我的。

比如：http://blog.csdn.net/u010654583/article/details/79444330

用管理员权限是无法解决这个问题的。还是会报这个错误。

在查找了很多文章后，我看见了这个：

http://blog.csdn.net/u011529752/article/details/54892488

我按照他的思路，首先改变编码格式，先输入chcp 65001，表示使用UTF-8。

chcp 65001
然后运行：



最后显示打包成功！









3、第三个坑：import matplotlib问题
原以为打包成功，结果我双击C:\Users\zhangping\Desktop\AeyeFaceDetection_python\dist\main.exe，遇到这样的问题：



其实这个问题在上面那个链接里面就已经有写了。但他写的方法只能对matplotlib在打包时候不出错，但不能使程序在运行时不出错！！！

根据错误的提示，在main.py的第8行，我找到了这句代码：

import matplotlib.pyplot as plt
很凑巧的是，这句代码和我的程序无关，当初加进来也没用上，所以我就直接删除了。

然后再次打包，运行exe文件，完美！





4、综上：
（1）要注意看看自己的requests版本，不出意外的话都说比较高的版本，要卸载，重装为2.10.0版本；

（2）要改变编码格式，先输入chcp 65001；

（3）要注意程序中是否出现了matplotlib的引入，如果有，则继续寻找解决办法（我没用到，就没有继续深究了）

（4）程序所在的目录最好不出现中文。











5、新坑：

AttributeError: module 'enum' has no attribute 'IntFlag'问题
https://blog.csdn.net/qq_41185868/article/details/80599336



6、新坑：

Maximum recursion depth exceeded 问题
https://blog.csdn.net/Sagittarius_Warrior/article/details/78457824



7、新坑：

AttributeError：type object 'pandas._libs.tslib_TSObject' has no attribute '__reduce_cpython__' 问题


尚未解决
------------------------------------------------
版权声明：本文为CSDN博主「天风海雨」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zhang669154/article/details/79434911







# 使用 py2exe 打包 Python 程序

[参考](https://blog.csdn.net/bruce_6/article/details/82906444)

原创hoxis 最后发布于2018-09-30 10:40:14 阅读数 16186  收藏
展开
上回在《使用 PyInstaller 打包 Python 程序》中，我们介绍了使用 PyInstaller 对 Python 程序进行打包，今天带大家认识一个新的工具：py2exe。

接下来将从这几个方面进行介绍：基本使用方法、高级参数、注意点等。

简介 & 安装
py2exe 是一个将 python 脚本转换成 Windows 上的可独立执行的可执行程序（*.exe）的工具，这样，你就可以不用装 python 而在 Windows 系统上运行这个可执行程序。

安装
pip install py2exe

或者

python -m pip install py2exe

基本用法
看一个简单的例子：先写一个简单的脚本，文件名：helloworld.py：

#!/usr/bin/env python  

-*- coding: utf-8 -*-  

```
def say_hello(name):  
    print("Hello, " + name)

if __name__ == "__main__":  
    name = input("What's your name：")
    say_hello(name)
```



下面还需要个用于发布程序的设置脚本：mysetup.py，在其中的 setup 函数前插入语句

```
import py2exe。

from distutils.core import setup
import py2exe

setup(console=["helloworld.py"])
```



然后按下面的方法运行 mysetup.py:

```
python mysetup.py py2exe
```



运行生成的文件：



需要注意，这里需要在 Windows 环境下运行！否则可能会出现以下异常：



上面的命令执行后将产生一个名为 dist 的子目录，其中包含了 helloworld.exe、python24.dll、library.zip 等等文件：



dist 子目录中的文件包含了程序所必须的东西，你需要将该目录中的所有内容一起发布。

默认情况下，py2exe 会在 dist 下创建以下这些文件：

1、一个或多个 exe 文件；
2、几个 .pyd 文件，它们是已编译的扩展名，是 exe 文件所需要的；
3、python**.dll，加上其它的 .dll 文件，这些 .dll 是 .pyd 所需要的；
4、一个 library.zip 文件，它包含了已编译的纯的 python 模块如 .pyc 或 .pyo；

扩展
setup 优化
我们可以看到生成的 dist 目录中文件很多，那么是不是可以进行优化呢？

```
# mysetup.py

# from distutils.core import setup

# import py2exe

# setup(console=["helloworld.py"])

# -*- encoding:utf-8 -*-

from distutils.core import setup
import py2exe

INCLUDES = []

options = {
    "py2exe" :
        {
            "compressed" : 1, # 压缩   
            "optimize" : 2,
            "bundle_files" : 1, # 所有文件打包成一个 exe 文件  
            "includes" : INCLUDES,
            "dll_excludes" : ["MSVCR100.dll"]
        }
}

setup(
    options=options,    
    description = "this is a py2exe test",   
    zipfile=None,
    console = [{"script":'helloworld.py'}])
```



options 可以用来指定一些编译的参数，譬如是否压缩，是否打包为一个文件等。

再次运行后，发现所有内容打包进了一个 helloworld.exe 程序中。

指定额外的文件
一些应用程序在运行时需要额外的文件，诸如配置文件、字体、图标。py2exe 并不会自动把他们打包到 dist 目录，不过可以通过配置参数来打包。

可以在安装脚本中用 data_files 可选项指定了那些额外的文件，那么 py2exe 能将这些文件拷贝到 dist 子目录中。

格式如下：data_files=[(“目的文件夹”,[“文件名”,]), (“目的文件夹”,[“文件名”,]), (“目的文件夹”,[“文件名”,]),]。

比如，我们的程序中有一个名为 images 的目录放置了程序需要的图片，

那么我们就需要在 setup 函数中配置参数 data_files，这个参数包含一个元组列表 (target_dir,files)，其中 target_dir 是指定文件存放的目标路径，files 是这些额外文件的一个列表。

示例如下：



    from distutils.core import setup
    import py2exe
    
    setup(windows = ['hello.py],
    data_files = [('images',['images\*.jpg'])]
    )

上面的示例中，会把 images 目录中所有的 jpg 文件打包到 dist/images 子目录中。

注意点
1、py2exe 新版本只支持 python3.3 以上，可以使用 pip install py2exe_py2 来安装兼容 python2 版本；
2、若在 python3.6 版本下运行报错，请切换到 python3.4 尝试；
3、python3 如果是 64 位，生成的 exe 只能在 64 位操作系统下运行，使用 32 位 python 可以解决；

4、从 Python 3.3，Windows 在构建 Python 时使用的是 Visual Studio 2010，因此生成后，需要手动将 msvcr100.dll 拷到生成目录下（dist目录），否则最终的文件运行时可能会报错；

或者通过 data_files=[("",["MSVCR100.dll"])], 打包其中；

比如，我在 Win10 下打的包，拷贝到 Win7 上，运行出错：



出现类似确实 dll 文件的情况，都可以参考这种方法进行解决；

总结
对于 pyinstaller 和 py2exe 两种把 Python 文件打包成 exe 的可执行文件的方法，都有各自的优缺点。但是最终目的都是为了在没有 Python 环境下的普通 Windows 系统的电脑中可直接运行，这点还是很不错的。

大家根据自己的需要，择优选择就行了。

参考：
1、http://irootlee.com/Py2exe/
2、https://www.jianshu.com/p/afc56b64786

------------------------------------------------
版权声明：本文为CSDN博主「hoxis」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/bruce_6/article/details/82906444