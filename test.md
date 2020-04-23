* 什么是数据可视化
* Matplotlib的用法
* 金融学图表
* 保存图表

# 1、什么是数据可视化

数据可视化在量化分析当中是一个非常关键的辅助工具，往往我们需要通过可视化技术，对我们的数据进行更清晰的展示，这样也能帮助我们理解交易、理解数据。通过数据的可视化也可以更快速的发现量化投资中的一些问题，更有利于分析并解决它们。接下来我们主要使用的可视化工具包叫做——
_`Matplotlib`_ ，它是基于Numpy和tkinter二次开发的，它是一个强大的Python绘图和数据可视化的工具包。

# 2、Matplotlib的用法

## 2.1、Matplotlib绘图基础

安装方式：

> pip install matplotlib

引用方法：

> import matplotlib.pyplot as plt

matplotlib是python中的2D绘图库，也是目前使用最广泛的python绘图库。虽然它很庞大，但是可以通过简单的概念框架和重要的知识来理解掌握。它的图像大概可以分为以下4层结构。

1）canvas（画板）：位于最底层，导入matplotlib库时就自动存在。

2）figure（画布）：建立在canvas之上，从这一层就可以开始设置参数

3）axes（子图）：将figure分成不同的块，实现分面绘图

4）图表信息（构图元素）：添件或修改axes上的图形信息，优化图表的显示效果

## 2.2、绘图基本流程

根据以上matplotlib的四层图像结构，pyplot模块绘制图形基本都遵循一个流程。

![pyplot基本绘图流程](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142731209-811607507..jpg)

> 导入模块

先导入相应工具包。根据官方认证方式导入：

​    
​    import numpy as np
​    import matplotlib.pyplot as plt

> 创建画布和子图

首先创建一张空白的画布，设置画布大小，根据需要同时展示几个图形，可以将画布划分为多个部分。然后使用对象方法来完成其余的工作。

​    
​    pic = plt.figure(figsize=(10,10),dpi=80)  # 创建一个10 x 10的画布，像素值为80
​    ax1 = pic.add_subplot(2,1,1)  # 划分为2 x 1的图形阵，选择第一张图片

> 添加画布内容

绘图的主体部分。添加标题、坐标轴名称等操作与绘制图形时并列的，没有先后顺序，可以先绘制图形，也可以先添加各类标签，但是添加图例一定要在绘制图形之后。

| 方法         | 描述         |
| ------------ | ------------ |
| plt.title()  | 设置图像标题 |
| plt.xlabel() | 设置x轴名称  |
| plt.ylabel() | 设置y轴名称  |
| plt.xlim()   | 设置x轴范围  |
| plt.ylim()   | 设置y轴范围  |
| plt.xticks() | 设置x轴刻度  |
| plt.yticks() | 设置y轴刻度  |
| plt.legend() | 设置曲线图例 |

> 图形保存与展示

```
plt.savefig('图片名称+后缀名')  # 保存图片，可以自由指定图片格式
plt.show()  # 展示图形
```

> 整体流程

```
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure(figsize = (10,10),dpi = 80)  # 创建画布。大小10x10，像素80
x = np.linspace(0,1,1000)  # 通过numpy生成随机数
fig.add_subplot(2,1,1)  # 分为2x1图形阵，选择第一张图片绘图
plt.title('y=x^2 or y=x')  # 添加标题
plt.xlabel('x')  # 添加x轴名称
plt.ylabel('y')  # 添加y轴名称
plt.xlim((0,1))  # 设置x轴范围（0，1）
plt.ylim((0,1))  # 设置y轴范围（0，1）
plt.xticks([0,0.3,0.6,1])  # 设置x轴刻度
plt.yticks([0,0.5,1])  # 设置y轴刻度
plt.plot(x,x**2)
plt.plot(x,x)
plt.legend(['y=x^2','y=x'])  # 添加图例
plt.savefig('整体绘图流程.png')  # 保存图片
plt.show()  # 展示图片
```

![整体绘图流程](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142731419-685764620..png)

## 2.3、绘图风格

在matplotlib中,pyplot的一个子模块style当中定义了很多预设风格，方便进行风格转换。每个预设风格都存储在一个.mplstyle为后缀的style文件中。

通过`print(plt.style.available)`可以查看所有预设风格的名称，通过use函数就可以直接设置预设风格。

> 查看所有风格名称

```
print(plt.style.available)

"""
['seaborn-dark', 'seaborn-darkgrid', 'seaborn-ticks', 'fivethirtyeight', 'seaborn-whitegrid', 'classic', '_classic_test', 'fast', 'seaborn-talk', 'seaborn-dark-palette', 'seaborn-bright', 'seaborn-pastel', 'grayscale', 'seaborn-notebook', 'ggplot', 'seaborn-colorblind', 'seaborn-muted', 'seaborn', 'Solarize_Light2', 'seaborn-paper', 'bmh', 'tableau-colorblind10', 'seaborn-white', 'dark_background', 'seaborn-poster', 'seaborn-deep']
"""
```

> 修改风格

```
x = np.linspace(0,1,1000)
plt.title('title')
plt.style.use('classic')  # 使用classic风格
plt.plot(x,x ** 2)
plt.plot(x,x)
plt.legend(['y=x^2','y=x'])
```

![修改风格](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142731564-11996618..png)

## 2.4、动态rc参数

pyplot模块使用rc配置文件来自定义图形的各种默认属性，称为rc配置或rc参数。通过修改rc参数可以修改默认的属性，包括窗体大小、每英寸的点数、线条宽度、颜色、样式、坐标轴、坐标和网络属性、文本、字体等。

 matplotlib将默认参数配置保存在matplotlibrc文件中，通过修改配置文件，可修改图标的的缺省样式。查看默认配置的方式如下：

1）直接打开matplotlibrc文件

2）`print(matplotlib.rc_params())`

3）`print(matplotlib.rcParamsDefault)`

4）`print(matplotlib.rcParams)`

### 1、线条常用的rc参数

管理线条属性的rc参数lines几乎可以控制线条的每一个细节。

> 线条的常用rc参数名称、解释与取值

| rc参数名称       | 解释           | 取值                                          |
| ---------------- | -------------- | --------------------------------------------- |
| lines.linewidth  | 线条宽度       | 取0~10之间的数值，默认为1.5                   |
| lines.linestyle  | 线条样式       | 可取“-”，“—”，“-.”，“:”4中，默认为“--”        |
| lines.marker     | 线条上点的形状 | 可取“o”,"D","h",".",",","S"等20种，默认为None |
| lines.markersize | 点的大小       | 取0～10数值，默认为1                          |

```
import matplotlib as mpl

fig = plt.figure(figsize = (10,10),dpi = 80)  # 创建画布。大小10x10，像素80
x = np.linspace(0,1,1000)  # 通过numpy生成随机数

# 绘制第一张子图
fig.add_subplot(2,2,1)  # 分为2x2图形阵，选择第一张图片绘图
plt.rcParams['lines.linestyle'] = '-.'  # 修改线条类型
plt.rcParams['lines.linewidth'] = 1  # 修改线条宽度
plt.plot(x,x**2)
plt.title('y=x^2')  # 添加标题

# 绘制第二张子图

fig.add_subplot(2,2,2)  # 分为2x2图形阵，选择第二张图片绘图
mpl.rc('lines',linestyle = '--', linewidth = 10)
plt.plot(x,x**2)
plt.title('y=x^2')  # 添加标题

# 绘制第三张子图

fig.add_subplot(2,2,3)  # 分为2x2图形阵，选择第三张图片绘图
plt.rcParams['lines.marker'] = None
plt.rcParams['lines.linewidth'] = 3
plt.plot(x,x**2)
plt.title('y=x^2')  # 添加标题

# 绘制第四张子图

fig.add_subplot(2,2,4)  # 分为2x2图形阵，选择第四张图片绘图
plt.rcParams['lines.linestyle'] = ':'
plt.rcParams['lines.linewidth'] = 6
plt.plot(x,x**2)
plt.title('y=x^2')  # 添加标题


​```
plt.savefig('修改线条的rc参数.png')
plt.show()
​```


```

![修改线条的rc参数](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142731717-88747395..png)

### 2、坐标轴常用的rc参数

同样，管理坐标轴属性的rc参数axes也能控制坐标轴的任意细节。

| rc参数名称                          | 解释       | 取值                                         |
| ----------------------------------- | ---------- | -------------------------------------------- |
| axas.facecolor                      | 背景颜色   | 接收颜色简写字符。默认为“W”                  |
| axas.edgecolor                      | 边线颜色   | 接收颜色简写字符。默认为“k”                  |
| axas.linewidth                      | 轴线宽度   | 接收0～1的float。默认为0.8                   |
| axas.grid                           | 添加网格   | 接收bool。默认为False                        |
| axas.titlesize                      | 标题大小   | 接收‘small’,‘medium’,'large'。默认为‘large’  |
| axas.labelsize                      | 轴标大小   | 接收‘small’,‘medium’,'large'。默认为‘medium’ |
| axas.lablelcolor                    | 轴标颜色   | 接收颜色简写字符。默认为“k”                  |
| axas.spines.{left,botton,top,tight} | 添加坐标轴 | 接收bool。默认为True                         |
| axas.{x,y}margin                    | 轴余留     | 接收float。默认为0.05                        |

原轴：

```
    
x = np.linspace(0,10,1000)
plt.plot(x, np.sin(x))
plt.show()
```



![原轴](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142731863-1822285392..png)

修改rc参数之后的轴：

```
x = np.linspace(0,10,1000)
plt.rcParams['axes.edgecolor'] = 'b'  # 轴颜色设置为蓝色
plt.rcParams['axes.grid'] = True  # 添加网格
plt.rcParams['axes.spines.top'] = False  # 去除顶部轴
plt.rcParams['axes.spines.right'] = False  # 去除右侧轴
plt.rcParams['axes.xmargin'] = 0.1  # x轴余留为区间长度的0.1倍
plt.plot(x, np.sin(x))
plt.show()
```



![修改rc](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142732048-1556286658..png)

### 3、字体常用的rc参数

 其实用到现在，可能有些同学已经发现，默认的pyplot字体，并不支持中文字符，因此需要通过修改`font.sans-
serif`参数来修改绘图时的字体，使得图形可以正常显示中文。同时由于修改字体后会导致坐标轴中负号无法正常显示，因此需要同时修改axes.uncode_minus参数。

| rc参数名称  | 解释                       | 取值             |
| ----------- | -------------------------- | ---------------- |
| font.family | 字体族，每一族对应多种字体 | 接收serif、sans- |

```
serif、cursive、fantasy、monospace五种。默认为sans-serif  
font.style | 字体风格 | 接收normal(roman)、italic、oblique三种，默认为normal  
font.variant | 字体变化 | 接收normal或small-caps。默认为normal  
font.widget | 字体重量 | 接收normal、bold、bolder、lighter四种及100、200、…、900.默认为nomal  
font.stretch | 字体延伸 |  
font.size | 字体大小 | 接收float。默认为10  

windows设置中文字体：


plt.rcParams['font.sans-serif'] = ['SimHei']  # 设置中文字体
plt.rcParams['axes.unicode_minus'] = False

mac设置中文字体：


plt.rcParams["font.family"] = 'Arial Unicode MS'
```



## 2.4、折线图

折线图是将"散点"按照横坐标顺序用线段依次连接起来的图形。以折线的上升或下降表示某一特征随另外一特征变化的增减以及总体变化趋势。一般用于展现某一特征随时间的变化趋势。

> plot函数常用参数及其说明

| 参数      | 说明                                        |
| --------- | ------------------------------------------- |
| x,y       | 分别表示x轴和y轴的数据。无默认值            |
| color     | 接收特定str，指定线条的颜色。默认为None     |
| linestyle | 接收特定str，指定线条类型。默认为 “-”       |
| marker    | 接收特定str，表示绘制的点的形状。默认为None |
| alpha     | 接收0～1的小说，表示点的透明度。默认为None  |

其中color参数的8种常用颜色的缩写。

| 颜色缩写 | 代表的颜色 |
| -------- | ---------- |
| b        | 蓝色       |
| g        | 绿色       |
| r        | 红色       |
| c        | 青色       |
| m        | 品红       |
| y        | 黄色       |
| k        | 黑色       |
| w        | 白色       |

```
plt.plot([0,3,9,15,30],linestyle = '-.',color = 'r',marker = 'o') 
```

![matplot2](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142733701-823933258..png)

> axis函数

```
plt.plot(y.cumsum())

plt.grid(True)
plt.axis('image')
```

运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142733890-1911881969..png)

接下来主要介绍axis函数的一些参数：

| 参数                  | 描述                           |
| --------------------- | ------------------------------ |
| Empty                 | 返回当前坐标轴限值             |
| off                   | 关闭坐标轴线和标签             |
| equal                 | 使用等刻度                     |
| scaled                | 通过尺寸变化平衡刻度           |
| tight                 | 使所有数据可见（缩小限值）     |
| image                 | 使所有数据可见（使用数据限值） |
| [xmin,xmax,ymin,ymax] | 将设置限制为给定的（一组）值   |

## 2.2、二维数据集

一维数据绘图只能说是一种特例，一般来说，数据集包含多个单独的子集。这些数据的处理也是同样遵循matplotlib处理一维数据时的原则。但是，这种情况会出现一些其他的问题，例如，两个数据集它们可能会有不同的刻度，无法用相同的y或者x轴刻度进行绘制，还有可能希望以不同的方式可视化两组不同的数据，例如，一组数据使用线图，另一组使用柱状图。

接下来，首先生成一个二维样本数据。

```python
    
np.random.seed(2000)
y = np.random.standard_normal((20,2)).cumsum(axis=0)

以上代码生成的是一个包含标准正态分布随机数的20*2的ndarray数组，如下：


array([[ 1.73673761,  1.89791391],
       [-0.37003581,  1.74900181],
       [ 0.21302575, -0.51023122],
       [ 0.35026529, -1.21144444],
       [-0.27051479, -1.6910642 ],
       [ 0.93922398, -2.76624806],
       [ 1.74614319, -3.05703153],
       [ 1.52519555, -3.22618757],
       [ 2.62602999, -3.14367705],
       [ 2.6216544 , -4.8662353 ],
       [ 3.67921082, -7.38414811],
       [ 1.7685707 , -6.07769276],
       [ 2.19296834, -6.54686084],
       [ 1.18689581, -7.46878388],
       [ 1.81330034, -7.11160718],
       [ 1.79458178, -6.89043591],
       [ 2.49318589, -6.05592589],
       [ 0.82754806, -8.95736573],
       [ 0.77890953, -9.00274406],
       [ 2.25424343, -9.51643749]])

将这样的二维数组传递给plot函数，他将自动把包含的数据解释为单独的数据集。


plt.figure(figsize=(7,4))
plt.plot(y,lw=1.5)
plt.plot(y,"rd")
plt.axis('tight')
```



![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142734115-1762365501..png)

像这种数据肯定就是看的一头乱麻，所以说我们需要将它进一步做一下注释，为了让我们能更好的理解图表。

```
plt.figure(figsize=(7,4))
# 分别为两条数据添加图例
plt.plot(y[:,0],lw=1.5,label='1st')  
plt.plot(y[:,1],lw=1.5,label='2nd')
plt.plot(y,"rd")
plt.grid(True)  # 网格设置
plt.legend(loc=0)  # 图例标签位置设置
plt.axis('tight')
plt.xlabel('index')
plt.ylabel('value')
plt.title('test1')
```

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142734311-1391205531..png)

通过刚才的操作我们也能够发现，虽然我们传进的是一个ndarray数组，但是它是一个二维数组，所以我们要想将数据全部展示出来就可以使用上面那种方式，但是上面的数据刻度都是相差无几的，如果说某一维的数据非常大，而另外一维的则都是一些小数据，那要怎么办呢。

首先先来看看会造成什么样的结果：

```python
y[:,0] = y[:,0] * 100
plt.figure(figsize=(7,4))
plt.plot(y[:,0],lw=1.5,label='1st')
plt.plot(y[:,1],lw=1.5,label='2nd')
plt.plot(y,"rd")
plt.grid(True)  # 网格设置
plt.legend(loc=0)  # 图例标签位置设置
plt.axis("tight")
plt.xlabel('index')
plt.ylabel('value')
plt.title("test2")
```



运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142734686-2034938745..png)

第一个数据由于数据量大，所以在这么大的刻度上依然可以将数据显示比较好辨认，而第二个数据就会因为这个原因看起来像一条直线，我们已经不能通过图像观察它的数据效果。

> 处理方式：

* 使用两个y轴（一左一右）
* 使用两个子图

首先先来看第一种方法：

```
fig,ax1 = plt.subplots()
# 第一组数据
plt.plot(y[:,0],lw=1.5,label='1st')
plt.plot(y[:,0],"rd")
plt.grid(True)  # 网格设置
plt.legend(loc=8)  # 图例标签位置设置
plt.axis("tight")
plt.xlabel('index')
plt.ylabel('value 1st')
# 第二组数据
ax2 = ax1.twinx()
plt.plot(y[:,1],'g',lw=1.5,label='2nd')
plt.plot(y[:,1],'bd')
plt.legend(loc=0)
plt.ylabel("value 2nd")

plt.title("test3")
```

运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142734924-2134655966..png)

这是通过在一张图上通过不同的刻度来展示不同的数据。

第二种方式：


    

```
plt.figure(figsize=(7,5))
plt.subplot(211)  # 指定子图位置，三个参数：行数、列数、子图编号
plt.plot(y[:,0],lw=1.5,label='1st')
plt.plot(y[:,0],"rd")
plt.grid(True)  # 网格设置
plt.legend(loc=0)  # 图例标签位置设置
plt.axis("tight")
plt.ylabel('value')

plt.title("test4")

plt.subplot(212)
plt.plot(y[:,1],'g',lw=1.5,label='2nd')
plt.plot(y[:,1],'rd')
plt.grid(True)  # 网格设置
plt.legend(loc=0)  # 图例标签位置设置
plt.axis("tight")
plt.xlabel('index')
plt.ylabel('value')
```

运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142735225-1623376418..png)

以上操作都是通过折线图来实现的，但是在matplotlib当中还支持很多种类型的图像。

## 2.3、plt对象支持的图类型

| 函数                               | 说明          |
| ---------------------------------- | ------------- |
| plt.plot(x,y,fmt)                  | 坐标系        |
| plt.boxplot(data,notch,position)   | 箱型图        |
| plt.bar(left,height,width,bottom)  | 柱状图        |
| plt.barh(width,bottom,left,height) | 横向柱状图    |
| plt.polar(theta,r)                 | 极坐标系      |
| plt.pie(data,explode)              | 饼图          |
| plt.psd(x,NFFT=256,pad_to,Fs)      | 功率谱密度图  |
| plt.specgram(x,NFFT=256,pad_to,F)  | 谱图          |
| plt.cohere(x,y,NFFT=256,Fs)        | X-Y相关性函数 |
| plt.scatter(x,y)                   | 散点图        |
| plt.step(x,y,where)                | 步阶图        |
| plt.hist(x,bins,normed)            | 直方图        |

### 2.3.1、柱状图

```
# 柱状图
data = [12,34,23,54]
labels = ['Jan','Fed','Mar','Apr']
plt.xticks([0,1,2,3],labels)  # 设置x轴刻度
plt.bar([0,1,2,3],data)    
```



![柱状图](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142735405-1130948976..png)

```
#横向柱状图
data = [12,34,23,54]
labels = ['Jan','Fed','Mar','Apr']
plt.yticks([0,1,2,3],labels)
plt.barh([0,1,2,3],data)   
```



![横向柱状图](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142735541-280933916..png)

​    
 

```
DataFrame数组图
df = pd.DataFrame({
'Jan':pd.Series([1,2,3],index=['a','b','c']),
'Fed':pd.Series([4,5,6],index=['b','a','c']),
'Mar':pd.Series([7,8,9],index=['b','a','c']),
'Apr':pd.Series([2,4,6],index=['b','a','c'])
})
    
df.plot.bar()  # 水平柱状图，将每一行中的值分组到并排的柱子中的一组
df.plot.barh(stacked=True,alpha=0.5)  # 横向柱状图，将每一行的值堆积到一起
```

![数组图](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142735698-622586338..png)

### 2.3.2、饼图

饼图用于表示不同类别的占比情况，通过弧度大小来对比各种类别。饼图将一个圆饼按照类别的占比划分成多个区块，整个圆饼代表数据的总量，每个区块表示该分类占总体的比例大小。饼图可以比较清楚地反映出部分与部分、部分与整体之间的比例关系，易于比例每个类别相对于总数的大小。但在对于面积大小的不敏感的情况下效果不是很好。

```
# 饼图
plt.pie([10,20,30,40],labels=list('abcd'),autopct="%.2f%%",explode=[0.1,0,0,0])  # 饼图
plt.axis("equal")
plt.show()
```



![饼图](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142738304-530017461..png)

### 2.3.3、散点图

对于二维绘图，线图和点图可能是金融学中的最重要的，刚才在上面线图已经有过简单接触，接下来主要介绍的就是点图了，这种图表类型可用于绘制一个金融时间序列的收益和另一个时间序列收益的对比。

散点图又称为散点分布图，是利用坐标点（散点）的分布形态反映特征间的相关关系的一种图形。实际中一般使用二维散点图，通过散点的疏密程度和变化趋势表示两个特征之间的关系。

主要有以下三个特点：

1）表现特征之间是否存在数值或者数量的关联趋势，关联趋势是线性的还是非线性的。

2）凸显出离群点（异常点）及其对整体的影响

3）数据量越大能发挥的作用越好。

基本语法格式：

```
    
    matplotlib.pyplot.scatter(x,y,s=None,c=None,marker=None,cmap=None,norm=None,vmin=None,
    vmax=None,alpha=None,linewidths=None,verts=None,edgecolors=None,*,data=None,**kwargs,
    )
```



函数参数相关说明：

| 参数名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| x,y      | 接收array，表示x轴和y轴对应的·数据。无默认值                 |
| s        | 接收数值或者一维array，指定点的大小，一维array表示每个点的大小。默认为None |
| c        | 接收颜色或者一堆array，指定点的颜色，一维array表示每个点的颜色。默认为None |
| marker   | 接收特定str，表示绘制的点的形状。默认为None                  |
| alpha    | 接收0～1的小数，表示点的透明度。默认为None                   |

```
y = np.random.standard_normal((1000,2))  # 生成正态分布的二维随机数组
c = np.random.randint(0,10,len(y))

plt.figure(figsize=(7,5))
plt.scatter(y[:,0],y[:,1],c=c,marker='o')  # 通过scatter函数加入第三维数据
plt.colorbar()  # 通过彩条对不用演示数据进行描述
plt.grid(True)
plt.xlabel('1st')
plt.ylabel('2nd')
plt.title("test5")
```

运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142739510-1286917536..png)

### 2.3.4、直方图

```
plt.figure(figsize=(7,4))
plt.hist(y,label=['1st','2nd'],bins=25)
plt.grid(True)  # 网格设置
plt.legend(loc=0)  # 图例标签位置设置
plt.axis("tight")
plt.xlabel('index')
plt.ylabel('frequency')
plt.title("test6")
```



运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142739831-1764744241..png)

直方图是金融应用当中比较常用的图表类型，接下来主要介绍一下plt.hist的使用方法以及它的参数说明

```
plt.hist(
['x', 'bins=None', 'range=None', 'density=None', 'weights=None', 'cumulative=False', 'bottom=None', "histtype='bar'", "align='mid'", "orientation='vertical'", 'rwidth=None', 'log=False', 'color=None', 'label=None', 'stacked=False', 'normed=None', '*', 'data=None', '**kwargs'],
)
```



| 参数        | 描述                                 |
| ----------- | ------------------------------------ |
| x           | 列表对象，ndarray对象                |
| bins        | 数据组（bin）数                      |
| range       | 数据组的上界和下界                   |
| normed      | 规范化为整数1                        |
| weights     | x轴上每个值的权重                    |
| cumulative  | 每个数据组包含较低组别的计数         |
| histtype    | 选项：bar,barstacked,step,stepfilled |
| align       | 选项：left,mid,right                 |
| orientation | 选项:horizontal,vertical             |
| rwidth      | 条块的相对宽度                       |
| log         | 对数刻度                             |
| color       | 每个数据集的颜色                     |
| label       | 标签所用的字符串或者字符串序列       |
| stacked     | 堆叠多个数据集                       |

### 2.3.5、箱型图

箱型图可以简洁地概述数据集的特性，可以很容易的比较多个数据集。

```
fig,ax = plt.subplots(figsize=(7,4))
plt.boxplot(y)
plt.grid(True)
plt.setp(ax,xticklabels=['1st','2nd'])
plt.xlabel('data set')
plt.ylabel("value")
plt.title("test7")
```



运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142739964-179468833..png)

### 2.3.6、补充：绘制数学函数

以图形的方式说明某个下限和上限之间函数图像下方区域的面积，简而言之就是，从下限到上限之间函数积分值


    

```
# 第一步：定义求取积分的函数
def func(x):
	return 0.5 * np.exp(x) + 1   # 指数函数
# 第二步：定义积分区间，生成必须得数值
a, b = 0.5 , 1.5
x = np.linspace(0,2)
y = func(x)

# 第三步：绘制函数图像
fig, ax = plt.subplots(figsize=(7,5))
plt.plot(x,y,'b',linewidth=2)
plt.ylim(ymin=0)

# 第四步：使用Polygon函数生成阴影部分，表示积分面积
Ix = np.linspace(a, b)
Iy = func(Ix)
verts = [(a,0)] + list(zip(Ix, Iy)) + [(b, 0)]
poly = plt.Polygon(verts,facecolor='0.7',edgecolor='0.5')
ax.add_patch(poly)

# 第五步：使用plt.text和plt.figtext在图表上添加数学公式和一些坐标轴标签
plt.text(0.5 * (a + b),1,r"$\int_a^b f(x)\mathrm{d}x$",horizontalalignment='center',fontsize=20)
plt.figtext(0.9, 0.075, "$x$")
plt.figtext(0.075,0.9,"$f(x)$")

# 第六步：设置刻度标签以及添加网格
ax.set_xticks((a, b))
ax.set_xticklabels(('$a$', '$b$'))
ax.set_yticks([func(a), func(b)])
ax.set_yticklabels(('$f(a)$', '$f(b)$'))
plt.grid(True)
```

运行结果：

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142740137-602187286..png)  

# 3、金融学图表

以上绘制出来的数据都是一些常用的数据图像，但是在金融行业会有一些独有的图像，之前在matplotlib当中还提供了少量的特殊金融图表，这些图表，就例如烛柱图，主要是用于可视化历史股价数据或者类似的金融时间序列。

现在这个方法已经独立出来自成一个模块了 _`mpl_finance`_

anaconda中mpl_finance安装方式：

将[https://github.com/matplotlib/mpl_finance/archive/master.zip](http://jump.bdimg.com/safecheck/index?url=rN3wPs8te/r8jfr8YhogjfUWFoMgIRa8DaWj5IFI7jB6TkQw42zce5RaIoYlO2yf2FD/58WVz7f/7MMdoKxjlhcy1SkdO1XJy3ST3QjZSi6e7pqerS8PFNTZZM1XyoDafm3alHHRoE0MyOc/xXFrwYQkmFrsLRsabLbzxSQM0Rs0OWbz6YBdTVX3ZMCahZXTMDxm7iZ2BjQ=)下载到本地

在anaconda环境中运行命令： _`pip install 本地路径/mpl_finance-master.zip`_

调用方式：

> import mpl_finance as mpf

```
import matplotlib.pyplot as plt
import mpl_finance as mpf
import tushare as ts
import pandas as pd
from matplotlib.pylab import date2num
from dateutil.parser import parse
import numpy as np
import matplotlib.dates as mdate

data = ts.get_k_data('000001')  # 获取平安的k线数据
data_of = data[:60]  # 只取前60份数据

fig, ax = plt.subplots(figsize=(15, 7))
__colorup__ = "r"
__colordown__ = "g"

# 图表显示中文
plt.rcParams['font.family'] = ['sans-serif']
plt.rcParams['font.sans-serif'] = ['SimHei']

qutotes = []
for index, (d, o, c, h, l) in enumerate(
        zip(data_of.date, data_of.open, data_of.close,
            data_of.high, data_of.low)):
    
    # 时间需要通过date2num转换为浮点型
    d = date2num(parse(d))
    # 日期，开盘，收盘，最高，最低组成tuple对象val
    val = (d, o, c, h, l)
    # 加val加入qutotes
    qutotes.append(val)

# 使用mpf.candlestick_ochl进行蜡烛绘制，ochl代表：open，close，high，low
mpf.candlestick_ochl(ax, qutotes, width=0.8, colorup=__colorup__,colordown=__colordown__)

#设置x轴为时间格式，否则x轴显示的将是类似于‘736268’这样的转码后的数字格式
ax.xaxis.set_major_formatter(mdate.DateFormatter('%Y-%m-%d'))

plt.xticks(pd.date_range('2016-08-01','2016-11-30',freq='W'),rotation=60)
plt.grid(True)  # 网格设置
plt.title("k线图")
ax.autoscale_view()
ax.xaxis_date()
```

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142744820-1182269713..png)

# 4、保存图表到文件

> plt.savafig('文件名.拓展名')

文件类型是通过文件扩展名推断出来的。因此，如果你使用的是.pdf，就会得到一个PDF文件。

```
    
plt.savefig('123.pdf')

savefig并非一定要写入磁盘，也可以写入任何文件型的对象，比如BytesIO：


from io import BytesIO
buffer = BytesIO()
plt.savefig(buffer)
plot_data = buffer.getvalue()
```



| 参数                 | 说明                                                         |      |
| -------------------- | ------------------------------------------------------------ | ---- |
| fname                | 含有文件路径的字符串或者Python的文件型对象。                 |      |
| dpi                  | 图像分辨率，默认为100                                        |      |
| format               | 显示设置文件格式("png","jpg","pdf","svg","ps",...)           |      |
| facecolor、edgecolor | 背景色，默认为"W"(白色)                                      |      |
| bbox_inches          | 图表需要保存的部分。设置为”tight“，则尝试剪除图表周围空白部分 |      |

* [Bokeh](http://link.zhihu.com/?target=https%3A//github.com/bokeh/bokeh) \- Python的交互式网络绘图.
* [Seaborn](http://link.zhihu.com/?target=https%3A//github.com/mwaskom/seaborn) \- 使用Matplotlib的统计数据可视化.

