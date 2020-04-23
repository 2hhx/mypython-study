# 中文界面

1. 文界面是根据你的shell界面来自适应的。也就是说，你需要一个中文的shell端。

   [![如何设置jupyter notebook为中文界面](https://imgsa.baidu.com/exp/w=500/sign=0b4f2f556e2762d0803ea4bf90ed0849/242dd42a2834349ba198fef3c4ea15ce36d3be2e.jpg)](http://jingyan.baidu.com/album/22fe7cede06bc73002617f32.html?picindex=3)

2. 

   上面的shell是安装好git之后，自带的Git Bush，大家可以自行搜索安装。git这一神器，是必须要有的，命令行也是要掌握的。亲们加油。

   [![如何设置jupyter notebook为中文界面](https://imgsa.baidu.com/exp/w=500/sign=b39072248835e5dd902ca5df46c7a7f5/bd3eb13533fa828b5a4c0f95f01f4134970a5a5d.jpg)](http://jingyan.baidu.com/album/22fe7cede06bc73002617f32.html?picindex=4)

3. 

   直接在右键打开git bush，输入jupyter notebook试试吧

   [![如何设置jupyter notebook为中文界面](https://imgsa.baidu.com/exp/w=500/sign=2baac4616fd0f703e6b295dc38fb5148/d52a2834349b033bfa0c6eac18ce36d3d439bdc4.jpg)](http://jingyan.baidu.com/album/22fe7cede06bc73002617f32.html?picindex=5)

# 什么是 notebook 扩展插件？

Jupyter Notebook 扩展插件是扩展 notebook 环境基本功能的简单插件。它们用 JavaScript
语言编写，会自动套用代码格式或者在单元格完成后发送浏览器通知。扩展插件目前仅支持 Jupyter Notebook（不支持 Jupyter Lab）。

# 为什么要使用扩展插件？

Jupyter Notebook 是一个很好用的工具，可用于教学、学习、原型设计、探索和尝试新方法（甚至可用于 Netflix 的生产过程中）。但是，原版
notebook 功能有限，有时令人挫败。虽然 Jupyter Notebook 扩展插件没有完全解决这个问题，但它们确实能让你的工作变得更轻松。

# 插件配置安装

直接使用pip安装


​    
    > pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install

启动 Jupyter Notebook，并导航至新的 Nbextensions 选项卡：

![](https://img2018.cnblogs.com/blog/1825659/202001/1825659-20200108164837370-1093398033.png)

选择你想要的扩展功能，享受它带来的优势。

（如果你没看到扩展选项，打开 notebook，单击「edit」，然后点「nbextensions config」）

在 notebook 的工具栏里可以看到扩展插件

# 常用插件列举

## 1 Table of Contents：更容易导航

如果你在一个 Jupyter Notebook 中同时开启了十几个单元格，那你想跟踪所有单元格就会有些困难。Table of Contents 通过添加
TOC 链接解决了这个问题，通过 TOC 链接你可以定位到页面中的任何位置。还可以使用该扩展插件在 notebook
的顶部添加一个链接目录。这样会显示你选择了哪一个目录以及哪一个正在运行：

![](https://img2018.cnblogs.com/blog/1825659/202001/1825659-20200108164851528-2054978491.png)

## 2 Autopep8：轻轻一击就能获得简洁代码

我们都应该编写符合 pep8 标准的代码，但有时你会陷入分析，难以坚持这种标准。所以当你写完代码后，只要单击这个选项，就可以让代码变得简洁漂亮。

这个插件可以称得上是最好的插件了，仅需点击一下，就能完成一项耗时又乏味的工作，让你专注于思考。

> 注意点：这个插件还需要通过pip安装一个autopep8的工具包

## 3 variable inspector：跟踪你的工作空间

variable inspector 会显示你在 notebook 中创建的所有变量的名称，以及它们的类型、大小、形状和值。

![](https://img2018.cnblogs.com/blog/1825659/202001/1825659-20200108164922909-574217088.png)

这个工具对于从 RStudio 迁移过来的数据科学家来说是无价之宝。如果你不想继续打印 df.shape 或无法重新调用 x 的
type，这个工具对你来说也同样重要。

## 4 ExecuteTime：显示单元格的运行时间和耗时

我经常不知道某个单元格需要运行多久或者最后一次运行一个打开好几天的 notebook 是什么时候。ExecuteTime
完美解决这个问题，它会显示单元格的运行完成时间和所耗时长

![](https://img2018.cnblogs.com/blog/1825659/202001/1825659-20200108164931688-324045294.png)

的确有更好的计时方法，如 %%time，但 ExecuteTime 易于实现，且可以覆盖 notebook 中的所有单元格。

## 5 隐藏代码输入：隐藏过程，展示结果

虽然有些人喜欢看到某项艰苦工作的具体分析，但有些人却只想看到结果。隐藏所有输入的插件让你能够立即隐藏 notebook 中的所有代码，只保留结果。

# 结论

安装 Jupyter Notebook
扩展插件，花点时间弄清楚哪些有用，然后提高自己的工作效率。虽然这些功能不至于改变你的人生，但它们带来的益处也是值得的。而且累积起来为你节约了很多宝贵的开发时间。

# 使用Git Bash激活python虚拟环境



![img](https://3x2.myfastcdn.com/www/images/c5b9a6300637bb02b3c4831434b3020c.jpeg?width=492)



Allblock - blocks all types of ads!



# 前言

在Window环境下，我使用Git Bash模拟Linux内的terminal进行sh操作。在sh操作之前，需要进入python创建的虚拟环境。
![在这里插入图片描述](https://www.pianshen.com/images/676/d4e5041b959bbeabf28e28fc71ca37bc.png)

# 方法

首先要确保此虚拟环境内安装`virtualenv`这个包，有了这个包，才能确保此虚拟环境的Script目录下包含`activate.bat`命令。
![在这里插入图片描述](https://www.pianshen.com/images/870/3f551a92cddafdf8902e155b0003ad9e.JPEG)
启动Git Bash，输入命令`source 虚拟环境目录/Scripts/activate`
例如：`E:/Python/virtualenv/deep_sort_pytorch/Scripts/activate`
![在这里插入图片描述](https://www.pianshen.com/images/79/ffdbabd9ba7afdcea8ae8b58aae9dda7.png)
此时，就进入了`deep_sort_pytorch`虚拟环境内。

