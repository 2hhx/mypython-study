# **一、环境准备**

[使用https://www.cnblogs.com/zhaof/p/8126306.html](https://www.cnblogs.com/zhaof/p/8126306.html)

[汉化https://jingyan.baidu.com/article/295430f1878f530c7f005040.html](https://jingyan.baidu.com/article/295430f1878f530c7f005040.html)

[运行python:ctrl+B https://jingyan.baidu.com/article/86f4a73eaff66637d65269f9.html](https://jingyan.baidu.com/article/86f4a73eaff66637d65269f9.html)

1、[官方网站地址:https://www.sublimetext.com/](https://www.sublimetext.com/)

2、Windows 10

3、Sublime Text 3 + 官网购买license（Just a suggestion，$80）

　　[购买链接](https://www.sublimetext.com/buy?v=3.0)，Sublime Text may be downloaded and evaluated for free, however a license must be purchased for continued use。如果资金紧张，可以搜索破解版License（建议支持正版，本文不再提供破解版License)。

# **二、安装Sublime Text 3**

1、双击下载的.exe文件安装，安装路径不要有中文目录

2、安装Sublime Text 3时，勾选**“Add to explorer context menu”**，可以在文件右键菜单添加“Open with Sublime Text”，方便使用Sublime Text打开文件。

# 三、配置Python环境

## **运行环境**

1、打开Tools > Build System > New Build System..

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813225408632-2105027740.png)

2、点击New Build System后，会生成一个空配置文件，在这个配置文件内覆盖配置信息，本文python安装路径为“D:/Anaconda3”，（注意区分正反斜杠，请将路径换成python实际安装路径），然后按ctrl+s，将文件保存在默认路径，文件名命名为“Python3”

```
{
    "cmd": ["D:/Anaconda3/python.exe","-u","$file"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.python",
}
```

如图：

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813230100549-667424665.png)

3、打开Tools > Build System，选择新建好的Python3即可

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813230248521-778731635.png)

**测试**

1、新建test.py文件，输入简单python语句，按Ctrl+B运行

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813231248999-2060320904.png)

## **字体及字号**

1、打开Preferences –>>Settings(Settings User),在右侧添加如下代码（font_face及font_size可根据个人喜好更改）



```
{
"color_scheme": "Packages/Color Scheme - Default/Monokai.sublime-color-scheme",
"font_face": "Consolas",
"font_size": 14,
"ignored_packages":
[
"Vintage"
],
"update_check": false,
// The number of spaces a tab is considered equal to
"tab_size": 4,
// Set to true to insert spaces when tab is pressed
"translate_tabs_to_spaces": true,
//设置保存时自动转换
"expand_tabs_on_save": true
}
```





 2、效果如下

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813234758999-430285791.png)

#  **四、安装Package Control(官网教程)**

## **第一种方法：在线安装**

通过快捷键[ctrl+`]或“View > Show Console”菜单打开控制台，将下面的Python代码粘贴到控制台里[
](https://sublime.wbond.net/installation)

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

注：上面代码仅适用于Sublime Text 3，如果是Sublime Text2请使用如下代码：

```
import urllib2,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
```

关掉并重新打开Sublime Text 3，如果在Perferences->package settings中能看到package control这一项，则表示安装成功。

## **第二种方法：手动安装**

1、点击 Preferences > Browse Packages… 菜单
2、Browse up a folder and then into the Installed Packages/ folder
3、Download Package [Control.sublime-package](https://packagecontrol.io/Package Control.sublime-package) and copy it into the Installed Packages/ directory
4、Restart Sublime Text

### **1、通过Package Control安装其他插件**

1.按下Ctrl+Shift+P调出命令面板

2.输入install 调出 Install Package

3.在列表中选中要安装的插件，或者输入插件名，根据命令面板中的过滤结果，选择要安装的插件

### **2、通过Package Control查看已安装的插件**

\1. 按下Ctrl+Shift+P调出命令面板

\2. 输入"package"，在下拉列表找到"Package Control: list packages"，选中后回车，可以显示全部插件列表。

# 五、常用插件介绍及安装

## **SublimeCodeIntel** 

**介绍**

Full-featured code intelligence and smart autocomplete engine

* Jump to Symbol Definition - Jump to the file and line of the definition of a symbol.
* Imports autocomplete - Shows autocomplete with the available modules/symbols in real time.
* Function Call tooltips - Displays information in the status bar about the working function.

**支持语言**

JavaScript, ES6, Mason, XBL, XUL, RHTML, SCSS, Python, HTML, Ruby, Python, XML, XSLT, Django, HTML5, Perl, CSS, Twig, Less, Smarty, Node.js, Tcl, TemplateToolkit, PHP.

**Installation**

1.Control+Shift+P打开Package Control控制台

2.输入install，选择关联出来的install package

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813171000282-1957946512.png)

3.输入sublimecodeintel，然后点击列表提示的sublimecodeintel安装

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813200940039-574769548.png)

4.安装完成之后，文本框会出现如下显示，或者可以通过【Preferences>Package Settings】中查看到已安装的sublimecodeintel插件

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813171556149-853882515.png)

5.打开preferences->packages settings ->Package Control ->Settings-User,检查是否有如下红框代码，如果没有得手动添加

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813172734097-26923195.png)

6.点击preferences中的browse Packages，进入SublimeCodeIntel文件夹，在当前的路径下新建.codeintel文件夹(**windows中文件命名的时候为 .codeintel.** )，之后进入到 .codeintel文件夹中，新建文件“config.log”文件，打开输入（下文以路径“D:\Anaconda3”为例，实际配置时请根据具体安装路径修改）：



```
"python3":{
    "python":"D:/Anaconda3/python.exe",
    "pythonExtraPaths":[
         "D:/Anaconda3/DLLs",
         "D:/Anaconda3/Lib",
         "D:/Anaconda3/Lib/lib-tk",
         "D:/Anaconda3/Lib/site-packages",
    ]
}
```



如图：

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813173721961-1173872481.png)

7.保存，重启Sublime Text 3

**测试**

新建文件并保存为.py文件，输入代码测试

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813174011127-1193959467.png)

 

**SublimeREPL**

**介绍**

添加快捷键后，可直接运行当前文件，非常方便

* Launch python in local or remote(1) virtualenv.
* Quickly run selected script or launch PDB.
* Use SublimeText Python console with history and multiline input.

**使用方法**

1、安装SublimeREPL插件后，打开Preferences->Key Bindings，添加快捷键：

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813204153739-2037178154.png)

代码粘贴：



```
[
    {
        "keys": ["f5"],
        "caption": "SublimeREPL: Python - RUN current file",
        "command": "run_existing_window_command",
        "args": {
            "id": "repl_python_run",
            "file": "config/Python/Main.sublime-menu"
        }
    }
]
```



 **测试**

1、新建test.py文件，例如：

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813204759536-594555151.png)

2、上文中SublimeREPL插件设置的快捷键是F5，所以按F5运行成功如下

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813204855905-1373831405.png)

 

##  **SublimeTmpl**

**介绍**

安装插件后，可以通过快捷键按照模板快速新建文件

**使用方法**

1、安装SublimeTmpl插件后，打开Preferences->Package Settings->SublimeTmpl->Settings User，添加以下内容



```
{  
    "disable_keymap_actions": false, // "all"; "html,css"  
    "date_format" : "%Y-%m-%d %H:%M:%S",  
    "attr": {  
        "author": "Yong Lee",  
        "email": "honkly@163.com",  
        "link": "http://www.cnblogs.com/honkly/"  
    }  
}
```



如图：

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813205642587-953534923.png)

 2、添加快捷键，打开Preferences->Key Bindings，添加红框中的快捷键代码



```
[
    {
        "caption": "Tmpl: Create python", "command": "sublime_tmpl",  
        "keys": ["ctrl+alt+n"], "args": {"type": "python"}  
    },
    {
        "keys": ["f5"],
        "caption": "SublimeREPL: Python - RUN current file",
        "command": "run_existing_window_command",
        "args": {
            "id": "repl_python_run",
            "file": "config/Python/Main.sublime-menu"
        }
    }
]
```



 如图：

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813210310957-658045534.png)

**测试**

打开Sublime Text 3，上文中SublimeTmpl插件设置的快捷键是"Ctrl+alt+n"，按快捷键后成功新建文件如下

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813210834699-359440590.png)

 

## **ColorSublime**

**介绍**

提供了主题更换功能，可根据个人喜好选择

**使用方法**

1、安装ColorSublime插件完成后，打开Preferences->Color Scheme...，选择主题

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813232903629-1602200697.png)

 

 **Anaconda**

**介绍**

代码提示等许多功能，必备

**安装方法**

1、Preferences->Package Settings->Anaconda->Settings Default,修改"python_interpreter"为实际Python安装路径

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180813233744562-1280486063.png)

2、Preferences->Package Settings->Anaconda->Settings User，添加如下内容



```
{
    "python_interpreter":"D:/Anaconda3/python.exe",
    "suppress_word_completions":true,
    "suppress_explicit_completions":true,
    "comlete_parameters":true,
    "swallow_startup_errors":true,
    "anaconda_linting":false
}
```



如图：

![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180818160227682-1065038624.png)

**测试**

1、打开Sublime Text3，新建任意*.py文件，输入语句测试，如图

**![img](https://images2018.cnblogs.com/blog/629575/201808/629575-20180818160425321-453984449.png)**



# 六、配置其他操作：

## 1.配置快捷键：

> 实现的功能是：
>
> #### F5 运行 python文件
>
> #### F4 打开Python交互环境
>
> #### F3 关闭窗口

### （1）.复制如下代码：



```json
[
    {"keys":["f5"],
    "caption": "SublimeREPL: Python - RUN current file",
    "command": "run_existing_window_command", "args":
    {"id": "repl_python_run",
    "file": "config/Python/Main.sublime-menu"}}
    ,
    {"keys":["f4"],
    "caption": "SublimeREPL: Python",
    "command": "run_existing_window_command", "args":
    {"id": "repl_python",
    "file": "config/Python/Main.sublime-menu"}}
    ,
    { "keys": ["f3"], "command": "close" }
]
```

### （2）.打开：Preference--》Key Builings



![img](https:////upload-images.jianshu.io/upload_images/14032224-2cc63e9c57b36df6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

10.png

### （3）粘贴到左侧框中，并按住：Ctrl + S 保存。关闭即可。



![img](https:////upload-images.jianshu.io/upload_images/14032224-28d665b65d2de612.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

快捷键.png

### （4）.测试效果：

#### a.分栏（View -> Layout ->Colunmns 2）

> 一般我喜欢吧屏幕分为两部分，一部分是用来编写代码，一部分用来查看运行结果。



![img](https:////upload-images.jianshu.io/upload_images/14032224-7816157c881cff3b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

分栏.png

#### b.文件式编程展示：F5



![img](https:////upload-images.jianshu.io/upload_images/14032224-d26a315fe82fd796.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

文件式.png

#### c.交互式编程展示：F4



![img](https:////upload-images.jianshu.io/upload_images/14032224-6a3aef46b1ccbc74.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

交互式.png

### d.退出运行，关闭运行窗口：F3



![img](https:////upload-images.jianshu.io/upload_images/14032224-3c49c857b11c1438.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

退出.png

## 2.项目的建立：

### （1）打开Project -> Add Folder to Project



![img](https:////upload-images.jianshu.io/upload_images/14032224-5e799d149ba8d3ba.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

项目.png

### （2）.选择一个文件夹作为项目：



![img](https:////upload-images.jianshu.io/upload_images/14032224-6d36ab09ee1b3653.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

## 3、创建模板：

> Sublime > Preferences > Package Settings > SublimeTmpl > Settings – User 添加如下代码

### （1）.复制如下代码 ：



```json
{  
    "disable_keymap_actions": false, // "all"; "html,css"  
    "date_format" : "%Y-%m-%d %H:%M:%S",  
    "attr": {  
        "author": "张一根",  
        "email": "2038145339@qq.com",  
        "link": "https://www.cnblogs.com/zyg123/"  
    }  
} 
```

### （2）. 打开Preferences > Package Settings > SublimeTmpl > Settings – User



![img](https:////upload-images.jianshu.io/upload_images/14032224-cbaf7b34d344791b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

模板.png

### （3）.粘贴刚才复制的代码：（并Ctrl + S 保存）



![img](https:////upload-images.jianshu.io/upload_images/14032224-3dd1245285f1f863.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

模板2.png

### （4）.查看效果（Ctrl + Shift +Alt + P）



![img](https:////upload-images.jianshu.io/upload_images/14032224-35e08566ccb03ae8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

[网址1：https://www.cnblogs.com/honkly/p/6599642.html](https://www.cnblogs.com/honkly/p/6599642.html)

[网址2：https://www.jianshu.com/p/d31141531057](https://www.jianshu.com/p/d31141531057)

[网址3：https://www.jianshu.com/p/5f4607748619](https://www.jianshu.com/p/5f4607748619)