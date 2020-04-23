# ipywidgets

ipywidgets可以用于在jupyter notebook当中进行界面设计，以及一些简单的交互式控件操作。

官方文档有详细介绍，本文主要将常用的部件进行了演示，如需详细研究，请移步官方文档[ipywidgets](https://ipywidgets.readthedocs.io/en/stable/#)

## 一、安装

    
    
    pip install ipywidgets

## 二、基础方法

### 1、滑块interact

interact方法可以实现一些基础的交互式控件,可以自动生成函数参数的UI控件。

    
    
    from ipywidgets import interact,fixed
    
    # 定义一个可供操作的函数
    def foo(x):
        return x

> 当传递一个参数x=10,会生成一个滑块并且绑定到函数参数
    
    
    interact(foo,x=10)

> 传递布尔值，生成复选框
    
    
    interact(foo,x=True)

> 传递字符串，生成文本框
    
    
    interact(foo,x="hello world")

> 传递列表，生成下拉框
    
    
    interact(foo,x=['a','b','c','d'])

> 传递字典，生成下拉框，键值对应
    
    
    interact(foo,x={"a":1,"b":2,"c":3})

> 传递元组，（min,max,step）最小值，最大值，步长
    
    
    interact(foo,x=(1,9,1))

补充：还可以使用浮点数，生成浮点数滑块。

以上方法都是将一个参数生成为特定值，当需要多个参数时就需要用到 **`fixed`** 参数。

    
    
    def func(p,q):
        return (p,q)
    
    interact(func,p=5,q=fixed(20))  # 设定20为固定值

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162910410-219579520..png)

### 2、按钮

    
    
    import ipywidgets as widgets
    from ipywidgets import Layout

#### 1、button（普通按钮）

    
    
    widgets.Button(
        description='点我啊！！！',  # 按钮提示
        disabled=False,
        button_style='info', # 'success', 'info', 'warning', 'danger' or '' 按钮样式
        tooltip='Click me',
        layout = Layout(width="98%",height="50px"),  # 按钮大小调整
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162910760-745593926..png)

#### 2、ToggleButton（布尔值按钮）

用于显示布尔值

    
    
    widgets.ToggleButton(
        value=False,
        description='点我！！',
        disabled=False,
        button_style='warning', # 'success', 'info', 'warning', 'danger' or ''
        tooltip='Description',
        icon='check',
        layout = Layout(width="60%",height='30px')
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162910926-1730418147..png)

#### 3、RadioButtons（单选按钮）

    
    
    widgets.RadioButtons(
        options=['numpy', 'pandas', 'matplotlib'],
    #     value='pineapple',
        description='',
        disabled=False
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162911106-1191803236..png)

#### 4、ToggleButtons

    
    
    widgets.ToggleButtons(
        options=['numpy', 'pandas', 'matplotlib'],
        description='Speed:',
        disabled=False,
        button_style='success', # 'success', 'info', 'warning', 'danger' or ''
    #     tooltips=['Description of slow', 'Description of regular', 'Description of fast'],
    #     icons=['check'] * 3
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162911257-755858135..png)

### 3、选择小部件

1、Dropdown（下拉框）

    
    
    widgets.Dropdown(
        options=['1', '2', '3'],
        value='2',
        description='Number:',
        disabled=False,
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162911404-738109735..png)

2、Select（单选框）

    
    
    widgets.Select(
        options=['Linux', 'Windows', 'OSX'],
        value='OSX',
        # rows=10,
        description='OS:',
        disabled=False
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162911531-2084917003..png)

3、SelectionSlider（滑动部件）

    
    
    widgets.SelectionSlider(
        options=['low level', 'ordinary', 'well', 'excellent'],
        value='ordinary',
        description='我的Python等级',
        disabled=False,
        continuous_update=False,
        orientation='horizontal',
        readout=True
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162911667-1840963799..png)

4、SelectMultiple（复选框）

    
    
    widgets.SelectMultiple(
        options=['Apples', 'Oranges', 'Pears'],
        value=['Oranges'],
        #rows=10,
        description='Fruits',
        disabled=False
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162911792-923247524..png)

### 3、进度条

#### 1、IntProgress (整数型进度条)

    
    
    widgets.IntProgress(
        value=5,  # 进度条数值
        min=0,
        max=10,
        step=1,
        description='Loading:',
        bar_style='danger', # 'success', 'info', 'warning', 'danger' or ''
        orientation='horizontal'
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162911961-1545378071..png)

#### 2、FloatProgress (浮点型进度条)

    
    
    widgets.FloatProgress(
        value=5.5,
        min=0,
        max=10.0,
        step=0.1,
        description='Loading:',
        bar_style='warning',
        orientation='horizontal'
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162912089-2103159994..png)

### 4、文本

#### 1、Text(固定大小)

    
    
    widgets.Text(
        value='Hello World',
        placeholder='Type something',
        description='String:',
        disabled=False
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162912217-1256518387..png)

#### 2、Textarea(可拉伸)

    
    
    widgets.Textarea(
        value='Hello World',
        placeholder='Type something',
        description='String:',
        disabled=False
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162912354-1422844495..png)

#### 3、BoundedIntText、BoundedFloatText（限值文本）

    
    
    # 整数
    widgets.BoundedIntText(
        value=7,
        min=0,
        max=10,
        step=1,
        description='Text:',
        disabled=False
    )
    # 浮点数
    widgets.BoundedFloatText(
        value=7.5,
        min=0,
        max=10.0,
        step=0.1,
        description='Text:',
        disabled=False
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162912497-1211897219..png)

### 5、图片

    
    
    file = open("小宝贝.jpg", "rb")
    image = file.read()
    widgets.Image(
        value=image,
        format='jpg',
        width=300,
        height=400,
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162912635-691453610..png)

### 6、日期选择器

    
    
    widgets.DatePicker(
        description='Pick a Date',
        disabled=False
    )

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162912790-1273614766..png)

### 7、容器/布局小部件

#### 1、Box

用于显示组件当中的多个小部件

    
    
    words = ['correct', 'horse', 'battery', 'staple']
    items = [Button(description=w) for w in words]
    Box([items[0], items[1], items[2], items[3]])

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162912921-1659051797..png)

#### 2、HBox

水平显示组件中多个小部件

    
    
    words = ['correct', 'horse', 'battery', 'staple']
    items = [Button(description=w) for w in words]
    HBox([items[0], items[1], items[2], items[3]])

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162913052-617928746..png)

#### 3、VBox

垂直显示组件中的多个小部件

    
    
    words = ['correct', 'horse', 'battery', 'staple']
    items = [Button(description=w) for w in words]
    VBox([items[0], items[1], items[2], items[3]])

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162913211-1951378843..png)

#### 4、折叠数据

    
    
    accordion = widgets.Accordion(children=[widgets.IntSlider(), widgets.Text()])
    accordion.set_title(0, '滑块')
    accordion.set_title(1, '文本')
    accordion

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162913349-1161741047..png)

#### 5、标签

    
    
    tab_contents = ['基本', '股池', '买策', '卖策', '选股']
    children = [widgets.Text(description=name) for name in tab_contents]
    tab = widgets.Tab()
    tab.children = children
    for index,i in enumerate(tab_contents):
        tab.set_title(index,i)  # 需要索引与值两个参数
    tab

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162913488-549672716..png)

#### 6、折叠数据与标签嵌套

    
    
    tab_nest = widgets.Tab()
    tab_nest.children = [accordion, accordion]
    tab_nest.set_title(0, '第一块')
    tab_nest.set_title(1, '第二块')
    tab_nest

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162913644-1855457743..png)

### 8、播放动画小部件

    
    
    # 播放动画结合进度条
    play = widgets.Play(
    #     interval=10,
        value=0,
        min=0,
        max=100,
        step=1,
        description="Press play",
        disabled=False
    )
    
    slider = widgets.IntProgress(
        value=100,# 进度条数值
        min=0,
        max=100,
        step=1,
        description='Loading:',
        bar_style='danger', # 'success', 'info', 'warning', 'danger' or ''
        orientation='horizontal'
    )
    
    widgets.jslink((play, 'value'), (slider, 'value'))
    widgets.HBox([play, slider])

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162913798-232051683..png)

## 三、常用事件

### 1、on_click()

    
    
    # 查看某个部件的文档
    print(widgets.Button.on_click.__doc__)

按钮点击是无状态发生的，因此想要将事件由前端传递到后端，就需要通过`on_click`方法，在点击时执行相应的函数：

    
    
    button = widgets.Button(description="点我啊！！")
    display(button)
    
    # 点击执行事件，点击一次，执行一次
    def on_button_clicked(b):
        print("Button clicked.")
    
    button.on_click(on_button_clicked)

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162913930-210843976..png)

### 2、Traitlet事件

#### observe

observe方法可以用于注册回调

    
    
    int_range = widgets.IntSlider()
    display(int_range)
    
    # 每一次事件都会回调当前函数，然后打印`change['new']`中对应的值
    def on_value_change(change):
        print(change['new'])
    
    int_range.observe(on_value_change, names='value')

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191014162914084-1564155079..png)

