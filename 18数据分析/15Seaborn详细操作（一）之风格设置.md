# Seaborn简介

seaborn同matplotlib一样，也是Python进行数据可视化分析的重要第三方包。但seaborn是在
matplotlib的基础上进行了更高级的API封装，使得作图更加容易，图形更加漂亮。

seaborn并不能替代matplotlib。虽然seaborn可以满足大部分情况下的数据分析需求，但是针对一些特殊情况，还是需要用到matplotlib的。换句话说，matplotlib更加灵活，可定制化，而seaborn像是更高级的封装，使用方便快捷。

应该把seaborn视为matplotlib的补充，而不是替代物。

## 绘图风格设置

    
    
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib as mpl

首先，我们定义一个简单的函数来绘制一些正弦波，用于测试

    
    
    def sinplot(flip = 1):
        x = np.linspace(0, 14, 100)
        for i in range(1, 7):
            plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)
    
    
    sinplot()

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093339974-1818300309.png)  

转换为seaborn默认绘图，可以简单的用set()方法。

    
    
    import seaborn as sns
    
    
    sns.set()
    sinplot()

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093401960-1454740231.png)  

`Seaborn` 将 `matplotlib`
的参数划分为两个独立的组合。第一组是设置绘图的外观风格的，第二组主要将绘图的各种元素按比例缩放的，以至可以嵌入到不同的背景环境中。

操控这些参数的接口主要有两对方法：

  * 控制风格：`axes_style()`, `set_style()`
  * 缩放绘图：`plotting_context()`, `set_context()`

每对方法中的第一个方法（`axes_style()`,
`plotting_context()`）会返回一组字典参数，而第二个方法（`set_style()`,
`set_context()`）会设置`matplotlib`的默认参数。

### Seaborn的五种绘图风格

有五种seaborn的风格，它们分别是：darkgrid, whitegrid, dark, white,
ticks。它们各自适合不同的应用和个人喜好。默认的主题是darkgrid。

    
    
    sns.set_style('whitegrid')
    data = np.random.normal(size = (20, 6)) + np.arange(6) / 2
    sns.boxplot(data = data)
    
    
    <matplotlib.axes._subplots.AxesSubplot at 0x1a1ae6da20>

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093417101-1199487984.png)  

    
    
    sns.set_style('dark')
    sinplot()

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093429075-567091040.png)  

    
    
    sns.set_style('white')
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093441565-509836713.png)  

    
    
    sns.set_style('ticks')
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093520369-371424266.png)  

### 移除轴脊柱

`white`和`ticks`两种风格都可以移除顶部和右侧的不必要的轴脊柱。使用matplotlib是无法实现这一需求的，但是使用seaborn的despine()方法可以实现

    
    
    sinplot()
    sns.despine()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093534103-1605732016.png)  

一些绘图也可以针对数据将轴脊柱进行偏置，当然也是通过调用despine()方法来完成。而当刻度没有完全覆盖整个轴的范围时，trim参数可以用来限制已有脊柱的范围。

    
    
    f, ax = plt.subplots()
    sns.violinplot(data=data)
    sns.despine(offset=10, trim=True)
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093549724-492048250.png)  

也可以通过despine()控制哪个脊柱将被移除。

    
    
    sns.set_style("whitegrid")
    sns.boxplot(data=data, palette="deep")
    sns.despine(left=True)
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093606528-1717794450.png)  

### 临时设置绘图风格

虽然来回切换风格很容易，但是你也可以在一个with语句中使用axes_style()方法来临时的设置绘图参数。这也允许你用不同风格的轴来绘图：

    
    
    with sns.axes_style("darkgrid"):
        plt.subplot(211)
        sinplot()
    plt.subplot(212)
    sinplot(-1)
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093640340-1370324856.png)  

### 覆盖seaborn风格元素

如果你想定制化seaborn风格，你可以将一个字典参数传递给axes_style()和set_style()的参数rc。而且你只能通过这个方法来覆盖风格定义中的部分参数。

如果你想要看看这些参数都是些什么，可以调用这个方法，且无参数，这将会返回下面的设置：

    
    
    sns.axes_style()
    
    
    
    {'axes.facecolor': 'white',
     'axes.edgecolor': '.8',
     'axes.grid': True,
     'axes.axisbelow': True,
     'axes.labelcolor': '.15',
     'figure.facecolor': 'white',
     'grid.color': '.8',
     'grid.linestyle': '-',
     'text.color': '.15',
     'xtick.color': '.15',
     'ytick.color': '.15',
     'xtick.direction': 'out',
     'ytick.direction': 'out',
     'lines.solid_capstyle': 'round',
     'patch.edgecolor': 'w',
     'image.cmap': 'rocket',
     'font.family': ['sans-serif'],
     'font.sans-serif': ['Arial',
      'DejaVu Sans',
      'Liberation Sans',
      'Bitstream Vera Sans',
      'sans-serif'],
     'patch.force_edgecolor': True,
     'xtick.bottom': False,
     'xtick.top': False,
     'ytick.left': False,
     'ytick.right': False,
     'axes.spines.left': True,
     'axes.spines.bottom': True,
     'axes.spines.right': True,
     'axes.spines.top': True}
    

然后，就可以设置这些参数的不同版本了。

    
    
    sns.set_style("darkgrid",{'axes.facecolor':"0.9"})
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093658065-1366186932.png)  

### 绘图元素比例

我们可以通过一套参数控制绘图元素的比例  
首先，我们通过`set()`方法重置默认的参数

    
    
    sns.set()
    

有四个预置的环境，按大小从小到大排列分别为：`paper`, `notebook`, `talk`, `poster`。其中，`notebook`是默认的。

    
    
    sns.set_context('paper')
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093710372-656205005.png)  

    
    
    sns.set_context('talk')
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093722586-402996735.png)  

    
    
    sns.set_context('poster')
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093735929-1810952161.png)  

我们可以通过`set_context()`方法设置相关参数，并且你可以通过提供一个字典参数值来覆盖参数。当改变环境时，你也可以独立的去缩放字体元素的大小。

    
    
    sns.set_context('notebook',font_scale = 1.5, rc = {'lines.linewidth':2.5})
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093747907-1497471408.png)  

同样也可以通过`with`语句控制绘图的比例

## 颜色风格设置

在Seaborn的使用中，是可以针对数据类型而选择合适的颜色，并且使用选择的颜色进行可视化，节省了大量的可视化的颜色调整工作。

还是一样，在介绍如何使用颜色外观设置之前，我们引入所需要的模块。

    
    
    import numpy as np
    import seaborn as sns
    import matplotlib.pyplot as plt
    
    
    
    sns.set(rc={'figure.figsize':(6, 6)})
    np.random.seed(sum(map(ord, 'palettes')))
    

### 建立调色板

对于不连续的外观颜色设置而言，最重要的函数恐怕要属color_palette了。这个函数拥有许多方法，让你可以随心所欲的可以生成各种颜色。并且，它可以被任何有palette参数的函数在内部进行使用（palette的中文意思是
"调色板"）。

关于这个函数有几个点需要知道一下：

  * color_palette函数可以接受任何seaborn或者matplotlib颜色表中颜色名称（除了jet），也可以接受任何有效的matplotlib形式的颜色列表（比如RGB元组，hex颜色代码，或者HTML颜色名称）。
  * 这个函数的返回值总是一个由RGB元组组成的列表，无参数调用color_palette函数则会返回当前默认的色环的列表。

    
    
    sns.color_palette()
    
    
    
    [(0.2980392156862745, 0.4470588235294118, 0.6901960784313725),
     (0.8666666666666667, 0.5176470588235295, 0.3215686274509804),
     (0.3333333333333333, 0.6588235294117647, 0.40784313725490196),
     (0.7686274509803922, 0.3058823529411765, 0.3215686274509804),
     (0.5058823529411764, 0.4470588235294118, 0.7019607843137254),
     (0.5764705882352941, 0.47058823529411764, 0.3764705882352941),
     (0.8549019607843137, 0.5450980392156862, 0.7647058823529411),
     (0.5490196078431373, 0.5490196078431373, 0.5490196078431373),
     (0.8, 0.7254901960784313, 0.4549019607843137),
     (0.39215686274509803, 0.7098039215686275, 0.803921568627451)]
    

  * 还有一个相应的函数，是set_palette，它接受与color_palette一样的参数，并会对所有的绘图的默认色环进行设置。当然，你也可以在with语句中使用color_palette来临时的改变默认颜色。

通常，在不知道数据特点的情况下，要找出并知道哪组颜色对一组数据是最好的有点不太现实。因此，我们将分为多种方式来使用color_palette函数和其它的
seaborn paletee 函数。

有三种通用的color palette可以使用，它们分别是：qualitative，sequential，diverging。

#### 分类色板（quanlitative）

Qualitative调色板，也可以说成是 类型 调色板，因为它对于分类数据的显示很有帮助。当你想要区别 不连续的且内在没有顺序关系的
数据时，这个方式是最好的。

当导入seaborn时，默认的色环就被改变成一组包含6种颜色的调色板，它使用了标准的matplolib色环，为了让绘图变得更好看一些。

    
    
    current_palette = sns.color_palette()
    sns.palplot(current_palette)
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093812701-1398027459.png)  

有6种不同的默认主题，它们分别是：deep，muted，pastel，birght，dark，colorblind。

    
    
    themes = ['deep', 'muted', 'pastel', 'bright', 'dark', 'colorblind']
    for theme in themes:
        current_palette = sns.color_palette(theme)
        sns.palplot(current_palette)
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093823754-899502740.png)  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093836864-2058269712.png)  

  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093854834-1596968601.png)  

  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093901994-1401162474.png)  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093910344-1425707853.png)  

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093924071-543508432.png)  

#### 使用色圈系统

默认是有10种颜色，但是我们要想要其他种类的颜色分配

或者说当你有超过10种类型的数据要区分时，最简单的方法就是
在一个色圈空间内使用均匀分布的颜色。这也是当需要使用更多颜色时大多数seaborn函数的默认方式。

最常用的方法就是使用 hls 色空间，它是一种简单的RGB值的转换。

    
    
    sns.palplot(sns.color_palette('hls', 8))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093939605-1867864774.png)  

除此之外，还有一个 hls_palette 函数，它可以让你控制 hls 颜色的亮度和饱和度。

    
    
    sns.palplot(sns.hls_palette(8, l=.3, s=.7))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011093952534-2123932967.png)  

然而，由于人类视觉系统工作的原因，根据RGB颜色产生的平均视觉强度的颜色，从视觉上看起来并不是相同的强度。如果你观察仔细，就会察觉到，黄色和绿色会更亮一些，而蓝色则相对暗一些。因此，如果你想用hls系统达到一致性的效果，就会出现上面的问题。

为了修补这个问题，seaborn给hls系统提供了一个接口，可以让操作者简单容易的选择均匀分布，且亮度和饱和度看上去明显一致的色调。

    
    
    sns.palplot(sns.color_palette('husl', 8))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094006417-844907006.png)  

同样与之对应的，也有个husl_palette函数提供更灵活的操作。

#### 使用分类Color Brewer调色板

另外一种对分类数据比较友好的调色板来自Color
Brewer工具。在matplotlib中也存在这些颜色表，但是它们并没有被合适的处理。在seaborn中，当你想要分类的 Color Brewer
调色板的时候，你总是可以得到不连续颜色，但是这也意味着在某一点上，这些颜色将会开始循环。

Color Brewer
网站中的一个很好的特点就是它提供了一个色盲安全指导。色盲颜色有很多种http://en.wikipedia.org/wiki/Color_blindness,
但是最常见的当属辨别绿色和红色。如果可以避免使用红色和绿色来对绘图元素上色，那么对于一些色盲人群将会是一个很好的消息。

下面两组颜色就是使用红色和绿色组合，这可能并不是最好的选择。

    
    
    sns.palplot(sns.color_palette('Paired',8))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094017541-1386514493.png)  

    
    
    sns.palplot(sns.color_palette("Set2", 10))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094027869-399131998.png)  

为了避免这些组合，我们需要从Color Brewer库中进行选择调色，有一个专门的 choose_colorbrewer_palette
函数可以实现这个功能。这个函数需要在 IPython notebook 中使用，因为 notebook
是一个交互式的工具，可以让你浏览各种选择并且调节参数。

    
    
    sns_type = ["qualitative", "sequential", "diverging"]
    for elem in sns_type:
        sns.choose_colorbrewer_palette(elem)
    
    
    
    interactive(children=(Dropdown(description='name', options=('Set1', 'Set2', 'Set3', 'Paired', 'Accent', 'Paste…
    
    
    
    interactive(children=(Dropdown(description='name', options=('Greys', 'Reds', 'Greens', 'Blues', 'Oranges', 'Pu…
    
    
    
    interactive(children=(Dropdown(description='name', options=('RdBu', 'RdGy', 'PRGn', 'PiYG', 'BrBG', 'RdYlBu', …
    

当然，您可能只想使用一组您特别喜欢的颜色。因为color_palette()接受一个颜色列表，这很容易做到。

    
    
    flatui = ["#9b59b6", "#3498db", "#95a5a6", "#e74c3c", "#34495e", "#2ecc71"]
    sns.palplot(sns.color_palette(flatui))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094055327-800408246.png)  

### 连续色板（sequential）

调色板的第二大类被成为
"顺序"，这种调色板对于有从低（无意义）到高（有意义）范围过度的数据非常适合。尽管有些时候你可能想要在连续色板中使用不连续颜色，但是更通用的情况下是连续色板会作为颜色表在
kdeplot() 或者 corrplot() 或是一些 matplotlib 的函数中使用。

对于连续的数据，最好是使用那些在色调上有相对细微变化的调色板，同时在亮度和饱和度上有很大的变化。这种方法将自然地将数据中相对重要的部分成为关注点。

Color Brewer 的字典中就有一组很好的调色板。它们是以在调色板中的主导颜色(或颜色)命名的.

    
    
    sns.palplot(sns.color_palette('Blues'))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094110479-1316110829.png)  

与在matplotlib中一样，如果想要翻转渐变，可以在名称后加`_r`后缀

    
    
    sns.palplot(sns.color_palette('Blues_r'))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094122198-1221712761.png)  

seaborn还增加了一个允许创建没有动态范围的"dark"面板。如果你想按顺序画线或点，这可能是有用的，因为颜色鲜艳的线可能很难区分。

类似的，这种暗处理的颜色，需要在面板名称中添加一个_d后缀。

    
    
    sns.palplot(sns.color_palette('GnBu_d'))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094133512-612897693.png)  

注意，你可能想使用 choose_colorbrewer_palette()
函数取绘制各种不同的选项。如果你想返回一个变量当做颜色映射传入seaborn或matplotlib的函数中，可以设置 as_cmap 参数为True。

### 离散色板

调色板中的第三类被称为“离散”。这类色板适用于数据特征含有大的低值和大的高值。数据中通常有一个意义明确的中点。例如，如果你想要从某个基线时间点绘制温度变化，最好使用离散的颜色表显示相对降低和相对增加面积的地区。

除了你想满足一个低强度颜色的中点以及用不同起始颜色的两个相对微妙的变化，其实选择离散色板的规则类似于顺序色板。同样重要的是，起始值的亮度和饱和度是相同的。

同样重要的是要强调，应该避免使用红色和绿色，因为大量的潜在观众将无法分辨它们。

同样，Color Brewer颜色字典里也同时拥有一套精心挑选的离散颜色映射

    
    
    sns.palplot(sns.color_palette('BrBG', 7))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094144888-165045869.png)  

    
    
    sns.palplot(sns.color_palette("RdBu_r", 7))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094200267-14014279.png)  

    
    
    sns.palplot(sns.color_palette("coolwarm", 7))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094210028-788820583.png)  

#### 定制的离散色板

也可以使用seaborn函数 diverging_palette()
为离散的数据创建一个定制的颜色映射。（当然也有一个类似配套的互动工具：choose_diverging_palette()）。该函数使用husl颜色系统的离散色板。你需要传递两种色调，并可选择性的设定明度和饱和度的端点。函数将使用husl的端点值及由此产生的中间值进行均衡。

    
    
    sns.palplot(sns.diverging_palette(220, 20, n=7))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094219941-2147036235.png)  

    
    
    sns.palplot(sns.diverging_palette(145, 280, s=85, l=25, n=7))
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094229846-1498796691.png)  

#### 设置默认的调色板

color_palette() 函数有一个名为set_palette()的配套使用函数。
set_palette()。set_palette()接受与color_palette()相同的参数，但是它会更改默认的matplotlib参数，以便成为所有的调色板配置。

    
    
    def sinplot(flip=1):
        x = np.linspace(0, 14, 100)
        for i in range(1, 7):
            plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)
    sns.set_palette("husl")
    sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094253831-452638655.png)  

color_palette()函数也可以在一个with块中使用，以达到临时更改调色板的目的。

    
    
    with sns.color_palette("PuBuGn_d"):
        sinplot()
    

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191011094321679-1347845113.png)  

