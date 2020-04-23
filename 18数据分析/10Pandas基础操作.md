# Pandas

* 什么是Pandas
* pandas能干什么
* 怎么用pandas 
  * Series
  * DataFrame
  * 时间对象处理
  * 数据分组和聚合
  * 其他常用方法

## 1、什么是Pandas

当大家谈论到数据分析时，提及最多的语言就是Python和SQL，而Python之所以适合做数据分析，就是因为他有很多强大的第三方库来协助，pandas就是其中之一，它是基于Numpy构建的，正因pandas的出现，让Python语言也成为使用最广泛而且强大的数据分析环境之一。如果说没有pandas的出现，目前的金融数据分析领域还应该是R语言的天下。

## 2、Pandas能干什么

Pandas的主要功能：

* 具备对应其功能的数据结构DataFrame，Series
* 集成时间序列功能
* 提供丰富的数学运算和操作
* 灵活处理缺失数据
* .....

以上就是pandas能完成的一些基础操作，当然并不完全，下面就来看看pandas到底是怎么用的。





## 3、怎么用Pandas

安装方法：

> pip install pandas

引用方法：

> import pandas as pd

### 3.1、Series

Series是一种类似于一维数组的对象，由一组数据和一组与之相关的数据标签(索引)组成。在数据分析的过程中非常常用。



#### 3.1.1、创建方法

```python
第一种：
pd.Series([4,5,6,7,8])
执行结果：
0    4
1    5
2    6
3    7
4    8
dtype: int64
# 将数组索引以及数组的值打印出来，索引在左，值在右，由于没有为数据指定索引，于是会自动创建一个0到N-1（N为数据的长度）的整数型索引，取值的时候可以通过索引取值，跟之前学过的数组和列表一样
-----------------------------------------------
第二种：
pd.Series([4,5,6,7,8],index=['a','b','c','d','e'])
执行结果：
a    4
b    5
c    6
d    7
e    8
dtype: int64
# 自定义索引，index是一个索引列表，里面包含的是字符串，依然可以通过默认索引取值。
-----------------------------------------------
第三种：
pd.Series({"a":1,"b":2})
执行结果：
a    1
b    2
dtype: int64
# 指定索引
-----------------------------------------------
第四种：
pd.Series(0,index=['a','b','c'])
执行结果：
a    0
b    0
c    0
dtype: int64
# 创建一个值都是0的数组
-----------------------------------------------

对于Series，其实我们可以认为它是一个长度固定且有序的字典，因为它的索引和数据是按位置进行匹配的，像我们会使用字典的上下文，就肯定也会使用Series
```





#### 3.1.2、缺失数据

* dropna() # 过滤掉值为NaN的行
* fillna() # 填充缺失数据
* isnull() # 返回布尔数组，缺失值对应为True
* notnull() # 返回布尔数组，缺失值对应为False



```python
* # 第一步，创建一个字典，通过Series方式创建一个Series对象

  st = {"sean":18,"yang":19,"bella":20,"cloud":21}
  obj = pd.Series(st)
  obj
  运行结果：
  sean     18
  yang     19
  bella    20
  cloud    21

  ## dtype: int64

  # 第二步

  ## a = {'sean','yang','cloud','rocky'}  # 定义一个索引变量

  #第三步
  obj1 = pd.Series(st,index=a)
  obj1  # 将第二步定义的a变量作为索引传入

  # 运行结果：

  rocky     NaN
  cloud    21.0
  sean     18.0
  yang     19.0
  dtype: float64

  # 因为rocky没有出现在st的键中，所以返回的是缺失值

通过上面的代码演示，对于缺失值已经有了一个简单的了解，接下来就来看看如何判断缺失值


1、
obj1.isnull()  # 是缺失值返回Ture
运行结果：
rocky     True
cloud    False
sean     False
yang     False
dtype: bool

2、
obj1.notnull()  # 不是缺失值返回Ture
运行结果：
rocky    False
cloud     True
sean      True
yang      True
dtype: bool

3、过滤缺失值 # 布尔型索引
obj1[obj1.notnull()]
运行结果：
cloud    21.0
yang     19.0
sean     18.0
dtype: float64
```





#### 3.1.3、Series特性

```python
import numpy as np
import pandas as pd

* 从ndarray创建Series:Series(arr)

  

  arr = np.arange(10)

  sr = pd.Series(arr)  # ndarray创建Series

* 与标量（数字）进行运算：sr * 2

  

  srx = sr * 2  # 与标量（数字）进行运算

* 两个Series运算

  

  sr * srx  # 两个Series运算

* 布尔值过滤：sr[sr>0]

  

  sr[sr>3]  # 布尔值过滤

* 统计函数：mean()、sum()、cumsum()

# 统计函数

sr.mean()
sr.sum()
sr.cumsum()
```







#### 3.1.4、支持字典的特性

```python
* 从字典创建Series：Series(dic),

  

  dic = {"A":1,"B":2,"C":3,"D":4,"E":5}

  # 字典创建Series

  dic_arr = pd.Series(dic)  

* In运算：'a'in sr、for x in sr

  

  ## "A" in dic_arr

  for i in dic_arr: 
      print(i)

* 键索引：sr['a'],sr[['a','b','d']]

  

  dic_arr[['A','B']]  # 键索引

* 键切片：sr['a':'c']

  

  dic_arr['A':'C']  # 键切片

* 其他函数：get('a',default=0)等

  

  dic_arr.get("A",default=0)  
```





#### 3.1.5、整数索引

```python
pandas当中的整数索引对象可能会让初次接触它的人很懵逼，接下来通过代码演示：


sr = pd.Series(np.arange(10))
sr1 = sr[3:].copy()
sr1
运行结果：
3    3
4    4
5    5
6    6
7    7
8    8
9    9
dtype: int32
# 到这里会发现很正常，一点问题都没有，可是当使用整数索引取值的时候就会出现问题了。因为在pandas当中使用整数索引取值是优先以标签解释的，而不是下标
sr1[1]

解决方法：

* loc属性 # 以标签解释

* iloc属性 # 以下标解释

  

  sr1.iloc[1]  # 以下标解释

  sr1.loc[3]  # 以标签解释
```





#### 3.1.6、Series数据对齐

```python
pandas在运算时，会按索引进行对齐然后计算。如果存在不同的索引，则结果的索引是两个操作数索引的并集。


sr1 = pd.Series([12,23,34], index=['c','a','d'])
sr2 = pd.Series([11,20,10], index=['d','c','a',])
sr1 + sr2
运行结果:
a    33
c    32
d    45
dtype: int64
# 可以通过这种索引对齐直接将两个Series对象进行运算
sr3 = pd.Series([11,20,10,14], index=['d','c','a','b'])
sr1 + sr3
运行结果：
a    33.0
b     NaN
c    32.0
d    45.0
dtype: float64
# sr1 和 sr3的索引不一致，所以最终的运行会发现b索引对应的值无法运算，就返回了NaN,一个缺失值

将两个Series对象相加时将缺失值设为0：


sr1 = pd.Series([12,23,34], index=['c','a','d'])
sr3 = pd.Series([11,20,10,14], index=['d','c','a','b'])
sr1.add(sr3,fill_value=0)
运行结果：
a    33.0
b    14.0
c    32.0
d    45.0
dtype: float64
# 将缺失值设为0，所以最后算出来b索引对应的结果为14

灵活的算术方法:add,sub,div,mul

到这里可能就会说了pandas就这么简单吗，那我们接下来一起看看这个二维数组DataFraeme


```



### 3.2、DataFrame

DataFrame是一个表格型的数据结构，相当于是一个二维数组，含有一组有序的列。他可以被看做是由Series组成的字典，并且共用一个索引。接下来就一起来见识见识DataFrame数组的厉害吧！！！



#### 3.2.1、创建方式

```python
创建一个DataFrame数组可以有多种方式，其中最为常用的方式就是利用包含等长度列表或Numpy数组的字典来形成DataFrame：


第一种：
pd.DataFrame({'one':[1,2,3,4],'two':[4,3,2,1]})
# 产生的DataFrame会自动为Series分配所索引，并且列会按照排序的顺序排列
运行结果：
one two
0   1   4
1   2   3
2   3   2
3   4   1

> 指定列
可以通过columns参数指定顺序排列
data = pd.DataFrame({'one':[1,2,3,4],'two':[4,3,2,1]})
pd.DataFrame(data,columns=['one','two'])

# 打印结果会按照columns参数指定顺序


第二种：
pd.DataFrame({'one':pd.Series([1,2,3],index=['a','b','c']),'two':pd.Series([1,2,3],index=['b','a','c'])})
运行结果：
   one  two
a   1   2
b   2   1
c   3   3

以上创建方法简单了解就可以，因为在实际应用当中更多是读数据，不需要自己手动创建


```



#### 3.2.2、查看数据

```
常用属性和方法：

* index 获取行索引

* columns 获取列索引

* T 转置

* values 获取值索引

* describe 获取快速统计

  

  one    two
  a   1   2
  b   2   1
  c   3   3
```



# 这样一个数组df

------

df.index

> Index(['a', 'b', 'c'], dtype='object')

------

df.columns

> Index(['one', 'two'], dtype='object')

------

df.T

> a  b   c
> one 1   2   3
> two 2   1   3

------

df.values

> array([[1, 2],
>        [2, 1],
>        [3, 3]], dtype=int64)

------

df.describe()

> one two
> count   3.0 3.0   # 数据统计
> mean    2.0 2.0   # 平均值
> std     1.0 1.0   # 标准差
> min     1.0 1.0   # 最小值
> 25%     1.5 1.5   # 四分之一均值
> 50%     2.0 2.0   # 二分之一均值
> 75%     2.5 2.5   # 四分之三均值
> max     3.0 3.0   # 最大值



#### 3.2.3、索引和切片

```
* DataFrame有行索引和列索引。
* DataFrame同样可以通过标签和位置两种方法进行索引和切片。

DataFrame使用索引切片：

* 方法1：两个中括号，无论是先取行还是先取列都可以。

  

  import tushare as ts

  data = ts.get_k_data("000001")

  data['open'][:10]  # 先取列再取行
  data[:10]['open']  # 先取行再取列

* 方法2（推荐）：使用loc/iloc属性，一个中括号，逗号隔开，先取行再取列。 

  * loc属性：解释为标签
  * iloc属性：解释为下标

  

  data.loc[:10,"open":"low"]  # 用标签取值

  data.iloc[:10,1:5]  # 用下标取值

* 向DataFrame对象中写入值时只使用方法2

* 行/列索引部分可以是常规索引、切片、布尔值索引、花式索引任意搭配。（注意：两部分都是花式索引时结果可能与预料的不同）
```





### 3.3、时间对象处理

处理时间对象可能是我们在进行数据分析的过程当中最常见的，我们会遇到各种格式的时间序列，也需要处理各种格式的时间序列，但是一定不能对这些数据懵逼，我们需要找到最合适的招式来处理这些时间。接下来就一起来看吧！！！



#### 3.3.1、时间序列类型

* 时间戳：特定时刻
* 固定时期：如2019年1月
* 时间间隔：起始时间-结束时间



#### 3.3.2、Python库：datatime

```python
* date、datetime、timedelta

  

  import datetime

  # datetime.date()  # date对象

  today = datetime.date.today()  # 获取当天日期，返回date对象
  t1 = datetime.date(2019,4,18) # 设置时间格式为datetime.date

  # datetime.datetime()  # datetime对象

  now = datetime.datetime.now()  # 获取当天日期，返回datetime对象
  t2 = datetime.datetime(2019,4,18) # 设置时间格式为datetime.datetime

  # datetime.timedelta()  # 时间差

  today = datetime.datetime.today()
  yestoday = today - datetime.timedelta(1)  # 以时间做运算
  print(today,yestoday)

* dt.strftime()

  

  # 将时间格式转换为字符串

  today.strftime("%Y-%m-%d")
  yestoday.strftime("%Y-%m-%d")

* strptime()

  

  # 将日期字符串转成datetime时间格式，第二个参数由时间格式决定

  datetime.datetime.strptime('2019-04-18','%Y-%m-%d')
```





#### 3.3.3、灵活处理时间对象：dateutil包

```python
dateutil.parser.parse()



import dateutil

# 将字符串格式的日期转换为datetime对象

date = '2019 Jan 2nd'
t3 = dateutil.parser.parse(date)
```





#### 3.3.4、成组处理时间对象：pandas

```python
* pd.to_datetime(['2018-01-01', '2019-02-02'])

将字符串转换为为时间对象


from datetime import datetime
import pandas as pd

date1 = datetime(2019, 4, 18, 12, 24, 30)
date2 = '2019-04-18'
t4 = pd.to_datetime(date1)
t5 = pd.to_datetime(date2)
t6 = pd.to_datetime([date1,date2])

t4,t5,t6  # 转换单个时间数据是返回Timestamp对象，转换多个时间数据返回DatetimeIndex对象
> 
"""
(Timestamp('2019-04-18 12:24:30'),
 Timestamp('2019-04-18 00:00:00'),
 DatetimeIndex(['2019-04-18 12:24:30', '2019-04-18 00:00:00'], dtype='datetime64[ns]', freq=None))
"""

将索引设置为时间序列


ind = pd.to_datetime(['2018-03-01','2019 Feb 3','08/12-/019'])
sr = pd.Series([1,2,3],index=ind)

# 补充：
"""
pd.to_datetime(['2018-03-01','2019 Feb 3','08/12-/019']).to_pydatetime()

> array([datetime.datetime(2018, 3, 1, 0, 0),
       datetime.datetime(2019, 2, 3, 0, 0),
       datetime.datetime(2019, 8, 12, 0, 0)], dtype=object)
# 通过to_pydatetime()方法将其转换为ndarray数组
"""
```





#### 3.3.5、产生时间对象数组：data_range

* start 开始时间

* end 结束时间

* periods 时间长度

* freq 时间频率，默认为'D'，可选H(our),W(eek),B(usiness),S(emi-)M(onth),(min)T(es), S(econd), A(year),…

  

  pd.date_range("2019-1-1","2019-2-2",freq="D")

  > DatetimeIndex(['2019-01-01', '2019-01-02', '2019-01-03', '2019-01-04',
  >                '2019-01-05', '2019-01-06', '2019-01-07', '2019-01-08',
  >                '2019-01-09', '2019-01-10', '2019-01-11', '2019-01-12',
  >                '2019-01-13', '2019-01-14', '2019-01-15', '2019-01-16',
  >                '2019-01-17', '2019-01-18', '2019-01-19', '2019-01-20',
  >                '2019-01-21', '2019-01-22', '2019-01-23', '2019-01-24',
  >                '2019-01-25', '2019-01-26', '2019-01-27', '2019-01-28',
  >                '2019-01-29', '2019-01-30', '2019-01-31', '2019-02-01',
  >                '2019-02-02'],
  >               dtype='datetime64[ns]', freq='D')



#### 3.3.6、时间序列

时间序列就是以时间对象为索引的Series或DataFrame。datetime对象作为索引时是存储在DatetimeIndex对象中的。

```
    
# 转换时间索引
dt = pd.date_range("2019-01-01","2019-02-02")
# 生成一个带有时间数据的DataFrame数组
a = pd.DataFrame({"num":pd.Series(random.randint(-100,100) for _ in range(30)),"date":dt})
# 通过index修改索引
a.index = pd.to_datetime(a["date"])

特殊功能：

* 传入“年”或“年月”作为切片方式

* 传入日期范围作为切片方式

* 丰富的函数支持：resample(), strftime(), ……

  

  a.resample("3D").mean()  # 计算每三天的均值
  a.resample("3D").sum()  #  计算每三天的和
  ...



 时间序列的处理在数据分析当中非常重要，但是有时候时间的格式不一致又会让人非常烦躁，只要把以上秘籍都学会就可以把时间序列制得服服帖帖。
```





### 3.4、数据分组和聚合

 在数据分析当中，我们有时需要将数据拆分，然后在每一个特定的组里进行运算，这些操作通常也是数据分析工作中的重要环节。  
分组聚合相对来说也是一个稍微难理解的点，需要各位有一定的功力来学习.



#### 3.4.1、分组(GroupBY机制)

pandas对象（无论Series、DataFrame还是其他的什么）当中的数据会根据提供的一个或者多个键被拆分为多组，拆分操作实在对象的特定轴上执行的。就比如DataFrame可以在他的行上或者列上进行分组，然后将一个函数应用到各个分组上并产生一个新的值。最后将所有的执行结果合并到最终的结果对象中。

![](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142406842-2091101199..png)

分组键可以是多种样式，并且键不一定是完全相同的类型：

* 列表或者数组，长度要与待分组的轴一样
* 表示DataFrame某个列名的值。
* 字典或Series，给出待分组轴上的值与分组名之间的对应关系
* 函数，用于处理轴索引或者索引中的各个标签

后三种只是快捷方式，最终仍然是为了产生一组用于拆分对象的值。

首先，通过一个很简单的DataFrame数组尝试一下：

```
 
df = pd.DataFrame({'key1':['x','x','y','y','x',                               
            'key2':['one','two','one',',two','one'],
            'data1':np.random.randn(5),
            'data2':np.random.randn(5)})
df
                           
>   key1    key2    data1   data2
0   x   one 0.951762    1.632336
1   x   two -0.369843   0.602261
2   y   one 1.512005    1.331759
3   y   two 1.383214    1.025692
4   x   one -0.475737   -1.182826

访问data1，并根据key1调用groupby：


f1 = df['data1'].groupby(df['key1'])
f1

> <pandas.core.groupby.groupby.SeriesGroupBy object at 0x00000275906596D8>

上述运行是没有进行任何计算的，但是我们想要的中间数据已经拿到了，接下来，就可以调用groupby进行任何计算


f1.mean()  # 调用mean函数求出平均值
> key1
x    0.106183
y    2.895220
Name: data1, dtype: float64

以上数据经过分组键（一个Series数组）进行了聚合，产生了一个新的Series，索引就是`key1`列中的唯一值。这些索引的名称就为`key1`。接下来就尝试一次将多个数组的列表传进来


f2 = df['data1'].groupby([df['key1'],df['key2']])
f2.mean()
> key1  key2
x     one     0.083878
      two     0.872437
y     one    -0.665888
      two    -0.144310
Name: data1, dtype: float64

传入多个数据之后会发现，得到的数据具有一个层次化的索引，key1对应的x\y;key2对应的one\two.


f2.mean().unstack()  # 通过unstack方法就可以让索引不堆叠在一起了

> key2  one     two
key1        
x   0.083878    0.872437
y   -0.665888   -0.144310
```



补充：

* 1、分组键可以是任意长度的数组

* 2、分组时，对于不是数组数据的列会从结果中排除，例如key1、key2这样的列

* 3、GroupBy的size方法，返回一个含有分组大小的Series

  

  # 以上面的f2测试

  f2.size()

  > key1  key2
  > x     one     2
  >       two     1
  > y     one     1
  >       two     1
  > Name: data1, dtype: int64

到这跟着我上面的步骤一步一步的分析，会发现还是很简单的，但是一定不能干看，还要自己下手练，只有多练才能融汇贯通！！！



#### 3.4.2、聚合(组内应用某个函数)

聚合是指任何能够从数组产生标量值的数据转换过程。刚才上面的操作会发现使用GroupBy并不会直接得到一个显性的结果，而是一个中间数据，可以通过执行类似mean、count、min等计算得出结果，常见的还有一些:

| 函数名      | 描述                          |
| ----------- | ----------------------------- |
| sum         | 非NA值的和                    |
| median      | 非NA值的算术中位数            |
| std、var    | 无偏（分母为n-1）标准差和方差 |
| prod        | 非NA值的积                    |
| first、last | 第一个和最后一个非NA值        |

> 自定义聚合函数

不仅可以使用这些常用的聚合运算，还可以自己自定义。

```
# 使用自定义的聚合函数，需要将其传入aggregate或者agg方法当中

def peak_to_peak(arr):
    return arr.max() - arr.min()
f1.aggregate(peak_to_peak)
运行结果：
key1
x    3.378482
y    1.951752
Name: data1, dtype: float64

多函数聚合：

​    
f1.agg(['mean','std'])
运行结果：
​    mean    std
key1        
x   -0.856065   0.554386
y   -0.412916   0.214939

最终得到的列就会以相应的函数命名生成一个DataFrame数组
```





以上我们是可以通过agg或者是aggregate来实现多函数聚合以及自定义聚合函数，但是一定要注意agg方法只能进行聚合操作，进行其他例如：排序，这些方法是会报错的。agg返回的是数据的标量，所以有些时候并不适合使用agg，这就要看我们接下来的操作了。

#### 3.4.3、apply

 GroupBy当中自由度最高的方法就是apply，它会将待处理的对象拆分为多个片段，然后各个片段分别调用传入的函数，最后将它们组合到一起。

> df.apply(  
> ['func', 'axis=0', 'broadcast=None', 'raw=False', 'reduce=None',
> 'result_type=None', 'args=()', '**kwds']

func:传入一个自定义函数  
axis:函数传入参数当axis=1就会把一行数据作为Series的数据

```
# 分析欧洲杯和欧洲冠军联赛决赛名单
import pandas as pd

url="https://en.wikipedia.org/wiki/List_of_European_Cup_and_UEFA_Champions_League_finals"
eu_champions=pd.read_html(url)  # 获取数据
a1 = eu_champions[2]    # 取出决赛名单
a1.columns = a1.loc[0]  # 使用第一行的数据替换默认的横向索引
a1.drop(0,inplace=True)  # 将第一行的数据删除
a1.drop('#',axis=1,inplace=True)  # 将以#为列名的那一列删除
a1.columns=['Season', 'Nation', 'Winners', 'Score', 'Runners_up', 'Runners_up_Nation', 'Venue','Attendance']  # 设置列名

    a1.tail()  # 查看后五行数据
    a1.drop([64,65],inplace=True)  # 删除其中的缺失行以及无用行
    a1
```



运行结果：

![分组apply1](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142409394-782915292..png)

现在想根据分组选出`Attendance`列中值最高的三个。

```
    
# 先自定义一个函数
def top(df,n=3,column='Attendance'):
    return df.sort_values(by=column)[-n:]
top(a1,n=3)
```



运行结果：

![分组apply2](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142409591-1633825267..png)

接下来，就对a1分组并且使用apply调用该函数：

​    
a1.groupby('Nation').apply(top)

![分组apply3](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012142412382-815455510..png)

运行之后会发现，我们通过这个操作将每个国家各个年份时段出席的人数的前三名进行了一个提取。

以上top函数是在DataFrame的各个片段上调用，然后结果又通过pandas.concat组装到一起，并且以分组名称进行了标记。

以上只是基本用法，apply的强大之处就在于传入函数能做什么都由自己说了算，它只是返回一个pandas对象或者标量值就行



### 3.5、其他常用方法

> pandas常用方法（适用Series和DataFrame）

* mean(axis=0,skipna=False)
* sum(axis=1)
* sort_index(axis, …, ascending) # 按行或列索引排序
* sort_values(by, axis, ascending) # 按值排序
* apply(func, axis=0) # 将自定义函数应用在各行或者各列上，func可返回标量或者Series
* applymap(func) # 将函数应用在DataFrame各个元素上
* map(func) # 将函数应用在Series各个元素上



# pandas方法汇总

## 一、生成数据表

```python
1、首先导入pandas库，一般都会用到numpy库，所以我们先导入备用：
import numpy as np
import pandas as pd

2、导入CSV或者xlsx文件：
df = pd.DataFrame(pd.read_csv(‘name.csv’,header=1))
df = pd.DataFrame(pd.read_excel(‘name.xlsx’))

或者
import pandas as pd
from collections import namedtuple

Item = namedtuple(‘Item’, ‘reply pv’)
items = []

with codecs.open(‘reply.pv.07’, ‘r’, ‘utf-8’) as f:
for line in f:
line_split = line.strip().split(’\t’)
items.append(Item(line_split[0].strip(), line_split[1].strip()))

df = pd.DataFrame.from_records(items, columns=[‘reply’, ‘pv’])

3、用pandas创建数据表：
df = pd.DataFrame({"id":[1001,1002,1003,1004,1005,1006], 
 "date":pd.date_range('20130102', periods=6),
  "city":['Beijing ', 'SH', ' guangzhou ', 'Shenzhen', 'shanghai', 'BEIJING '],
 "age":[23,44,54,32,34,32],
 "category":['100-A','100-B','110-A','110-C','210-A','130-F'],
  "price":[1200,np.nan,2133,5433,np.nan,4432]},
  columns =['id','date','city','category','age','price'])
1
2
3
4
5
6
7
```



## 二、数据表信息查看

1、维度查看：
df.shape

2、数据表基本信息（维度、列名称、数据格式、所占空间等）：
df.info()

3、每一列数据的格式：
df.dtypes

4、某一列格式：
df[‘B’].dtype

5、空值：
df.isnull()

6、查看某一列空值：
df.isnull()

7、查看某一列的唯一值：
df[‘B’].unique()

8、查看数据表的值：
df.values

9、查看列名称：
df.columns

10、查看前10行数据、后10行数据：
df.head() #默认前10行数据
df.tail() #默认后10 行数据

## 三、数据表清洗

1、用数字0填充空值：
df.fillna(value=0)

2、使用列prince的均值对NA进行填充：
df[‘prince’].fillna(df[‘prince’].mean())

3、清楚city字段的字符空格：
df[‘city’]=df[‘city’].map(str.strip)

4、大小写转换：
df[‘city’]=df[‘city’].str.lower()

5、更改数据格式：
df[‘price’].astype(‘int’)

6、更改列名称：
df.rename(columns={‘category’: ‘category-size’})

7、删除后出现的重复值：
df[‘city’].drop_duplicates()

8 、删除先出现的重复值：
df[‘city’].drop_duplicates(keep=‘last’)

9、数据替换：
df[‘city’].replace(‘sh’, ‘shanghai’)

## 四、数据预处理

```
df1=pd.DataFrame({"id":[1001,1002,1003,1004,1005,1006,1007,1008], 
"gender":['male','female','male','female','male','female','male','female'],
"pay":['Y','N','Y','Y','N','Y','N','Y',],
"m-point":[10,12,20,40,40,40,30,20]})
1
2
3
4
1、数据表合并
1.1 merge
df_inner=pd.merge(df,df1,how='inner')  # 匹配合并，交集
df_left=pd.merge(df,df1,how='left')        #
df_right=pd.merge(df,df1,how='right')
df_outer=pd.merge(df,df1,how='outer')  #并集
1
2
3
4
1.2 append
result = df1.append(df2)
1

1.3 join
result = left.join(right, on='key')
1

1.4 concat
pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,
          keys=None, levels=None, names=None, verify_integrity=False,
          copy=True)
1
2
3
objs︰ 一个序列或系列、 综合或面板对象的映射。如果字典中传递，将作为键参数，使用排序的键，除非它传递，在这种情况下的值将会选择 （见下文）。任何没有任何反对将默默地被丢弃，除非他们都没有在这种情况下将引发 ValueError。
axis: {0，1，…}，默认值为 0。要连接沿轴。
join: {‘内部’、 ‘外’}，默认 ‘外’。如何处理其他 axis(es) 上的索引。联盟内、 外的交叉口。
ignore_index︰ 布尔值、 默认 False。如果为 True，则不要串联轴上使用的索引值。由此产生的轴将标记 0，…，n-1。这是有用的如果你串联串联轴没有有意义的索引信息的对象。请注意在联接中仍然受到尊重的其他轴上的索引值。
join_axes︰ 索引对象的列表。具体的指标，用于其他 n-1 轴而不是执行内部/外部设置逻辑。
keys︰ 序列，默认为无。构建分层索引使用通过的键作为最外面的级别。如果多个级别获得通过，应包含元组。
levels︰ 列表的序列，默认为无。具体水平 （唯一值） 用于构建多重。否则，他们将推断钥匙。
names︰ 列表中，默认为无。由此产生的分层索引中的级的名称。
verify_integrity︰ 布尔值、 默认 False。检查是否新的串联的轴包含重复项。这可以是相对于实际数据串联非常昂贵。
副本︰ 布尔值、 默认 True。如果为 False，请不要，不必要地复制数据。

例子：1.frames = [df1, df2, df3]
2.result = pd.concat(frames)

2、设置索引列
df_inner.set_index(‘id’)

3、按照特定列的值排序：
df_inner.sort_values(by=[‘age’])

4、按照索引列排序：
df_inner.sort_index()

5、如果prince列的值>3000，group列显示high，否则显示low：
df_inner[‘group’] = np.where(df_inner[‘price’] > 3000,‘high’,‘low’)

6、对复合多个条件的数据进行分组标记
df_inner.loc[(df_inner[‘city’] == ‘beijing’) & (df_inner[‘price’] >= 4000), ‘sign’]=1

7、对category字段的值依次进行分列，并创建数据表，索引值为df_inner的索引列，列名称为category和size
pd.DataFrame((x.split(’-’) for x in df_inner[‘category’]),index=df_inner.index,columns=[‘category’,‘size’]))

8、将完成分裂后的数据表和原df_inner数据表进行匹配
df_inner=pd.merge(df_inner,split,right_index=True, left_index=True)
```



## 五、数据提取

```python
主要用到的三个函数：loc,iloc和ix，loc函数按标签值进行提取，iloc按位置进行提取，ix可以同时按标签和位置进行提取。

1、按索引提取单行的数值
df_inner.loc[3]

2、按索引提取区域行数值
df_inner.iloc[0:5]

3、重设索引
df_inner.reset_index()

4、设置日期为索引
df_inner=df_inner.set_index(‘date’)

5、提取4日之前的所有数据
df_inner[:‘2013-01-04’]

6、使用iloc按位置区域提取数据
df_inner.iloc[:3,:2] #冒号前后的数字不再是索引的标签名称，而是数据所在的位置，从0开始，前三行，前两列。

7、适应iloc按位置单独提起数据
df_inner.iloc[[0,2,5],[4,5]] #提取第0、2、5行，4、5列

8、使用ix按索引标签和位置混合提取数据
df_inner.ix[:‘2013-01-03’,:4] #2013-01-03号之前，前四列数据

9、判断city列的值是否为北京
df_inner[‘city’].isin([‘beijing’])

10、判断city列里是否包含beijing和shanghai，然后将符合条件的数据提取出来
df_inner.loc[df_inner[‘city’].isin([‘beijing’,‘shanghai’])]

11、提取前三个字符，并生成数据表
pd.DataFrame(category.str[:3])
```



## 六、数据筛选

```python
使用与、或、非三个条件配合大于、小于、等于对数据进行筛选，并进行计数和求和。

1、使用“与”进行筛选
df_inner.loc[(df_inner[‘age’] > 25) & (df_inner[‘city’] == ‘beijing’), [‘id’,‘city’,‘age’,‘category’,‘gender’]]

2、使用“或”进行筛选
df_inner.loc[(df_inner[‘age’] > 25) | (df_inner[‘city’] == ‘beijing’), [‘id’,‘city’,‘age’,‘category’,‘gender’]].sort([‘age’])

3、使用“非”条件进行筛选
df_inner.loc[(df_inner[‘city’] != ‘beijing’), [‘id’,‘city’,‘age’,‘category’,‘gender’]].sort([‘id’])

4、对筛选后的数据按city列进行计数
df_inner.loc[(df_inner[‘city’] != ‘beijing’), [‘id’,‘city’,‘age’,‘category’,‘gender’]].sort([‘id’]).city.count()

5、使用query函数进行筛选
df_inner.query(‘city == [“beijing”, “shanghai”]’)

6、对筛选后的结果按prince进行求和
df_inner.query(‘city == [“beijing”, “shanghai”]’).price.sum()
```



## 七、数据汇总

```python
主要函数是groupby和pivote_table

1、对所有的列进行计数汇总
df_inner.groupby(‘city’).count()

2、按城市对id字段进行计数
df_inner.groupby(‘city’)[‘id’].count()

3、对两个字段进行汇总计数
df_inner.groupby([‘city’,‘size’])[‘id’].count()

4、对city字段进行汇总，并分别计算prince的合计和均值
df_inner.groupby(‘city’)[‘price’].agg([len,np.sum, np.mean])
```



## 八、数据统计

```
数据采样，计算标准差，协方差和相关系数

1、简单的数据采样
df_inner.sample(n=3)

2、手动设置采样权重
weights = [0, 0, 0, 0, 0.5, 0.5]
df_inner.sample(n=2, weights=weights)

3、采样后不放回
df_inner.sample(n=6, replace=False)

4、采样后放回
df_inner.sample(n=6, replace=True)

5、 数据表描述性统计
df_inner.describe().round(2).T #round函数设置显示小数位，T表示转置

6、计算列的标准差
df_inner[‘price’].std()

7、计算两个字段间的协方差
df_inner[‘price’].cov(df_inner[‘m-point’])

8、数据表中所有字段间的协方差
df_inner.cov()

9、两个字段的相关性分析
df_inner[‘price’].corr(df_inner[‘m-point’]) #相关系数在-1到1之间，接近1为正相关，接近-1为负相关，0为不相关

10、数据表的相关性分析
df_inner.corr()
```



## 九、数据输出

```
分析后的数据可以输出为xlsx格式和csv格式

1、写入Excel
df_inner.to_excel(‘excel_to_python.xlsx’, sheet_name=‘bluewhale_cc’)

2、写入到CSV
df_inner.to_csv(‘excel_to_python.csv’)
```

