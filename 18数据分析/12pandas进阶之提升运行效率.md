# 前言

如果你现在正在学习数据分析，或者正在从事数据分析行业，肯定会处理一些大数据集。pandas就是这些大数据集的一个很好的处理工具。那么pandas到底是什么呢？官方文档上说：

> " **快速** ， **灵活** ，富有表现力的数据结构，旨在使”关系“或”标记“数据的使用既 **简单** 又 **直观** 。"

快速、灵活、简单、直观！这些听起来感觉很棒。如果你的工作涉及到构建复杂的数据模型，你肯定不希望花费大量的开发时间等待模块处理大数据集。我们需要将大量的时间与精力放在解释数据当中，而不是使用那些功能较少的工具，为了处理数据而煞费苦心。

# Pandas处理数据慢？

在使用的pandas的过程中有人说，虽然他是一个很好的解析数据的工具，但是因为它的速度太慢了，无法作为统计建模工具。对于初学者在自己的使用当中可能会发现，它的运行速度，并不符合一个数据分析工具的标准。

但是Pandas的开发是建立在Numpy的数组结构之上的，它的许多操作都是通过C语言实现的，基于Numpy和Pandas自己的拓展模块来编写的，这些模块是Cpython编写的，编译成C语言。这样来看，pandas的速度肯定快的。

事实证明，肯定是，但是你必须正确的使用它！

本文不是讲如何过度优化Pandas的代码，而是讲如何正确的使用它，主要介绍几种pandas中常用到的方法，对于这些方法的使用存在哪些需要注意的问题，以及如何对它们进行速度上的提升。

  * 讲datetime数据与时间序列一起使用的优点
  * 进行批量计算的最有效的途径
  * 通过HDFStore存储数据节省时间

# 使用datetime数据节省时间

    
    
    import pandas as pd
    from timer import timeit
    import time
    import numpy as np
    
    
    pd.__version__
    
    
    '0.24.2'
    
    
    df = pd.read_csv('demand_profile.csv')
    df.head()

| date_time | energy_kwh  
---|---|---  
0 | 1/1/13 0:00 | 0.586  
1 | 1/1/13 1:00 | 0.580  
2 | 1/1/13 2:00 | 0.572  
3 | 1/1/13 3:00 | 0.596  
4 | 1/1/13 4:00 | 0.592  
  
从运行上面的代码得到的结果来看，好像没有任何问题。但是实际上pandas和numpy都有一个dtypes的概念。如果每天指定的话，那么date_time将会使用一个object的dtype类型，如下：

    
    
    df.dtypes
    
    
    date_time      object
    energy_kwh    float64
    dtype: object
    
    
    type(df.iat[0,0])
    
    
    str

object类型就像一个大的容器，不仅仅可以承载str类型，也可以包含那些不能很好地融进一个数据类型的任何特征列。而如果我们将日期作为str类型就会极大的影响效率。  
因此针对时间序列的数据而言，我们需要将date_time列的格式转换为datetime对象数组（pandas称之为Timestap）。具体操作如下：

    
    
    df['date_time'] = pd.to_datetime(df['date_time'])
    print(df['date_time'].dtype)
    df['date_time'].head()
    
    
    datetime64[ns]
    
    0   2013-01-01 00:00:00
    1   2013-01-01 01:00:00
    2   2013-01-01 02:00:00
    3   2013-01-01 03:00:00
    4   2013-01-01 04:00:00
    Name: date_time, dtype: datetime64[ns]

以上就是date_time列转换类型的操作以及转换后的效果。

接下来我们自定义一个`@timeit`装饰器进行测试，它可以返回函数的运行结果并从多次实验中打印它的平均运行时间。

    
    
    @timeit(repeat = 3, number = 10)
    def convert(df, column_name):
        return pd.to_datetime(df[column_name])
    
    df['date_time'] = convert(df, 'date_time')
    
    
    Best of 3 trials with 10 function calls per trial:  
    Function `convert` ran in average of 0.785 seconds.

实际运行速度0.785s，看上去非常快了，但是其实还可以更快

    
    
    @timeit(repeat = 3, number = 10)
    def convert_with_format(df, column_name):
        return pd.to_datetime(df[column_name],format="%d/%m/%y %H:%M")
    
    df['date_time'] = convert_with_format(df, 'date_time')
    
    
    Best of 3 trials with 10 function calls per trial:  
    Function `convert` ran in average of 0.022 seconds.

结果只有0.022s，快了将近35倍。原因是：我们设置了转化的格式format。由于在CSV中的datetimes并不是ISO
8601格式的，如果不进行设置的话，那么pandas将使用dateutil包把每个字符串str转化成date日期。  
相反，如果原始数据datetime已经是ISO
8601格式了，那么pandas就可以立即使用最快速的方法来解析日期。这也就是为什么提前设置好格式format可以提升这么多的速度

# Pandas数据的循环操作

仍然基于上面的数据，我们再添加一个新的特征，但是这个新的特征是基于一些时间条件，根据时长而变化，如下：

    
    
    data = pd.DataFrame({'Tariff Type':['Peak','Shoulder','Off-Peak'],
                'Cents per kWh':['28','20','12'],
                'Time Range':['17:00 to 24:00','7:00 to 17:00','0:00 to 7:00']})
    data

| Tariff Type | Cents per kWh | Time Range  
---|---|---|---  
0 | Peak | 28 | 17:00 to 24:00  
1 | Shoulder | 20 | 7:00 to 17:00  
2 | Off-Peak | 12 | 0:00 to 7:00  
  
因此，按照我们正常的做法就是使用apply方法写一个函数，函数里面写好时间条件的逻辑代码

    
    
    def apply_tariff(kwh, hour):
        """
        计算每个小时的电费
        """
        if 0 <= hour < 7:
            rate = 12
        elif 7 <= hour < 17:
            rate = 20
        elif 17 <= hour < 24:
            rate = 28
        else:
            raise ValueError(f"Invalid hour:{hour}")
        return rate * kwh

通过for循环来遍历df，根据apply函数逻辑添加新的特征，如下：

    
    
    @timeit(repeat = 3, number = 100)
    def apply_tariff_loop(df):
        """
        循环计算，修改df的特征
        """
        energy_cost_list = []
        for i in range(len(df)):
            energy_used = df.iloc[i]['energy_kwh']
            hour = df.iloc[i]['date_time'].hour
            energy_cost = apply_tariff(energy_used, hour)
            energy_cost_list.append(energy_cost)
        df['cost_cents'] = energy_cost_list
    
    apply_tariff_loop(df)
    
    
    Best of 3 trials with 100 function calls per trial:
    Function `apply_tariff_loop` ran in average of 2.291 seconds.

对于那些经常使用python编程的coder来说，这个设计看起来非常的自然。但是这个循环会严重影响效率，你要是做数据分析，肯定是不赞成这么做的

  * 1、它需要初始化一个将记录输出的列表
  * 2、它使用不透明对象范围`(0,len(df))`循环，然后在应用apply_tariff()之后，它必须将结果附加到用于创建新DataFrame列的列表中。它还使用df.iloc [i] ['date_time']执行所谓的链式索引，这通常会导致意外的结果。
  * 3、这种方法最大的问题是计算的时间成本。对于八千多行的数据，使用循环花了2.29秒钟。

# 使用itertuples()和iterrows()循环

可能有些同学看到这两个方法有些熟悉，我们通过pandas导入itertuples和iterrows方法可以使效率更快。这些都是一次产生一行的生产器方法，类似与python生成器中的yield用法

.itertuples为每一行产生一个namedtuple，并且行的索引值作为元组的第一个元素。nametuple是Python的collections模块中的一种数据结构，其行为类似于Python元组，但具有可通过属性查找访问的字段。

.iterrows为DataFrame中的每一行产生（index，series）这样的元组。

虽然.itertuples往往会更快一些，但是在这个例子中使用.iterrows，我们看看这使用iterrows后效果如何。

    
    
    @timeit(repeat = 3, number = 100)
    def apply_tariff_iterrows(df):
        energy_cost_list = []
        for index, row in df.iterrows():
             # 获取用电量和时间（小时）
            energy_used = row['energy_kwh']
            hour = row['date_time'].hour
            # 添加cost列表
            energy_cost = apply_tariff(energy_used, hour)
            energy_cost_list.append(energy_cost)
        df['cost_cents'] = energy_cost_list
    
    apply_tariff_iterrows(df)
    
    
    Best of 3 trials with 100 function calls per trial:
    Function `apply_tariff_iterrows` ran in average of 0.674 seconds.

语法方面：这种类型的语法更加明确，并且行值引用中的混乱更少，因此它更具有可读性

时间收益方面：快了将近3.5倍，但是我们还有改进的空间。因为我们依然在使用python的for循环，这就意味着每个函数调用都是在python中完成的，理想情况是它可以用pandas内部架构中内置的更快的语言完成

# Pandas的.apply()方法

使用.apply方法而不是.iterrows进一步改进此操作。Pandas的.apply方法接受函数(callables)并沿DataFrame的轴(所有行或所有列)应用它们。在此示例中，lambda函数将帮助你将两列数据传递给apply_tariff()：

    
    
    @timeit(repeat = 3, number = 100)
    def apply_tariff_withapply(df):
        df['cost_cents'] = df.apply(
            lambda row:apply_tariff(
                kwh = row['energy_kwh'],
                hour = row['date_time'].hour),
                axis = 1)
    apply_tariff_withapply(df)
    
    
    Best of 3 trials with 100 function calls per trial:
    Function `apply_tariff_withapply` ran in average of 0.141 seconds.

.apply的语法优点很明显，行数少，代码可读性高。在这种情况下，所花费的时间大约是.iterrows方法的五分之一。

但是，这还不是“非常快”。一个原因是.apply()将在内部尝试循环遍历Cython迭代器。但是在这种情况下，传递的lambda不是可以在Cython中处理的东西，因此它在Python中调用，因此并不是那么快。

如果你使用.apply()获取10年的小时数据，那么你将需要大约15分钟的处理时间。如果这个计算只是大型模型的一小部分，那么你真的应该加快速度。这也就是矢量化操作派上用场的地方。

# 矢量化操作：使用.isin()选择数据

矢量化操作这个东西是学习numpy、pandas比较基础的一个知识点，在这边就不做过多介绍了。  
它是pandas中执行的最快方法。

    
    
    # df.set_index('date_time',inplace=True)
    
    @timeit(repeat=3,number=100)
    def apply_tariff_isin(df):
        peak_hours = df.index.hour.isin(range(17,24))
        shoulder_hours = df.index.hour.isin(range(7,17))
        off_peak_hours = df.index.hour.isin(range(0,7))
        
        df.loc[peak_hours, 'cost_cents'] = df.loc[peak_hours, 'energy_kwh'] * 28
        df.loc[shoulder_hours, 'cost_cents'] = df.loc[shoulder_hours, 'energy_kwh'] * 20
        df.loc[off_peak_hours, 'cost_cents'] = df.loc[off_peak_hours, 'energy_kwh'] * 12
        
    apply_tariff_isin(df)
    
    
    Best of 3 trials with 100 function calls per trial:
    Function `apply_tariff_isin` ran in average of 0.003 seconds.

isin()方法返回的是一个布尔值数组，如下所示：

    
    
    [False, False, False, ...,True ,True ,True]

这些值标识哪些DataFrame索引(datetimes)落在指定的小时范围内。然后，当你将这些布尔数组传递给DataFrame的.loc索引器时，你将获得一个仅包含与这些小时匹配的行的DataFrame切片。在那之后，仅仅是将切片乘以适当的费率，这是一种快速的矢量化操作。

这与我们上面的循环操作相比如何？首先，你可能会注意到不再需要apply_tariff()，因为所有条件逻辑都应用于行的选择。因此，你必须编写的代码行和调用的Python代码会大大减少。

处理时间怎么样？比不是Pythonic的循环快764倍，比.iterrows快224倍，比.apply快47倍。

# 咱们还能做的更好吗

其实通过以上的对比，我们会发现它的时间效率进步是非常大的，但是这就完了吗？

在apply_tariff_isin中，我们仍然可以通过调用df.loc和df.index.hour.isin三次来进行一些“手动工作”。如果我们有更精细的时隙范围，你可能会争辩说这个解决方案是不可扩展的。幸运的是，在这种情况下，你可以使用Pandas的pd.cut()函数进行离散化分箱，以编程方式执行更多操作：

    
    
    @timeit(repeat=3, number=100)
    def apply_tariff_cut(df):
        cents_per_kwh = pd.cut(x=df.index.hour,
                               bins=[0, 7, 17, 24],
                               include_lowest=True,
                               labels=[12, 20, 28]).astype(int)
        df['cost_cents'] = cents_per_kwh * df['energy_kwh']
    
    apply_tariff_cut(df)
    
    
    Best of 3 trials with 100 function calls per trial:
    Function `apply_tariff_cut` ran in average of 0.001 seconds.

到目前为止，时间上基本已经快要到达极限了，只花费了0.001秒的时间来处理完整的十年的小时数据集。最后一个选项是使用 NumPy
函数来操作每个DataFrame的底层NumPy数组，然后将结果集成回Pandas数据结构中。

# 结合numpy继续加速

我们在玩pandas的时候肯定不能忘记的一点就是，它是基于numpy进行编写的，它最常用的两种数据结构`DataFrame`和`Series`都是在numpy库之上设计的。这使得我们计算更加灵活，因为pandas可以和numpy的矩阵和操作无缝衔接

接下来，我们将使用NumPy的 digitize()
函数。它类似于Pandas的cut()，因为数据将被分箱，但这次它将由一个索引数组表示，这些索引表示每小时所属的bin。然后将这些索引应用于价格数组：

    
    
    @timeit(repeat=3, number=100)
    def apply_tariff_digitize(df):
        prices = np.array([12, 20, 28])
        bins = np.digitize(df.index.hour.values, bins=[7, 17, 24])
        df['cost_cents'] = prices[bins] * df['energy_kwh'].values
        
    apply_tariff_digitize(df)
    
    
    Best of 3 trials with 100 function calls per trial:
    Function `apply_tariff_digitize` ran in average of 0.001 seconds.

与pandas的cut()函数一样，这种语法非常简洁易读。

运行后时间显示依然是0.001s，其实性能是有提升的，只不过时间显示的问题。使用pandas，可以帮助维持`层次结构`,如果你想的话，可以像在此处一样进行批量计算，

  * 使用向量化操作：没有for循环的Pandas方法和函数。
  * 将.apply方法：与可调用方法一起使用。
  * 使用.itertuples：从Python的集合模块迭代DataFrame行作为namedTuples。
  * 使用.iterrows：迭代DataFrame行作为(index，Series)对。虽然Pandas系列是一种灵活的数据结构，但将每一行构建到一个系列中然后访问它可能会很昂贵。
  * 使用“element-by-element”循环：使用df.loc或df.iloc一次更新一个单元格或行。

> 上面的优先顺序是pandas开发人员的建议

# 使用HDFStore防止重新处理

现在你已经了解了Pandas中的加速数据流程，接着让我们探讨如何避免与最近集成到Pandas中的HDFStore一起重新处理时间。

通常，在构建复杂数据模型时，可以方便地对数据进行一些预处理。例如，如果您有10年的分钟频率耗电量数据，即使你指定格式参数，只需将日期和时间转换为日期时间可能需要20分钟。你真的只想做一次，而不是每次运行你的模型，进行测试或分析。

你可以在此处执行的一项非常有用的操作是预处理，然后将数据存储在已处理的表单中，以便在需要时使用。但是，如何以正确的格式存储数据而无需再次重新处理？如果你要另存为CSV，则只会丢失datetimes对象，并且在再次访问时必须重新处理它。

Pandas有一个内置的解决方案，它使用 HDF5，这是一种专门用于存储表格数据阵列的高性能存储格式。 Pandas的 HDFStore
类允许你将DataFrame存储在HDF5文件中，以便可以有效地访问它，同时仍保留列类型和其他元数据。它是一个类似字典的类，因此您可以像读取Python
dict对象一样进行读写。

以下是将预处理电力消耗DataFrame df存储在HDF5文件中的方法：

    
    
    # 创建储存对象，并存为 processed_data
    data_store = pd.HDFStore('processed_data.h5')
    
    # 将 DataFrame 放进对象中，并设置 key 为 preprocessed_df
    data_store['preprocessed_df'] = df
    data_store.close()

现在，你可以关闭计算机并休息一下。等你回来的时候，你处理的数据将在你需要时为你所用，而无需再次加工。以下是如何从HDF5文件访问数据，并保留数据类型：

    
    
    # 获取数据储存对象
    data_store = pd.HDFStore('processed_data.h5')
    
    # 通过key获取数据
    preprocessed_df = data_store['preprocessed_df']
    data_store.close()

数据存储可以容纳多个表，每个表的名称作为键。

关于在Pandas中使用HDFStore的注意事项：您需要安装PyTables> =
3.0.0，因此在安装Pandas之后，请确保更新PyTables，如下所示：

    
    
    # pip install --upgrade tables

# 结论

如果你觉得你的Pandas项目不够快速，灵活，简单和直观，请考虑重新考虑你使用该库的方式。

这里探讨的示例相当简单，但说明了Pandas功能的正确应用如何能够大大改进运行时和速度的代码可读性。以下是一些经验，可以在下次使用Pandas中的大型数据集时应用这些经验法则：

尝试尽可能使用矢量化操作，而不是在df 中解决for
x的问题。如果你的代码是许多for循环，那么它可能更适合使用本机Python数据结构，因为Pandas会带来很多开销。  
如果你有更复杂的操作，其中矢量化根本不可能或太难以有效地解决，请使用.apply方法。  
如果必须循环遍历数组（确实发生了这种情况），请使用.iterrows()或.itertuples()来提高速度和语法。  
Pandas有很多可选性，几乎总有几种方法可以从A到B。请注意这一点，比较不同方法的执行方式，并选择在项目环境中效果最佳的路线。  
一旦建立了数据清理脚本，就可以通过使用HDFStore存储中间结果来避免重新处理。  
将NumPy集成到Pandas操作中通常可以提高速度并简化语法。

> 参考:<https://realpython.com/fast-flexible-pandas/>

