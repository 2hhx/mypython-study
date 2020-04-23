### 1. Matplotlib简介

  Matplotlib是Python的一个2D图形库，能够生成各种格式的图形（诸如折线图，散点图，直方图等等），界面可交互（可以利用鼠标对生成图形进行点击操作），同时该2D图形库跨平台，即既可以在Python脚本中编码操作，也可以在Jupyter Notebook中使用，以及其他平台都可以很方便的使用Matplotlib图形库，而且生成图形质量较高，甚至可以达到出版级别。需要注意的是，在相关Python软件中调用Matplotlib图形库时，需要利用shell进行单独安装，假如使用Jupyter Notebook时，相关图形库已直接配置在软件内，不过其生成的图形无法进行交互，而是内嵌在Jupyter Notebook生成界面内。

  针对Matplotlib的官方说明如下所示：

> ***Matplotlib is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. Matplotlib can be used in Python scripts, the Python and IPython shells, the Jupyter notebook, web application servers, and four graphical user interface toolkits.***

### 2. Matplotlib操作简介

#### 2.1 Matplotlib示例

  在Python中使用Matplotlib时，常用到Matplotlib模块和Numpy模块，下面以一个简单的线性图形的例子进行说明：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzumhor9doj20hs0dcglr.jpg)图 1

  通过上述生成如下一次函数图形，首先对上述代码进行简要解说：

* 首先绘图需要导入matplotlib.pyplot，其中pyplot是matplotlib的绘图框架，功能类似于于MATLAB的绘图功能，图形绘制需要利用到pyplot，即 ***plt.plot()***和***plt.show()***；
* 程序通过Numpy生成绘图所需数据，Numpy是Python的一个数据处理包，极大的方便了Python在科学计算方面的应用，在程序中通过使用Numpy内置 ***linspace()***生成区间在[-3,3]的数组。

------

  假如需要图像上具有多个函数图形，可进行如下操作：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
y_2 = x**2

# plt.figure(num=1,figsize=(8,5))
plt.plot(x,y_1)
plt.plot(x,y_2)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzvvpbrohuj20m80dwmxm.jpg)图 2

  如上述代码所示，只需在上述两个函数后进行两次图像绘制即可实现复合图像的生成。

#### 2.2 Matplotlib参数设置

  前面通过简单示例对Matplotlib进行初步的讲解，但仔细观察后，图1仍缺失很多绘图属性元素，下面将对Matplotlib中的绘图属性进行简要介绍：

##### 2.2.1 轴线设置

###### （1）轴线范围及标签设置

  首先进行坐标轴设置的解读，主要涉及**坐标范围**，**数值**，**刻度修改**和**坐标轴显示**等多个方面。

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)

# 坐标轴设置
# 坐标轴范围
plt.xlim(-1,2)    
plt.ylim(-2,3)
# 坐标轴标签
plt.xlabel('X Axis')   
plt.ylabel('Y Axis')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzumiwxcaij20hs0dcwen.jpg)图 3

  通过对比图1和图3可以发现，相较于图1，图二刻度值范围改变，而且在坐标轴底侧和左侧分别增加了*X Axis* 和 *Y Axis* 两个轴标签。此处用到几个方法：

> 坐标轴范围设置： **xlim()** 和 **ylim()**；
>
> 坐标轴标签设置：**xlabel()** 和 **ylabel()**；

------

###### （2）轴线刻度值修改

  上述进行坐标轴区间设置和标签的添加，有时候我们还会遇到这样两种情况：

* 由于坐标轴的单位刻度不当，导致图形不美观或者信息缺失；
* 需要对坐标轴上某个值进行修改或替换；

  针对上述第一种情况，下面通过相应的函数来处理：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)

# 坐标轴设置
# 坐标轴刻度修改
new_ticks = np.linspace(-1,2,5)
plt.xticks(new_ticks)
plt.yticks([-2,-1.5,0,1,2])
# 坐标轴标签
plt.xlabel('X Axis')   
plt.ylabel('Y Axis')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzumk4stmrj20hs0dcjrm.jpg)图 4

  例子中使用到以下两个方法：

> 坐标轴刻度值修改：**xticks()** 和 **yticks()**；

  通过将替换刻度数据键入上述两个方法中，然后进行显示（只有***plt.show()***之后才能显示修改），原刻度值即可被修改。

------

###### （3）轴线刻度值替换

  针对上述的第二种情况，下述代码将给出详细的讲解：

```python
import matplotlib.pyplot as plt
import numpy as np

# 坐标轴设置
# 坐标轴刻度值替换
# 示图1（见图5）
x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)
new_ticks = np.linspace(-1,2,5)
plt.xticks(new_ticks)
plt.yticks([-2,-1.5,0,1,2],['bad','not bad','normal','good','best'])

plt.show()

# 示图2（见图6）
x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)
new_ticks = np.linspace(-1,2,5)
plt.xticks(new_ticks)
plt.yticks([-2,-1.5,0,1,2],[r'$bad$',r'$not\ bad$',r'$normal$',r'$good$',r'$best$'])

plt.show()

# 示图3（见图7）
x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)
new_ticks = np.linspace(-1,2,5)
plt.xticks(new_ticks)
plt.yticks([-2,-1.5,0,1,2],[r'$alpha$',r'$\beta$',r'$\theta$',r'$\gamma$',r'$\phi$'])
# 坐标轴标签
plt.xlabel('X Axis')
plt.ylabel('Y Axis')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzum9ckbjgj20hs0dcaaa.jpg)图 5![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzum9cgnplj20hs0dcweq.jpg)图 6![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzum9csvy9j20hs0dc74i.jpg)图 7

  在上述代码中，坐标值替换发生在y轴，首先能够确定的事，进行替换的字符需要与相应的坐标值一一对应，这是首要的原则，接下来对不同的字符替换形式进行了简单的尝试：

* 第一种只进行字符的对应，没有进行其他符号处理（即未添加$处理)，结果显示替换字符为一般的字体；
* 第二种对替换字符进行了数学符号的处理（即添加$进行标注），结果显示为斜体，同时需要输入空格时，需要在空格前添加反斜杠 **\ **；
* 第三种进行希腊字母的输出，规则是在希腊字母英语读法前添加反斜杠 **\ **，假如不添加，则会输出第二种字母斜体。

  在代码中，在处理字符前还添加有***r*** ，它表示正则化，假如不添加在上述例子中也不影响，至于具体作用，在此不过多介绍，感兴趣的人可以自查资料。

------

###### （4）轴线隐去设置

  在实际应用我们还会遇到这两种图形：

* 图像只需要坐标轴xy所在轴的框线；
* 图形贯穿多个象限，即此时在图像上需要显示多个象限，换而言之就是需要挪动坐标轴位置；

  下面首先针对第一个问题进行简要介绍：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)

# 去除多余坐标轴
# gca = 'get current axis'
ax = plt.gca()
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzunjvdy10j20hs0dcaa7.jpg)图 8

  去除不需要的坐标轴不是真的将其“删除”，而是将其变为无色，使之与底图背景色相同。在该处理过程中，主要使用到了以下几种方法：

> 激活当前坐标轴：***gca()***
>
> 轴线选取选项：***spine()***，选项有(***top***、***bottom***、***left***、***right***)
>
> 颜色设置选项：***set_color()***，当为none时，表示为无色

------

###### （5）轴线移动设置

  经过坐标轴线的去除处理，接下来移动坐标轴就很方便：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)

# 去除多余坐标轴
# gca = 'get current axis'
ax = plt.gca()
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
# 选定要移动的坐标轴
ax.xaxis.set_ticks_position('bottom')
ax.yaxis.set_ticks_position('left')
# 进行坐标轴的移动-data
ax.spines['bottom'].set_position(('data',0))
# ax.spines['left'].set_position(('data',0))

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzvqm7j23qj20hs0dcaa7.jpg)图 9![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzuo2wet7kj20hs0dcaa7.jpg)图 10

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
plt.plot(x,y_1)

# 去除多余坐标轴
# gca = 'get current axis'
ax = plt.gca()
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
# 选定要移动的坐标轴
ax.xaxis.set_ticks_position('bottom')
ax.yaxis.set_ticks_position('left')
# 进行坐标轴的移动-axes
ax.spines['bottom'].set_position(('axes',0.5))
# ax.spines['left'].set_position(('axes',0.5))

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzvqst2w3ej20hs0dc74f.jpg)图 11![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzvqtqse6sj20hs0dc74f.jpg)图 12

  在上述代码中，移动坐标轴需要经过以下几个步骤：

  **隐去非坐标轴框线** —> **确定待移动的坐标轴** —> **移动坐标轴**

  第一步在图7中已经进行了处理，在第二步和第三步中用到了一下几个方法：

> 将“bottom”作为x轴移动：**ax.xaxis.set_ticks_position('bottom')**
>
> 移动作为x轴的轴线“bottom”：**ax.spines['bottom'].set_position(('data',0))**

  此处需要强调下***set_position()***方法，该方法中具有三个参数，即***data***，***outward*** 和***axes***。

* 针对***data*** ，对比图9和图10，当进行bottom或者left轴设置时，处理效果为移动相应坐标轴到坐标刻度为0的位置。
* 针对***axes***，首先参数axes的取值范围为0~1，对比图11和图12，axes的移动规则与***data***相同，但是axes所取小数为整个坐标轴的比例位置，从负数方向向正数方向增加。由于坐标轴中点位置并非原点位置，所以出现了图10和图11所示现象。
* 针对***outward***，此处由于自己理解不到位，所以此处不在进行赘述，但能说道的一点就是系统默认outward=0。
* 另外为了方便书写，Matplotlib定义了一些简写符号：***'center' -> ('axes',0.5)*** 和 ***'zero' -> ('data', 0.0)***

------

###### （6）主次坐标轴

  我们在作图时还会遇到一些**共轴图像**，即图像上具有多个函数图像，函数图像横坐标值数据相同，但是纵坐标值不同，此处进行次坐标轴***(secondary axis)*** 的简单讲述：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0,10,.1)
y_1 = 0.05*x**2
y_2 = -1*y_1

fig,ax_1 = plt.subplots()
ax_2 = ax_1.twinx()
ax_1.plot(x,y_1,'g-')
ax_2.plot(x,y_2,'b--')

ax_1.set_xlabel('X data')
ax_1.set_ylabel('Y_1',color='g')
ax_2.set_ylabel('Y_2',color='b')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzvw6hk4wfj20hs0dcgm0.jpg)图 13

  如上图和代码所示，Y_1图像用 *left* 轴,Y_2图像使用 *right* 轴，整个程序中除 ***figure*** 和 ***twinx()*** 两种方法外，其余代码在前面已讲解，***figure*** 将在图像一节中进行详细讲解，此处主要对方法 ***twinx()*** 进行简要的说明：

* 代码 ***ax_2 = ax_1.twinx()*** 中，***twinx()*** 的作用主要是将坐标轴ax_1“镜像”给ax_2，此处“镜像”不仅仅是坐标轴线，而是连同坐标轴的特性一并“镜像”过来。

##### 2.2.2 图例设置

  图例是图像中必不可少的元素，接下来将对图像标签的设置进行简单的讲解，在讲解之前首先进行**复合图像**，即同一图中具有多条函数曲线的讲解：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0,10,.1)
y_1 = 0.05*x**2
y_2 = -1*y_1
# 第一种表示
plt.plot(x,y_1,'g-',label='y_1')
plt.plot(x,y_2,'b--',label='y_2')
plt.legend(loc='best')

# 第二种表示
# l_1, = plt.plot(x,y_1,'g-')
# l_2, = plt.plot(x,y_2,'b--')
# plt.legend(handles=[l_1,l_2],labels=['y_1','y_2'],loc='best')

plt.xlabel('X data')
plt.ylabel('Y data')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzwg4p9cggj20hs0dc0t0.jpg)图 14

  观察图14，图中标签被放置在了左上方，标签设置过程中主要使用了两种表示形式，首先第一种涉及到了以下几个方法：

* ***plt.plot(x,y_1,'g-',label='y_1')*** ，通过在 ***plot*** 中设置 ***label*** 显示标签名称；
* ***plt.legend(loc='best')***，通过方法 ***legend*** 进行标签位置设置，代码中使用了***best*** ，即系统自动将标签放置在空白较多的区域，位置设置主要有以下几个：***best、upper right、upper left、lower left、lower right、right、center left、center right、lower center、 upper center、center***。

  第二种主要涉及到了一下几个方法：

* ***handles*** 和 ***label***，如代码所示，在使用***handles***前先要进行相应的函数定义，按照定义方法，一般在定义名称后加上英文输入下的逗号，即***l_1,*** 查找相关资料解释说：

  > 当要将 plot 函数 返回的结果保存下来，由于 plot 函数返回的是一个列表，所以需要添加逗号，或者在 plot 函数末尾添加 [0]。

  但是并没有真正的理解，仅供大家参考，同时在使用***handles***时，需要进行使用**[ ]**，同理，***label*** 进行标签名称设置时，也需要使用 **[ ]** 表示。

  上面只是进行Legend的基本使用进行简述，Legend还有其他相应的处理方法，假如感兴趣，个人可以进一步研究，这里给出一个网上的详细讲解，方便大家学习，[*Legend Method*](http://blog.sina.com.cn/s/blog_b09d460201019c10.html)。

##### 2.2.3 标注设置

  在绘图过程中，我们有时会对图像中的关键点或信息进行标注以示强调，接下来就对标注的具体过程进行简要讲述：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3,50)
y = 2*x + 1
plt.plot(x,y)

ax = plt.gca()
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
ax.xaxis.set_ticks_position('bottom')
ax.spines['bottom'].set_position(('data',0))
ax.yaxis.set_ticks_position('left')
ax.spines['left'].set_position(('data',0))

# 特征点标注
x_0 = 1
y_0 = 2*x_0 + 1
# scatter是点画线，即上述的plt.plot()可以写成plt.scatter()，则会形成点划线
plt.scatter(x_0,y_0)
plt.plot([x_0,x_0],[y_0,0],'r--',lw=2.5)  # 设置特征点到x轴的引线

# 关键点信息标注
# method_1
plt.annotate('$2x_0+1=%s$' %y_0,xy=(x_0,y_0),xycoords='data',xytext=(+30,-30),anncoords='offset points',fontsize=16,arrowprops=dict(arrowstyle='->',connectionstyle='arc3,rad=.2'))
# method_2
# plt.text(-3.7,3,r'$This\ is\ the\ some\ text.\ \mu\ \sigma_i\ \alpha_t$',fontdict={'size':14,'color':'r'})

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzwir62kbzj20m80dw3yv.jpg)图 15

  代码前半部分主要讲述的是坐标轴的移动，从注释开始进入特征点部分的书写，观察图15，图像标注主要分为两部分：

* 关键点标注：如图15右侧，关键点的标注主要分为四部分，（1）关键点绘制；（2）关键点对齐线；（3）关键点公式和引线。

> （1）关键点绘制：代码中选取x=1处的坐标点；在突出显示确定点时使用如下代码 ***plt.scatter(x_0,y_0)***，即将确定点绘制成散点模式（***scatter()*** 是绘制散点图的方法）。
>
> （2）关键点对齐线：***plt.plot([x_0,x_0],[y_0,0],'r--',lw=2.5)***， # 设置特征点到x轴的引线，其中涉及到了线型的设置，即***('r--',lw=2.5)***，线型将在2.2.4节进行讲述，此处只需要知道在代码中，线型被设置为红色虚线绘制，线宽2.5；关于对齐线的画法，代码使用***plot()*** 的方法，根据列表区间绘制线条，即引线横坐标区间为x=1，纵坐标区间为[0,y_0]。
>
> （3）关键点公式和引线：代码中使用***annotate*** 的方法，首先'2x_0+1=3' 的绘制方法同2.2.1节中所提到的利用***$*** 进行数学斜体的绘制方法，其次为标注信息的绘制，即首先确定绘制点，即 ***xy=(x_0,y_0)***，然后定位传递数据的坐标位置，即***xycoords='data'***，然后确定标注式子的放置位置，即利用 ***xytext=(+30,-30),anncoords='offset points',fontsize=16*** 将标注公式放置在偏移关键点(x+30,y-30)的位置，然后利用***arrowprops=dict(arrowstyle='->',connectionstyle='arc3,rad=.2'))*** 进行引线设置，首先设置引线的样式，代码中使用的是**—>**，而且引线呈弓线状，即通过***connectionstyle***进行设置，弓线弯曲半径0.2，即使用***rad*** 进行设置。
>
> 上例只是进行简单讲解，其实包括引线形式，格式等还有很多，此处贴下两篇博客供大家参考：[Annotation_1](https://www.cnblogs.com/flyfat/p/10288280.html)、[Annotation_2](https://www.jianshu.com/p/1411c51194de)。

* 关键信息标注：设置方法与关键点的设置类似，此处通过另外使用***fontdict*** 方法进行标注信息的格式设置，代码进行了标准信息大小和颜色的设置，即***size()、color()***。
* 基于官方消息，xytext()和textcoords()在将来经不再提供支持，开始支持xyann()和anncoords()。

##### 2.2.4 图像设置

  在图形绘制中，我们经常会涉及到**图像分栏**以及**图中图**等问题，接下来将对图像的上述问题进行一一解读。

------

###### （1）图像尺寸及线型设置

  首先讲述下最简单的问题，即对图像的一些简单设置进行简要讲解，主要涉及图像大小设置和线型的设置：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3.50)
y_1 = 2*x+1
y_2 = x**2

plt.figure(num=1,figsize=(8,5))
plt.plot(x,y_1,color='r',linewidth=1.5,linestyle='--',marker='+')
plt.plot(x,y_2,color='g',linewidth=1.5,linestyle='-',marker='<')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzxn9zi6x8j20m80dw74s.jpg)图 16

  上述代码关于图像设置主要涉及两部分内容，即图像编号及大小设置和图像线性的设置：

* 图像编号及大小的设置：***plt.figure(num=1,figsize=(8,5))*** 图像中设置图像需要用到***figure()*** 方法，如需设置绘图编号，可以利用参数 ***num*** 进行指定，未指定时，系统默认首图为1，利用 ***figsize*** 可以进行图像大小的设置，设置对象为整个轴线框的尺寸；
* 图像线型设置：利用 ***plt.plot(x,y_1,color='r',linewidth=1.5,linestyle='--',marker='+')*** 将直线渲染成红色，线宽1.5，线型为虚线，标记为‘+’ 的状态。

> 针对颜色设置，可以写成以下几种形式：
>
> > (a) 使用颜色全称代号，即***color='r'*** 或者***color='red'***；
> >
> > (b) 使用颜色16进制，***color='#FF0000’***；
> >
> > (c) 使用颜色RGB或RGBA元组，即 ***color=(1,0,0)*** 或 ***color=(1,0,0,1)***，前者为RGB模式，后者为RGBA模式，两者前三个数字相同，RGBA模式的最后一个数字表示颜色的透明度，范围是0~1。另外需要注意的是，Matplotlib中的RGB格式和常见的RGB格式不太相同，Matplotlib中的RGB数值均为 0~1，而我们常见的RGB格式为 0~255；
> >
> > (d) 使用灰度强度进行表示：即 ***color=0.5***，关于灰度值的具体定义和计算，可以参照相关资料，此处给出百度百科解释：[***Gray***](https://baike.baidu.com/item/灰度值/10259111?fr=aladdin)。
> >
> > (e) 针对常用颜色表示，此处给出图例以示参考：
> >
> > ```css
> >   'aliceblue':                   '#F0F8FF'
> >   'antiquewhite':                '#FAEBD7'
> >   'aqua':                        '#00FFFF'
> >   'aquamarine':                  '#7FFFD4'
> >   'azure':                       '#F0FFFF'
> >   'beige':                       '#F5F5DC'
> >   'bisque':                      '#FFE4C4'
> >   'black':                       '#000000'
> >   'blanchedalmond':              '#FFEBCD'
> >   'blue':                        '#0000FF'
> >   'blueviolet':                  '#8A2BE2'
> >   'brown':                       '#A52A2A'
> >   'burlywood':                   '#DEB887'
> >   'cadetblue':                   '#5F9EA0'
> >   'chartreuse':                  '#7FFF00'
> >   'chocolate':                   '#D2691E'
> >   'coral':                       '#FF7F50'
> >   'cornflowerblue':              '#6495ED'
> >   'cornsilk':                    '#FFF8DC'
> >   'crimson':                     '#DC143C'
> >   'cyan':                        '#00FFFF'
> >   'darkblue':                    '#00008B'
> >   'darkcyan':                    '#008B8B'
> >   'darkgoldenrod':               '#B8860B'
> >   'darkgray':                    '#A9A9A9'
> >   'darkgreen':                   '#006400'
> >   'darkkhaki':                   '#BDB76B'
> >   'darkmagenta':                 '#8B008B'
> >   'darkolivegreen':              '#556B2F'
> >   'darkorange':                  '#FF8C00'
> >   'darkorchid':                  '#9932CC'
> >   'darkred':                     '#8B0000'
> >   'darksalmon':                  '#E9967A'
> >   'darkseagreen':                '#8FBC8F'
> >   'darkslateblue':               '#483D8B'
> >   'darkslategray':               '#2F4F4F'
> >   'darkturquoise':               '#00CED1'
> >   'darkviolet':                  '#9400D3'
> >   'deeppink':                    '#FF1493'
> >   'deepskyblue':                 '#00BFFF'
> >   'dimgray':                     '#696969'
> >   'dodgerblue':                  '#1E90FF'
> >   'firebrick':                   '#B22222'
> >   'floralwhite':                 '#FFFAF0'
> >   'forestgreen':                 '#228B22'
> >   'fuchsia':                     '#FF00FF'
> >   'gainsboro':                   '#DCDCDC'
> >   'ghostwhite':                  '#F8F8FF'
> >   'gold':                        '#FFD700'
> >   'goldenrod':                   '#DAA520'
> >   'gray':                        '#808080'
> >   'green':                       '#008000'
> >   'greenyellow':                 '#ADFF2F'
> >   'honeydew':                    '#F0FFF0'
> >   'hotpink':                     '#FF69B4'
> >   'indianred':                   '#CD5C5C'
> >   'indigo':                      '#4B0082'
> >   'ivory':                       '#FFFFF0'
> >   'khaki':                       '#F0E68C'
> >   'lavender':                    '#E6E6FA'
> >   'lavenderblush':               '#FFF0F5'
> >   'lawngreen':                   '#7CFC00'
> >   'lemonchiffon':                '#FFFACD'
> >   'lightblue':                   '#ADD8E6'
> >   'lightcoral':                  '#F08080'
> >   'lightcyan':                   '#E0FFFF'
> >   'lightgoldenrodyellow':        '#FAFAD2'
> >   'lightgreen':                  '#90EE90'
> >   'lightgray':                   '#D3D3D3'
> >   'lightpink':                   '#FFB6C1'
> >   'lightsalmon':                 '#FFA07A'
> >   'lightseagreen':               '#20B2AA'
> >   'lightskyblue':                '#87CEFA'
> >   'lightslategray':              '#778899'
> >   'lightsteelblue':              '#B0C4DE'
> >   'lightyellow':                 '#FFFFE0'
> >   'lime':                        '#00FF00'
> >   'limegreen':                   '#32CD32'
> >   'linen':                       '#FAF0E6'
> >   'magenta':                     '#FF00FF'
> >   'maroon':                      '#800000'
> >   'mediumaquamarine':            '#66CDAA'
> >   'mediumblue':                  '#0000CD'
> >   'mediumorchid':                '#BA55D3'
> >   'mediumpurple':                '#9370DB'
> >   'mediumseagreen':              '#3CB371'
> >   'mediumslateblue':             '#7B68EE'
> >   'mediumspringgreen':           '#00FA9A'
> >   'mediumturquoise':             '#48D1CC'
> >   'mediumvioletred':             '#C71585'
> >   'midnightblue':                '#191970'
> >   'mintcream':                   '#F5FFFA'
> >   'mistyrose':                   '#FFE4E1'
> >   'moccasin':                    '#FFE4B5'
> >   'navajowhite':                 '#FFDEAD'
> >   'navy':                        '#000080'
> >   'oldlace':                     '#FDF5E6'
> >   'olive':                       '#808000'
> >   'olivedrab':                   '#6B8E23'
> >   'orange':                      '#FFA500'
> >   'orangered':                   '#FF4500'
> >   'orchid':                      '#DA70D6'
> >   'palegoldenrod':               '#EEE8AA'
> >   'palegreen':                   '#98FB98'
> >   'paleturquoise':               '#AFEEEE'
> >   'palevioletred':               '#DB7093'
> >   'papayawhip':                  '#FFEFD5'
> >   'peachpuff':                   '#FFDAB9'
> >   'peru':                        '#CD853F'
> >   'pink':                        '#FFC0CB'
> >   'plum':                        '#DDA0DD'
> >   'powderblue':                  '#B0E0E6'
> >   'purple':                      '#800080'
> >   'red':                         '#FF0000'
> >   'rosybrown':                   '#BC8F8F'
> >   'royalblue':                   '#4169E1'
> >   'saddlebrown':                 '#8B4513'
> >   'salmon':                      '#FA8072'
> >   'sandybrown':                  '#FAA460'
> >   'seagreen':                    '#2E8B57'
> >   'seashell':                    '#FFF5EE'
> >   'sienna':                      '#A0522D'
> >   'silver':                      '#C0C0C0'
> >   'skyblue':                     '#87CEEB'
> >   'slateblue':                   '#6A5ACD'
> >   'slategray':                   '#708090'
> >   'snow':                        '#FFFAFA'
> >   'springgreen':                 '#00FF7F'
> >   'steelblue':                   '#4682B4'
> >   'tan':                         '#D2B48C'
> >   'teal':                        '#008080'
> >   'thistle':                     '#D8BFD8'
> >   'tomato':                      '#FF6347'
> >   'turquoise':                   '#40E0D0'
> >   'violet':                      '#EE82EE'
> >   'wheat':                       '#F5DEB3'
> >   'white':                       '#FFFFFF'
> >   'whitesmoke':                  '#F5F5F5'
> >   'yellow':                      '#FFFF00'
> >   'yellowgreen':                 '#9ACD32'
> > ```
>
> 针对线型设置，主要有以下几种：
>
> > |    -    |      实线      |
> > | :-----: | :------------: |
> > | **- -** |    **短线**    |
> > | **-.**  | **短点相间线** |
> > |  **:**  |   **虚点线**   |
>
> 针对标记的设置如下所示：
>
> > ```css
> > 		  '.'             point marker
> > 		  ','             pixel marker
> > 		  'o'             circle marker
> > 		  'v'             triangle_down marker
> > 		  '^'             triangle_up marker
> > 		  '<'             triangle_left marker
> > 		  '>'             triangle_right marker
> > 		  '1'             tri_down marker
> > 		  '2'             tri_up marker
> > 		  '3'             tri_left marker
> > 		  '4'             tri_right marker
> > 		  's'             square marker
> > 		  'p'             pentagon marker
> > 		  '*'             star marker
> > 		  'h'             hexagon1 marker
> > 		  'H'             hexagon2 marker
> > 		  '+'             plus marker
> > 		  'x'             x marker
> > 		  'D'             diamond marker
> > 		  'd'             thin_diamond marker
> > 		  '|'             vline marker
> > 		  '_'             hline marker
> > ```

------

###### （2）图像透明度设置

  在实际绘图中，由于某些原因造成所绘图像的坐标轴信息（比如坐标值）被图像所覆盖，导致图像质量下降，我们可以通过设置图像透明度的问题来处理这种问题，接下来将对 **图像透明度**进行简要讲解：

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3,3,50)
y = 0.1*x

fig = plt.figure()
plt.plot(x,y,linewidth=10,alpha=0.8)
plt.ylim(-2,2)
ax = plt.gca()
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
ax.xaxis.set_ticks_position('bottom')
ax.spines['bottom'].set_position(('data',0))
ax.yaxis.set_ticks_position('left')
ax.spines['left'].set_position(('data',0))

for label in ax.get_xticklabels() + ax.get_yticklabels():
    # 设置字体
    label.set_family('monospace')
    label.set_fontsize(12)
    # alpha是调整透明度
    label.set_bbox(dict(facecolor='grey',edgecolor='None',alpha=0.5))

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzxzhh0i41j20hs0dcwek.jpg)图 17

  如代码所示，设置主要涉及以下几个方面：

* 首先利用 ***plt.plot(x,y,linewidth=10,alpha=0.8)*** 进行图像透明度的设置，其中设置透明度所用关键参数为 ***alpha***，该值的范围为0~1，数值越大，透明度越低。

* 其次涉及坐标轴数值的设置：

  ```python
  for label in ax.get_xticklabels() + ax.get_yticklabels():
      # 设置字体
      label.set_family('monospace')
      label.set_fontsize(12)
      # alpha是调整透明度
      label.set_bbox(dict(facecolor='grey',edgecolor='None',alpha=0.5))
  ```

  如上述代码所示，首先利用 ***get_xticklabels()*** 和 ***get_yticklabels()*** 获得坐标轴数据，然后对坐标轴数据进行字体设置，主要用到 ***set_family()*** 和 ***set_fontsize()***，前者设置字体格式，后者设置字体大小。最后利用***set_bbox()***对坐标轴数据绘制背景框，边框内部颜色设置- ***facecolor()*** 和边框框线设置 ***edgecolor()***，需要说明的是，由于透明度设置不是很合理，所以图17效果并不是很理想，但原理大致如上所讲述。

------

###### （3）图像分栏设置

  接下来对 **图像分栏**，即同一图中具有多个分格图像进行简要讲解：

```python
import matplotlib.pyplot as plt

# 子图均分
plt.subplot(2,2,1)  # 2行2列
plt.plot([0,1],[0,1])

plt.subplot(222)  # 可以去掉逗号，但是建议加上逗号
plt.plot([0,1],[0,1])

plt.subplot(223)
plt.plot([0,1],[0,1])

plt.subplot(224)
plt.plot([0,1],[0,1])

plt.show()
################################################
# 子图不等均分
plt.subplot(2,1,1)  # 2行1列
plt.plot([0,1],[0,1])

plt.subplot(234)  # 按照索引值进行排列
plt.plot([0,1],[0,1])

plt.subplot(235)
plt.plot([0,1],[0,1])

plt.subplot(236)
plt.plot([0,1],[0,1])

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzy06mtmzvj20hs0dcaal.jpg)图 18![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzy06n08jfj20hs0dc3z0.jpg)图 19

  图18和图19主要涉及图像分栏显示，主要涉及 ***subplot()*** 方法的使用，同时 ***subplot(a,b,c)*** 方法中*a*和*b*分别表示将图像界面分为*a*行*b*列，而*c*表示将图像放置在第几个位置（按照行列号索引值进行排序），另外需要注意的是在数量不多情况下（*abc* 为10以内数字时）可以省略 *abc* 之间的逗号，当数量较多时可能会引发错误。下面在对图像分栏显示的不同形式简要概述：

* 均匀分栏时，即图18所示形式，确定好行列数后，*c* 范围为*[1,a*b]*，同时先排行在排列；
* 不均匀分栏时，即图19所示形式，在代码中第一行使用 ***plt.subplot(2,1,1)*** 表示第一行第一列(首先将整个图像区域分成了2行，该代码占据了第一行)，由于是在后面出现了***plt.subplot(2,3,4)***，这意味着将第二行划分为3等分，但是计数从整个图像开始计算，即*(2,3)*，同时需要注意的是，由于从整体开始计数，所以第二行第一个列计数从第4个开始。

  另外针对不均匀图像分栏，上述方法使用不是很方便，此处再进行三种方法的详细介绍：

```python
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec

# method 1:subplot2grid
ax_1 = plt.subplot2grid((3,3),(0,0),colspan=3,rowspan=1)
ax_1.plot([1,2],[1,2])
ax_1.set_title('ax_1_title')
# (3,3)表示三行三列,(0,0)表示从0行0列开始，其中0,0是index
ax_2 = plt.subplot2grid((3,3),(1,0),colspan=2)
ax_3 = plt.subplot2grid((3,3),(1,2),rowspan=2)
ax_4 = plt.subplot2grid((3,3),(2,0))
ax_4 = plt.subplot2grid((3,3),(2,1))

plt.show()
# method 2:gridspec
plt.figure()
gs = gridspec.GridSpec(3,3)
ax_1 = plt.subplot(gs[0,:])
ax_2 = plt.subplot(gs[1,:2])
ax_3 = plt.subplot(gs[1:,2])
ax_4 = plt.subplot(gs[-1,0])
ax_5 = plt.subplot(gs[-1,-2])

plt.show()
# method 3:easy to define structure
f,((ax_1,ax_2),(ax_3,ax_4)) = plt.subplots(2,2,sharex=True,sharey=True)
ax_1.scatter([1,2],[1,2])
plt.tight_layout()
# fig1,f1_axes = plt.subplots(ncols=2,nrows=2,constrained_layout=True)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzy6arh42oj20hs0dc0sz.jpg)图 20![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzy6arcvbmj20hs0dc74e.jpg)图 21![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzy6aqf7usj20hs0dcdfv.jpg)图 22

  相较于直接使用***subplot***方法不等分图像，上述三种方法更为便利：

* 首先观察*method_1*中代码：

  ```python
  ax_1 = plt.subplot2grid((3,3),(0,0),colspan=3,rowspan=1)
  ax_1.plot([1,2],[1,2])
  ax_1.set_title('ax_1_title')
  ```

  代码主要使用了***subplot2grid*** 方法进行网格划分，官方定义为：

  > ***A helper function that is similar to subplot(), but uses 0-based indexing and let subplot to occupy multiple cells.***

  观察图20，*ax_1*图像占据了第一行，图像设置仍是将图像整体划分成3行3列，即(3,3)，然后ax_1图像从(0,0)索引值开始放置（类似于列表索引，下标值从(0,0)开始，然后使用关键参数定义*ax_1*图像的范围，即*colspan=3,rowspan=1*(3列1行)。***set_title()*** 是设置图像标题方法。同理关于*ax_2* 和 *ax_3* 图像设置方法相同。

* 其次观察*method_2*中代码：

  ```python
  plt.figure()
  gs = gridspec.GridSpec(3,3)
  ax_1 = plt.subplot(gs[0,:])
  ax_2 = plt.subplot(gs[1,:2])
  ax_3 = plt.subplot(gs[1:,2])
  ax_4 = plt.subplot(gs[-1,0])
  ax_5 = plt.subplot(gs[-1,-2])
  ```

  ***gridspec*** 方法是通过设置将图像划分为特定的网格形状，官方解释为：

  > ***Specifies the geometry of the grid that a subplot will be placed ***

  首先通过 ***GridSpec*** 设置总的网格数量（以最小网格为单位），剩下的进行类似于列表的切片处理，如***ax_1 = plt.subplot(gs[0,:])*** 表示第1行的占据，***ax_2 = plt.subplot(gs[1,:2])*** 进行第2行，前两列位置的占据，而***ax_3 = plt.subplot(gs[1:,2])*** 则进行第二行到最后一行的行位置占据和第三列位置的占据，上下的依次类推。

* 其次观察*method_3*中代码：

  ```python
  f,((ax_1,ax_2),(ax_3,ax_4)) = plt.subplots(2,2,sharex=True,sharey=True)
  ax_1.scatter([1,2],[1,2])
  plt.tight_layout()
  # fig1,f1_axes = plt.subplots(ncols=2,nrows=2,constrained_layout=True)
  ```

  方法3主要涉及***subplots*** 方法的使用，官方定义为：

  > ***Perhaps the primary function used to create figures and axes. It’s also similar to `matplotlib.pyplot.subplot()`, but creates and places all axes on the figure at once. ***

  * 首先 ***f,*** 是定义了一个图像，其中 ***f*** 是***figure*** 的缩写，也可以写成***fig***，然后设置2行2列的网格图，四个网格图共享xy轴，需要注意的是，为了使整个图像更加的紧凑，代码使是用了***plt.tight_layout()***，个人感兴趣的话可以自己观察。
  * 我们也可以使用另外一种表示方式，即通过使用下述代码进行图像设置：***fig1,f1_axes=plt.subplots(ncols=2,nrows=2,constrained_layout=True)***，代码中***ncols,nrows*** 表示的都是要定义的列行数，***constrained_layout*** 的使用效果类似于***plt.tight_layout()***。

------

###### （4）图中图设置

  在某些情况下，我们需要对图中的细节进行放大，一般我们通过在图中进行绘制另一个小图，即图中图的模式，接下来将对图中图的格式及内容进行简单的介绍：

```python
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec

fig = plt.figure()
x = [1,2,3,4,5,6,7]
y = [1,3,4,2,5,8,6]

left,bottom,width,height = .1,.1,.8,.8
ax_1 = fig.add_axes([left,bottom,width,height])
ax_1.plot(x,y,'r')
ax_1.set_xlabel('x')
ax_1.set_ylabel('y')
ax_1.set_title('title')

left,bottom,width,height = .2,.6,.25,.25
ax_2 = fig.add_axes([left,bottom,width,height])
ax_2.plot(x,y,'b')
ax_2.set_xlabel('x')
ax_2.set_ylabel('y')
ax_2.set_title('title inside_1')

plt.axes([0.6,0.2,0.25,0.25])
plt.plot(y[::-1],x,'g')
plt.xlabel('x')
plt.ylabel('y')
plt.title('title inside_2')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzy8x1j32pj20hs0dcmxn.jpg)图 23

  通过观察代码发现，设置图中图的格式很简单，跟绘图时的情况一样，其中主要通过***fig.add_axes([left,bottom,width,height])*** 来实现：

* ***fig.add_axes()*** 如字面意所示，增加坐标轴线，内部参数 ***[left,bottom,width,height]*** 表示距离整个图框边缘的比例，如 ***left=0.1*** 表示整个图框从左向右的百分之10的位置绘制左框线，然后在 ***width=0.8*** 位置绘制右框线，同理***bottom*** 和 ***height***。
* 另外 ***(y[::-1],x)*** 表示x数据正序，而y数据降序。

#### 2.3 Matplotlib绘图样式

##### 2.3.1 2D图形

  Matplotlib中具有多种绘图样式，2D图中除了折线图外，还有2D的有**散点图**、**直方图**、**等高线图**以及3D图等，接下来将对Matplotlib中常用的图形样式进行简单的讲解。

------

###### （1） 散点图

  前面进行过简单的散点图的应用，但未就散点图作为整个专题进行讲述，接下来将针对散点图的基本应用做简要讲述：

```python
import matplotlib.pyplot as plt
import numpy as np

n = 1024
X = np.random.normal(0,1,n)
Y = np.random.normal(0,1,n)
T = np.arctan2(Y,X)    # for color value

# plt.scatter(X,Y,s=75,c=T,alpha=0.5)
plt.scatter(np.arange(5),np.arange(5))

# plt.xlim((-1.5,1.5))
# plt.ylim((-1.5,1.5))
plt.xticks(())
plt.yticks(())

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzyzbhqvinj20hs0dcq6l.jpg)图 24![img](https://ws1.sinaimg.cn/large/8f50be7fly1fzyz1zcpmwj20hs0dcq2r.jpg)图 25

  图24和图25分别由***plt.scatter(X,Y,s=75,c=T,alpha=0.5)*** 和 ***plt.scatter(np.arange(5),np.arange(5))*** 生成：

* 图24中X，Y用到了***np.random.normal(0,1,n)***，表示使用随机正态分布，其中0为平均值，1为方差，n为生成点的个数，代码中为1024；
* 另外***T = np.arctan2(Y,X)***，该代码是颜色生成函数，其作用是生成不同颜色的点，具体形式如图24所示，点的颜色都呈现出不同的颜色。
* 针对图25所生成的点中，用到了***arange()*** 函数，该函数与***range()*** 函数用法类似，但是***arange()*** 函数生成的事列表，即在代码中具体表现为$[1,2,3,4,5]$，而***range()*** 函数生成的为单个的***int*** 类型的整数。

------

###### （2） 柱状图

  在众多图形中，柱状图是我们遇到的比较常见的图形形式，方便我们对比观察不同的数据量，接下来将对柱状图的绘制进行简要概述：

```python
import matplotlib.pyplot as plt
import numpy as np

n = 12
X = np.arange(n)
Y_1 = (1-X/float(n))*np.random.uniform(0.5,1.0,n)
Y_2 = (1-X/float(n))*np.random.uniform(0.5,1.0,n)

plt.bar(X,+Y_1,facecolor='#9999ff',edgecolor='white')
plt.bar(X,-Y_2,facecolor='#ff9999',edgecolor='white')

for x,y in zip(X,Y_1):
    plt.text(x+0.04,y+0.05,'%.2f' %y,ha='center',va='bottom')

for x,y in zip(X,Y_2):
    plt.text(x+0.04,-y-0.05,'-%.2f' %y,ha='center',va='top')

plt.xlim(-.5,n)
plt.xticks(())    # 坐标值去除替换
plt.ylim(-1.25,1.25)
plt.yticks(())

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01201nbuij20hs0dct8v.jpg)图 26

  在代码中，主要涉及到以下几个方法：

* ***np.random.uniform(0.5,1.0,n)*** 表示随机均匀分布，0.5和1.0表示均匀分布的最小值和最大值，n表示生成的数据个数，代码中为12个；为了方便生成浮点数，代码中对Y值进行了***float()*** 类型的相应转换；

* ***plt.bar(X,+Y_1,facecolor='#9999ff',edgecolor='white')*** 整个方法的理解可以参照基本绘图方法***plt.plot()*** 的类型理解，另外***facecolor*** 表示柱状图内部填充颜色，颜色可使用的表示形式可查询2.2.4节（1）部分的讲解，***edgecolor*** 表示柱状图外在框线的颜色；

* 在循环函数中：

  ```python
  for x,y in zip(X,Y_1):
      plt.text(x+0.04,y+0.05,'%.2f' %y,ha='center',va='bottom')
  ```

  ***zip()*** 方法为Python语言内置方法，可将(X,Y_1)转换为元组打印。***plt.text()*** 标注前2.2.3节讲解过，此处不在进行详细解释，只针对内部参数进行讲解：*(x+0.04,y+0.05,'%.2f'\ %y,ha='center',va='bottom')* 表示柱状图y轴数据相对坐标位置*(x,y)* 偏移量为*(+0.04,+0.05)* 的位置，又由于标注内容整体是个框形区域，依次可以进一步对对齐位置进行设置，代码汇总设置为标注数据底部水平居中，其中***ha*** 和 ***va*** 分别表示水平和竖直方向上的设置（格式可以参照Excel中的手动对齐样式，可上下左右对齐）；

* 代码最后进行了坐标轴范围及显示设置即利用***plt.xlim()*** 和 ***plt.xticks()*** 函数，又***plt.xticks(())*** 中括号内数据为空，所以坐标轴数据显示无。

------

###### （3） 直方图

  在部分统计图中，直方图是一种比较重要的图像，需要注意的事直方图和前面的柱状图不同，需要区别对待，具体的区别可参考此处的博文，讲的比较细致，方便大家理解：[*直方图VS柱状图*](http://dy.163.com/v2/article/detail/DG3OF9N605118F5T.html)。接下来我们将进行简单的直方图的讲解：

```python
import numpy as np
import matplotlib.pyplot as plt

mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

plt.hist(x, 50, density=1, facecolor='g', alpha=0.75)
 
plt.xlabel('Smarts')
plt.ylabel('Probability')

plt.title('Histogram of IQ')

plt.text(60, .025, r'$mu=100, sigma=15$')
plt.axis([40, 160, 0, 0.03])
plt.grid(True)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g013lefnorj20hs0dcmxf.jpg)图 27

  如代码所示，代码主要分为以下几个部分：

* ***np.random.randn()*** 是生成正态分布的函数，它具有以下几个特性：

  > 语法：
  >
  > > > [np.random.randn(d0,d1,d2……dn)](https://docs.scipy.org/doc/numpy/reference/generated/numpy.random.randn.html)
  > > >
  > > > 1. 当函数括号内没有参数时，则返回一个浮点数；
  > > > 2. 当函数括号内有一个参数时，则返回秩为1的数组，不能表示向量和矩阵；
  > > > 3. 当函数括号内有两个及以上参数时，则返回对应维度的数组，能表示向量或矩阵；
  > > > 4. [np.random.standard_normal()](https://docs.scipy.org/doc/numpy/reference/generated/numpy.random.standard_normal.html#numpy.random.standard_normal)函数与**np.random.randn()**类似，但是**np.random.standard_normal()**的输入参数为元组(tuple).
  > > > 5. **np.random.randn()**的输入通常为整数，但是如果为浮点数，则会自动直接截断转换为整数。
  >
  > 作用：
  >
  > > 返回一个或一组服从标准正态分布的随机样本值。
  >
  > 特点：
  >
  > > 标准正态分布是以0为均数、以1为标准差的正态分布，记为N(0,1)。

  因此***np.random.randn(1000)*** 将随机生成1000个服从标准正态分布的样本值作为绘图数据。

* 绘制直方图，将利用方法***plt.hist(x, 50, normed=1, facecolor='g', alpha=0.75)***，基于官方数据，***hist*** 的参数主要有以下几个：

  ```python
  matplotlib.pyplot.hist(x, bins=None, range=None, density=None, weights=None, cumulative=False, bottom=None, histtype='bar', align='mid', orientation='vertical', rwidth=None, log=False, color=None, label=None, stacked=False, normed=None, *, data=None, **kwargs)  
  
  '''
  #################################################
  x: (n,) array or sequence of (n,) arrays
  指定每个bin分布的数据,对应x轴
  #################################################
  bins: integer or array_like, optional
  指定bin的个数,即共有几条条状图
  #################################################
  density: boolean, optional,If `True`, the first element of the return tuple will be the counts normalized to form a probability density, i.e.,`n/(len(x)`dbin)`
  指定密度,也就是每个条状图的占比例比,默认为1
  #################################################
  color: color or array_like of colors or None, optional
  '''
  ```

* ***plt.grid(True)*** 中，当参数为***False*** 时，图27中网格线将消失.

------

###### （4） 等高线图

  在部分工程学科中，我们通常会使用到等高线的绘制，方便我们观察整个图中的地形地貌，因此接下来将对等高线的绘制进行简要的概述：

```python
import matplotlib.pyplot as plt
import numpy as np

def f(x,y):
    # the height function
    return (1-x/2+x**5+y**3)*np.exp(-x**2-y**2)

n = 256
x = np.linspace(-3,3,n)
y = np.linspace(-3,3,n)
X,Y = np.meshgrid(x,y)

# use plt.contourf to filling contours
# X,Y and value for (X,Y) point
plt.contourf(X,Y,f(X,Y),8,alpha=0.85,cmap=plt.cm.rainbow)

# use plt.contour to add contour lines
C = plt.contour(X,Y,f(X,Y),8,colors='black',linewidth=.2)

# adding label
plt.clabel(C,inline=True,fontsize=10)

plt.xticks(())
plt.yticks(())
plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g012mfouhqj20hs0dc3zw.jpg)图 28

  代码主要分为以下几个部分：

* （1）坐标数据网格化：等高线图需要三维坐标数据，网格化的目的就是首先是建立平面网格化坐标（xy坐标系），然后再利用高度坐标进行网格数据升高，主要使用使用到的就是***np.meshgrid()*** 方法，另外需要说明的是代码中定义的函数目的是计算高度，没有其他含义；

* （2）等高线地图设置：根据等高线数据***plt.contourf(X,Y,f(X,Y),8,alpha=0.85,cmap=plt.cm.rainbow)*** 其中*(X,Y,f(X,Y))* 是坐标数据，不进行解释；方法中的**8** 表示从最小数据到最大数据将分成9条分界线，可以理解为*[0,8]*中共9个数据，分别将数据等分为 *(9+1)*个颜色区域进行显示；参数***cmap*** 表示图像映射类型，代码中使用了彩虹样式（***ranbow***）进行展示，处理***ranbow*** 外，还有以下映射方式进行展示：

  > ```python
  > Accent, Accent_r, Blues, Blues_r, BrBG, BrBG_r,BuGn, BuGn_r, BuPu, BuPu_r, CMRmap, CMRmap_r,Dark2, Dark2_r, GnBu, GnBu_r, Greens, Greens_r,Greys, Greys_r, OrRd, OrRd_r, Oranges, Oranges_r,PRGn, PRGn_r, Paired, Paired_r, Pastel1, Pastel1_r,Pastel2, Pastel2_r, PiYG, PiYG_r, PuBu, PuBuGn,PuBuGn_r, PuBu_r, PuOr, PuOr_r, PuRd, PuRd_r,Purples, Purples_r, RdBu, RdBu_r, RdGy, RdGy_r, RdPu,RdPu_r, RdYlBu, RdYlBu_r, RdYlGn, RdYlGn_r, Reds, Reds_r,Set1, Set1_r, Set2, Set2_r, Set3, Set3_r, Spectral,Spectral_r, Wistia, Wistia_r, YlGn, YlGnBu, YlGnBu_r,YlGn_r, YlOrBr, YlOrBr_r, YlOrRd, YlOrRd_r, afmhot,afmhot_r, autumn, autumn_r, binary, binary_r, bone,bone_r, brg, brg_r, bwr, bwr_r, cividis, cividis_r,cool, cool_r, coolwarm, coolwarm_r, copper, copper_r,cubehelix, cubehelix_r, flag, flag_r, gist_earth,gist_earth_r, gist_gray, gist_gray_r, gist_heat,gist_heat_r, gist_ncar, gist_ncar_r, gist_rainbow,gist_rainbow_r, gist_stern, gist_stern_r, gist_yarg,gist_yarg_r, gnuplot, gnuplot2, gnuplot2_r, gnuplot_r,gray, gray_r, hot, hot_r, hsv, hsv_r, inferno, inferno_r,jet, jet_r, magma, magma_r, nipy_spectral, nipy_spectral_r,ocean, ocean_r, pink, pink_r, plasma, plasma_r, prism, prism_r,rainbow, rainbow_r, seismic, seismic_r, spring, spring_r, summer,summer_r, tab10, tab10_r, tab20, tab20_r, tab20b, tab20b_r, tab20c,tab20c_r, terrain, terrain_r, twilight, twilight_r, twilight_shifted,twilight_shifted_r, viridis, viridis_r, winter, winter_r
  > ```

  为方便观察应用步骤，此处给出第二步下的生成图，如图28.1所示，通过图可以发现，前两步只给出了等高线背景图，但是缺少等高线，这也是第三步所做内容。

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g016kvxyurj20hs0dct8t.jpg)图 28.1

* （3）等高线线型设置：基于图28.1我们需要对不同区域的等高线进行边界线的添加，主要用到***C = plt.contour(X,Y,f(X,Y),8,colors='black',linewidth=.2)***，生成内容如图28.2所示：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g016kw2wkkj20hs0dcgmw.jpg)图 28.2

* （4）等高线标签设置：基于图28.2进行相应区域等高线数据的添加，主要涉及***plt.clabel(C,inline=True,fontsize=10)***，***clabel()*** 中的***C*** 为前述生成等高线C的指代内容，因此生成结果如图28所示，另外假如***inline=False*** 则线会穿过标签数据，如图28.3所示，显然不太美观：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01b5bsbzij20hs0dc3zx.jpg)图 28.3

------

###### （5） 数字图像

  有时我们会对部分数据进行图像绘制，此处所指图像是针对不同数据绘制不同区域以显示的图像，可能语言不直观，接下来以具体实例进行示意讲解：

```python
import matplotlib.pyplot as plt
import numpy as np

# image data
a = np.array([0.313660827978,0.365348418405,0.423733120134,
            0.365348418405,0.439599930621,0.525083754405,                        0.423733120134,0.525083754405,0.651536351379]).reshape(3,3)
'''
for the value of 'interpolation',check this:
http:/matplotlib.org/examples/images_contours_and_fields/interpolation_methods.html
'''
plt.imshow(a,interpolation='nearest',cmap='bone',origin='upper')
plt.colorbar(shrink=.9)

plt.xticks(())
plt.yticks(())
plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g012uw9pfoj20hs0dcjrd.jpg)图 29

  上述代码主要涉及以下几个部分：

* 数据处理部分：在代码中定义了一个列表，其包含9个数据，并利用***reshape()*** 方法将*(9*1)*的列表（矩阵）转换成了*(3*3)* 矩阵；

* 图像绘制部分：将数据展示为图片样式涉及使用方法***plt.imshow(a,interpolation='nearest',cmap='bone',origin='upper')***，参数***interpolation*** 展示方法有很多种，官网上给出了很多形式，具体网址为[*Matplotlib_interpolation*](https://matplotlib.org/examples/images_contours_and_fields/interpolation_methods.html)，方法主要有：

  > ```Python
  > None, 'none', 'nearest', 'bilinear', 'bicubic', 'spline16','spline36', 'hanning', 'hamming', 'hermite', 'kaiser', 'quadric','catrom', 'gaussian', 'bessel', 'mitchell', 'sinc', 'lanczos'
  > ```

  参数***cmap*** 是颜色映射形式，具体参考2.3.1节（4）等高线中的讲解，此处不再赘述；参数***origin*** 是处理数据大小排列问题，可以理解为降序或者升序。

* 图像标签设置：数据标签展示涉及***plt.colorbar(shrink=.9)*** 即图像右侧数据标签，内部参数众多，代码中使用了***shrink=0.9*** 即标签高度是整个图片图像的90%。

------

###### （6） 饼状图

  在我们统计数据时，饼状图能够很方便我们对整体-部分之间关系的描述，接下来我们将对饼状图进行简要概述：

```python
import matplotlib.pyplot as plt

label = 'Frogs', 'Hogs', 'Dogs', 'Logs'  # 设置标签
sizes = [15, 30, 45, 10]  # 占比，和为100
color = ['yellowgreen', 'gold', 'lightskyblue', 'lightcoral']  # 颜色
explode_ = (0, 0.1, 0, 0)  # 展开第二个扇形，即Hogs，间距为0.1

plt.pie(sizes, explode=explode_, labels=label, colors=color, autopct='%1.1f%%', shadow=True,startangle=90)
# startangle控制饼状图的旋转方向
plt.axis('equal')  # 保证饼状图是正圆，否则会有一点角度偏斜

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g013tdh2jkj20hs0dcq3i.jpg)图 30

  如代码所示，饼状图代码较为简单，主要利用了***plt.pie()*** 的方法进行绘制，其中常用的几个参数如下：

* **sizes:** 参数size主要定义了饼状图的数据分布，代码定义了4个数据，和为100，这是正好的情况，假如4个数据和不等于100，则程序自动计算各个数据在占据整体的百分比。当***size=[15, 40, 45, 10]*** 时，图像如30.1所示：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01dh4txjvj20hs0dcq3f.jpg)图 30.1

* **color:** 参数color可以根据颜色全称定义，也可以根据颜色16进制定义颜色；

* **label:** 参数label定义区域名称；

* **explode:** 参数explode定义部分区域偏移距离，如图30.1，代码中这对*Hogs* 进行偏移，偏移距离为0.1；

* **autopct:** 扇形区域数据表示格式，控制输出格式内容与***print()*** 控制输出数据格式长度及保留位数方法相同；

* **shadow:** 是否显示阴影，图30为显示**shadow**，图30.2为不显示**shadow**：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01dh4q6cyj20hs0dct91.jpg)图 30.2

* **startangle:** 表示饼状图以x轴为基准轴逆时针旋转角度，官方定义为：

  > ***If not None, rotates the start of the pie chart by angle degrees counterclockwise from the x-axis***

  图30.3为旋转45度后的图形：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01dh4xnbqj20hs0dcgm5.jpg)图 30.3

  另外除了上述参数外，***plt.pie()***还有其他参数，具体请参看官网[*Matplotlib_pie*](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.pie.html#matplotlib.pyplot.pie)，同时需要注意，上述参数中的数据一一对应，所以绘制是务必重视。

------

###### （7） 玫瑰图

  在部分领域，如气象等常使用风向玫瑰图进行统计数据展示，玫瑰图实际是一种2维极坐标统计图,以动径和动径角表示制图对象按方位、时间或多项指标的统计数据,有很好的直观效果。接下来我们将对玫瑰图的绘制做简要概述：

```python
import numpy as np
import matplotlib.pyplot as plt

N = 20
theta = np.linspace(0.0, 2 * np.pi, N, endpoint=False)
radii = 10 * np.random.rand(N)
width_ = np.pi / 4 * np.random.rand(N)
# 验证
# print(theta*/np.pi,radii,width)

ax = plt.subplot(111, projection='polar')
bars = ax.bar(theta, radii, width=width_, bottom=0.0)

# Use custom colors and opacity
for r, bar in zip(radii, bars):
    bar.set_facecolor(plt.cm.jet(r / 10.))
    bar.set_alpha(0.5)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01488v3knj20hs0dcgn2.jpg)图 31

  如代码所示，玫瑰图基于极坐标图绘制，观察代码，主要涉及以下几个部分：

* （1）数据处理：***np.linspace()*** 的参数定义为：

  ***linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)***，基于参数定义，代码中的最后一个数值将不被利用。

  > **start:** scalar
  >
  > 队列的开始值
  >
  > **stop:** scalar
  >
  > 队列的结束值。当‘endpoint=False’时,不包含该点。在这种情况下，队列包含除了“num+1"以外的所有等间距的样本，要注意的是，当‘endpoint=False’时，步长会发生改变。
  >
  > **num:** int, optional
  >
  > 要生成的样本数。默认是50，必须要非负。
  >
  > **endpoint:** bool, optional
  >
  > 如果是True，’stop'是最后的样本。否则，'stop'将不会被包含。默认为true
  >
  > **retstep:** bool, optional
  >
  > If True, return (`samples`, `step`), where `step` is the spacing between samples.

* （2）极坐标绘制：***ax = plt.subplot(111, projection='polar')*** 代码利用参数***projection*** 进行极坐标的启用，实际上***ax = plt.subplot(111, polar=True)*** 这样写也是可以的，**polar=True** 等价于**projection='polar'**，另外**projection='rectilinear'** 为默认设置，即为直角坐标系，具体参数可参加官网 [*matplotlib_projections*](https://matplotlib.org/api/projections_api.html#module-matplotlib.projections)。生成图形如图31.1所示：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01gr0dnc1j20hs0dcdgu.jpg)图 31.1

* （3）玫瑰花瓣绘制：基于数据和极坐标系，利用***bars = ax.bar(theta, radii, width=width_, bottom=0.0)*** 进行玫瑰花瓣的绘制，其中**(theta, radii, width)** 分别为花瓣角度，半径和花瓣高度，**theta**即每个花瓣对称轴位置与0度所成角度；**radii** 即为花瓣半径，**width** 即为花瓣外延侧长度，可以理解弧长的长度；需要注意的是**theta** 并非花瓣两条半径之间的角度，而是花瓣对称轴与0度 轴线所成角度；另外可通过修改三个参数内的数字并打印然后并对比图像可验证三个参数的具体含义（代码中注释为验证的位置）。**bottom** 是设置花瓣内沿侧宽度，观察图31.2即可明白，通过将**bottom** 增加为10，则可获得。另外需要强调，由于花瓣数据随机生成，所以每次上述的**(theta, radii, width)** 会有所不同：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01jlyf8cpj20hs0dc763.jpg)图 31.2

* （4）玫瑰花瓣上色：代码中通过循环语句相应操作进行不同花色的显示：

  ```python
  for r, bar in zip(radii, bars):
      bar.set_facecolor(plt.cm.jet(r / 10.))
      bar.set_alpha(0.5)
  ```

  代码中主要说明两点，

  > a. 循环参数中**r** 不是固定的，也可用**width_** 进行处理，但是由于width_数值变化小，所以会造成花瓣整体色差不明显，因此用**r** 效果更好些，但是**bar** 是必须存在，不可替换，因为着色对象即为**bar**。
  >
  > b. 在花瓣内部着色中用到了**plt.cm.jet()**，关于**jet**，官方给出的定义是：
  >
  > > ***Set the colormap to "jet".This changes the default colormap as well as the colormap of the current image if there is one.***
  >
  > 代码中的**jet**可参照等高线图中给出的色表，其效果等同于rainbow等。

------

##### 2.3.2 3D图形

  Matplotlib虽然是2D图像库，但是仍能够简单绘制部分3D图像，接下来将对3D图像的简单绘制进行介绍：

###### （1） 散点图

  首先进行3D散点图的讲解，相对来说3D散点图应用较少，但其仍是一种很重要的3D数据表现形式，因此接下来将对其进行简要讲解：

```python
# 3维散点图
import numpy as np
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt

# x, y, z 均为 0 到 1 之间的 100 个随机数
x = np.random.normal(0, 1, 100)
y = np.random.normal(0, 1, 100)
z = np.random.normal(0, 1, 100)

fig = plt.figure()
ax = Axes3D(fig)

ax.scatter(x, y, z)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01lctojbjj20hs0dcdhr.jpg)图 32

  绘制3D图前首先要导入***mpl_toolkits.mplot3d.Axes3D*** 图形库，但同时仍需要使用pyplot 模块，因为Matplotlib 绘制三维图像实际上是在二维画布上展示。mplot3d 模块下主要包含 5 个大类，分别为***axes3d***、***axis3d***、***art3d***、***Art3D Utility Functions*** 和 ***proj3d***，其中最常用的就是***axes3d***，其他详细情况请参见官网：[*Mplot3d_API*](https://matplotlib.org/api/toolkits/mplot3d.html#axes3d)。

  代码内容很简单，主要用到的方法为***ax = Axes3D()***，即添加三维坐标轴，具体如图32.1所示，然后依据*(x,y,z)* 坐标绘制散点图即可得到图32所示图形。

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01o41fin6j20hs0dcta6.jpg)图 32.1

###### （2） 曲线图

  接下来进行3D曲线图的讲解，同样作为3D绘图形式必不可少的绘图形式，接下来将对3D曲线图进行简要概述：

```python
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import numpy as np

# 生成数据
x = np.linspace(-6 * np.pi, 6 * np.pi, 1000)
y = np.sin(x)
z = np.cos(x)

# 创建 3D 图形对象
fig = plt.figure()
ax = Axes3D(fig)

ax.plot(x, y, z)
plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01lhsv3aaj20hs0dc77h.jpg)图 33

  代码整体形式与三维散点图代码类似，因此在此不再做过多赘述，主要还是利用***ax = Axes3D()*** 绘制出坐标轴后进行三维数据的添加绘制。

###### （3） 柱状图

  通过三维柱状图可以多维度的反映数据变化，比二维柱状图更加形象，接下来将对其进行简单概述：

```python
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import numpy as np

# 创建 3D 图形对象
fig = plt.figure()
ax = Axes3D(fig)

# 生成数据并绘图
x = [0, 1, 2, 3, 4, 5, 6]
for i in x:
    y = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    z = abs(np.random.normal(1, 10, 10))
    ax.bar(y, z, i, zdir='y', color=['r', 'g', 'b', 'y'])

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01lomkut8j20hs0dcdhq.jpg)图 34

  此处主要进行下两部分内容的讲解：

* ***zdir()*** 表示向某方向投影的函数，代码中函数参数为y，即将3维柱状图转换为2维柱状图视角的话，整个柱面基于y轴，同理通过修改参数，将其改为x，则图形显示为图31.1所示结果：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01ollsws2j20hs0dcq4w.jpg)图 34.1

* 在循环语句中，确定z轴颜色时，代码给出了***color=['r', 'g', 'b', 'y']*** 四种颜色，其实这个没有数量固定，也可以给一种颜色，也可以给多于4种的颜色，以下是只给出红色的情况下结果的显示：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01olg7t9vj20hs0dc0u6.jpg)图 34.2

###### （4） 线框图

  线框图是曲面图中网格线的图像，以下将对其进行简要概述：

```python
import numpy as np
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(8,6))
ax = fig.add_subplot(1, 1, 1, projection='3d')
ax = Axes3D(fig)

x = np.arange(-4,4,0.25)
y = np.arange(-4,4,0.25)
X,Y = np.meshgrid(x,y)
R = np.sqrt(X**2+Y**2)
# height value
Z = np.sin(R)
# rstride和cstride分别为网格下的行跨和列跨
ax.plot_wireframe(X, Y, Z, rstride=1, cstride=1)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01matj4kyj20m80gojz0.jpg)图 35

  此处重点对***ax.plot_wireframe(X, Y, Z, rstride=1, cstride=1)*** 进行解说，绘制线框图需要利用***ax.wireframe()*** 的方法，该方法的参数较多，具体可查看官网说明：[*plot_wireframe*](https://matplotlib.org/api/_as_gen/mpl_toolkits.mplot3d.axes3d.Axes3D.html?highlight=plot_wireframe#mpl_toolkits.mplot3d.axes3d.Axes3D.plot_wireframe)，此处重点进行内部参数***rstride*** 和 ***cstride*** 的解释。参数***rstride*** 和 ***cstride*** 分别为网格下的行跨和列跨，两参数需是正整数（为负整数时，图像消失；为小数时，程序出错），且密度与数字成反比，也就是说当 ***(rstride=1, cstride=1)*** 时，网格最密，以下是修改网格，使 ***(rstride=3, cstride=3)*** 的情形：

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g01pdsts0vj20m80go79y.jpg)图 35.1

###### （5） 曲面图

  曲面图（Surface Plot）是3D图像中常用的形式，接下来将对3D曲面图进行简要讲解：

```python
import matplotlib.pyplot as plt
import numpy as np
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = Axes3D(fig)
# X,Y value
X = np.arange(-4,4,0.25)
Y = np.arange(-4,4,0.25)
X,Y = np.meshgrid(X,Y)
R = np.sqrt(X**2+Y**2)
# height value
Z = np.sin(R)
# rstride和cstride分别为网格下的行跨和列跨
ax.plot_surface(X,Y,Z,rstride=1,cstride=1,cmap=plt.get_cmap('rainbow'))
# zdir是三维图投影方向的方法
ax.contourf(X,Y,Z,zdir='z',offset=-2,cmap='rainbow')
ax.set_zlim(-2,2)

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g012yuf11ij20hs0dcq6a.jpg)图 36

  针对代码主要介绍三个内容：

* ***ax.set_zlim(-2,2):*** 设置z轴范围，即效果同***xlim()*** 和 ***ylim()*** 作用相同；

* ***ax.plot_surface():*** 即绘制三维曲面图函数***ax.plot_surface()***，其中，***cmap=plt.get_cmap('rainbow')*** 等价于***cmap=plt.cm.rainbow*** 和 ***cmap=‘rainbow’***，其余内部参数前面已做讲述，不再赘述。

* ***ax.contourf():*** 设置投影等高线图，实质上***contour*** 和***contourf*** 都能进行等高线投影图的设置，不过***contour*** 不同区域间未进行填充，而***contourf*** 区域内部进行了填充，将代码中的***contourf*** 修改为 ***contour*** 则可得图36.1所示图形：

  ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g028i9pho0j20hs0dcjve.jpg)图 36.1

##### 2.3.3 动图绘制

  在部分场合我们需要观察一段时期内图形的数据变化，相对于静图，动态在该种场合下优势就比较明显，因此接下来将对动图的设置进行简要介绍：

```python
import matplotlib.pyplot as plt
from matplotlib import animation
import numpy as np

fig,ax = plt.subplots()

x = np.arange(0,2*np.pi,0.01)
line, = ax.plot(x,np.sin(x))

def animate(i):
    line.set_ydata(np.sin(x+i/10))
    return line,

def init():
    line.set_ydata(np.sin(x))
    return line,
# 图像元素全部更改blit=False,变化元素更改blit=True
ani = animation.FuncAnimation(fig=fig,func=animate,frames=100,init_func=init,interval=100,blit=False)
# 保存GIF格式
ani.save('st_.gif',writer='imagemagick')

plt.show()
```

![img](https://ws1.sinaimg.cn/large/8f50be7fly1g02hg9z6ezg20hs0dc168.gif)

图 37

  使用动图需要导入***animation*** 模块，基于官方定义，详细请参见[*Animation*](https://matplotlib.org/api/animation_api.html?highlight=animation#module-matplotlib.animation)：

> ***The easiest way to make a live animation in matplotlib is to use one of the Animation classes.***
>
> （1）`FuncAnimation` —— Makes an animation by repeatedly calling a function `func`.
>
> （2）`ArtistAnimation` —— Animation using a fixed set of `Artist` objects.

  上述代码主要涉及以下几个关键函数和方法：

* ***animation.FuncAnimation:*** 设置动图需要使用方法***FuncAnimation***，其中定义了几个关键参数：

  > （1）***fig:*** 设置动图的图形界面；
  >
  > （2）***func:*** 设置动图变化的函数；
  >
  > （3）***frames:*** 设置动图帧数，即可以看做图像更新次数，其中图37.1表示帧数为10，由于帧数未形成一个周期，所以相对于图37来说显得有点断断续续：
  >
  > ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g02hz92euwg20hs0dcac3.gif)图 37.1
  >
  > （4）***init_func:*** 函数最初形式，即动画第一帧的函数图像；
  >
  > （5）***interval:*** 设置动画更新间隔时间；其中图37.2表示间隔为500，由于间隔时间较长，所以图37.2显得有点迟钝，图37.3表示间隔为20，相较于图37和图37.2，图37.3变化速度要快很多，但是需要注意点，由于帧数不合适，所以在速度较快的情况下，显示出与图37.1类似的情况，因此***interval*** 和 ***frame*** 要选择恰当：
  >
  > ![img](https://ws1.sinaimg.cn/large/8f50be7fly1g02i3spfbvg20hs0dc14g.gif)图 37.2![img](https://ws1.sinaimg.cn/large/8f50be7fly1g02i67h9m0g20hs0dc0y8.gif)图 37.3
  >
  > （6）***blit:*** 设置图像元素更新对象，blit=False时，图像元素全部更新，blit=True，表示产生变化的元素更改（不变化的元素不更新）

* 在***animate(i)*** 中，需要传入变化值，此处为 *i* ，取值范围为*[0,frames]*；

* ***line,***：由于***plot*** 返回值为列表，***line,*** 作为列表的第一个值，需要添加**,**，同理在函数***init()*** 中的表示。

  最后需要说明的是，Matplotlib并没有内置的GIF导出模块，需要借助第三方工具，此处给出设置详细步骤及下载工具界面，请参见网址[*Matplotlib2Gif*](https://my.oschina.net/u/2474629/blog/1794311)。