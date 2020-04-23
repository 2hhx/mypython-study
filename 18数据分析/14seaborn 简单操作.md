## 1、Seaborn

在上节中我们学习了matplotlib，这节课我们来看看另一个可视化的模块`seaborn`，它是基于matplotlib的更高级的开源库，主要用作于数据可视化，解决了matplotlib的两大问题。正如Michael
Waskom所说的：`Matplotlib试着让简单的事情更加简单，困难的事情变得可能，那么Seaborn就是让困难的东西更加简单。`

使用matplotlib最大的问题就是它默认的各种参数，在serborn当中则完全避免了这些问题

    
    
    # 导入模块
    import matplotlib.pyplot as plt
    import pandas as pd
    
    # 初始化 Figure 和 Axes 对象
    fig, ax = plt.subplots()
    
    # 加载数据
    tips = pd.read_csv("https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv")
    
    # 创建 violinplot
    ax.violinplot(tips["total_bill"], vert=False)

![png](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142700754-1166876379..png)

    
    
    # 导入模块
    import matplotlib.pyplot as plt
    import pandas as pd
    import seaborn as sns
    
    # 加载数据
    tips = pd.read_csv("https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv")
    
    # 创建 violinplot
    sns.violinplot(x = "total_bill", data=tips)
    
    # 展示图像
    plt.show()

![png](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142702174-1787133027..png)

使用matplotlib，通常都需要我们自己来增加颜色、刻度标签等一些样式。seaborn就是matplotlib的延伸，如果会用matplotlib，那么使用seaborn也没问题

## 2、加载数据

使用seaborn不仅可以将自己本地的数据绘制成图表，还可以使用库本身提供的内置数据集

### 2.1、加载内置数据集

可以通过`load_dataset()`来使用内置的seaborn数据集，需要查看所有内置数据集可以点击<https://github.com/mwaskom/seaborn-
data>

    
    
    # 导入模块
    import seaborn as sns
    import matplotlib.pyplot as plt
    
    # 加载鸢尾花数据
    iris = sns.load_dataset("iris")
    
    # 构建 iris plot
    sns.swarmplot(x="species", y="petal_length", data=iris)
    
    # 展示 图像
    plt.show()

![png](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142704413-1624637401..png)

### 2.2、加载本地数据集

    
    
    数据可视化大部分的应用场景都是使用自己的数据集，seaborn主要适用于我们平时常用的DataFrame数组。它是一种类似于二维数组的数据结构。
    Seaborn对DataFrame非常友好的原因是，因为DataFrame的标签会自动导入到图表当中。在第一个Seaborn示例中绘制的一个小提琴图像，x轴会自动添加一个标签`total_bill`，这个在matplotlib中就需要我们自己手动添加了。
    
    
    # 导入模块
    import matplotlib.pyplot as plt
    import seaborn as sns
    import tushare as ts
    
    data = ts.get_k_data('000001')
    data = data.tail()
    
    # 建立一个 factorplot
    g = sns.factorplot('high','low',data=data, kind="bar",palette="muted", legend=False)
    
    # 展示图像
    plt.show()

![png](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142704603-1159058930..png)

## 3、配置

### 3.1、在seaborn中使用matplotlib的默认值

    
    
    # Import Matplotlib
    import matplotlib.pyplot as plt
    
    # 查看所有可用的样式
    plt.style.available
    
    # 使用matplotlib的默认值
    plt.style.use("classic")
    
    
    ['bmh',
     'classic',
     'dark_background',
     'fast',
     'fivethirtyeight',
     'ggplot',
     'grayscale',
     'seaborn-bright',
     'seaborn-colorblind',
     'seaborn-dark-palette',
     'seaborn-dark',
     'seaborn-darkgrid',
     'seaborn-deep',
     'seaborn-muted',
     'seaborn-notebook',
     'seaborn-paper',
     'seaborn-pastel',
     'seaborn-poster',
     'seaborn-talk',
     'seaborn-ticks',
     'seaborn-white',
     'seaborn-whitegrid',
     'seaborn',
     'Solarize_Light2',
     'tableau-colorblind10',
     '_classic_test']

### 3.2、在matplotlib中使用seaborn的颜色作为色彩

可以使用color_palette（）来定义要使用的颜色映射和参数n_colors的颜色数

    
    
    # 导入相关模块
    import seaborn as sns
    import matplotlib.pyplot as plt
    import numpy as np
    from matplotlib.colors import ListedColormap
    
    # 定义一个变量N
    N = 500
    
    # 构建 colormap
    current_palette = sns.color_palette("muted", n_colors=5)
    cmap = ListedColormap(sns.color_palette(current_palette).as_hex())
    
    # 初始化数据
    data1 = np.random.randn(N)
    data2 = np.random.randn(N)
    # 产生随机数标签
    colors = np.random.randint(0,5,N)
    
    # 创建散点图图表
    plt.scatter(data1, data2, c=colors, cmap=cmap)
    
    # 添加颜色条
    plt.colorbar()
    
    # 展示图像
    # plt.show()
    
    
    <matplotlib.colorbar.Colorbar at 0x23f39041828>

![png](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142704875-1233906640..png)

### 3.3、在seaborn中旋转标签文本

    
    
    # Import the necessary libraries
    import matplotlib.pyplot as plt
    import seaborn as sns 
    import numpy as np
    import pandas as pd
    
    # 初始化数据
    x = 10 ** np.arange(1, 10)
    y = x * 2
    data = pd.DataFrame(data={'x': x, 'y': y})
    
    # 创建 lmplot
    grid = sns.lmplot('x', 'y', data, size=7, truncate=True, scatter_kws={"s": 100})
    
    # 在X轴上旋转标签
    grid.set_xticklabels(rotation=90)
    
    # 展示图像
    # plt.show()
    
    
    <seaborn.axisgrid.FacetGrid at 0x23f39090b38>

![png](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142705254-497712785..png)

