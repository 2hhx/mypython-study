[TOC]

# time模块

# 1 time模块的导入

```python
import time
```

## 1.1 时间戳

时间戳(timestamp)：时间戳表示的是从1970年1月1日00:00:00开始按秒计算的偏移量。

```python
import time
print(time.time())


# 1566559582.0275092

```

## 1.2 格式化时间

格式化的时间字符串(format string)：格式化时间表示的是普通的字符串格式的时间。

```python
import time
print(time.strftime("%Y-%m-%d %X"))
# 
2019-08-23 19:27:33

```

## 1.3 结构化时间

结构化的时间(struct time)：struct_time元组共有9个元素共九个元素，分别为(年，月，日，时，分，秒，一年中第几周，一年中第几天，夏令时)

```python
# 本地时区
import time
print(time.localtime())
# time.struct_time(tm_year=2019, tm_mon=8, tm_mday=23, tm_hour=19, tm_min=29, tm_sec=0, tm_wday=4, tm_yday=235, tm_isdst=0)
```



```python

# UTC
#协调世界时，又称世界统一时间、世界标准时间、国际协调时间。由于英文（CUT）和法文（TUC）的缩写不同，作为妥协，简称UTC
#以本初子午线的平子夜起算的平太阳时。又称格林尼治平时或格林尼治时间
import time
print(time.gmtime())
#
time.struct_time(tm_year=2019, tm_mon=8, tm_mday=23, tm_hour=11, tm_min=29, tm_sec=50, tm_wday=4, tm_yday=235, tm_isdst=0)
```



```python
#结构化时间的基准时间（开始时间）
import time
print(time.localtime(0))
#
time.struct_time(tm_year=1970, tm_mon=1, tm_mday=1, tm_hour=8, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=1, tm_isdst=0)
```



```python
#
import time
print(time.gmtime(0))
#
time.struct_time(tm_year=1970, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=1, tm_isdst=0)
```



```python
# 结构化时间的基准时间上增加一年时间
import time
print(time.localtime(3600*24*365))
#
time.struct_time(tm_year=1971, tm_mon=1, tm_mday=1, tm_hour=8, tm_min=0, tm_sec=0, tm_wday=4, tm_yday=1, tm_isdst=0)
```

## 1.4 不同格式时间的转换

 如上图所示，我们总能通过某些方法在结构化时间-格式化时间-时间戳三者之间进行转换，下面我们将用代码展示如何通过这些方法转换时间格式。

![](https://img2018.cnblogs.com/blog/1739658/201908/1739658-20190823193730566-1171915046.jpg)

```python
#结构化时间
import time
now_time = time.localtime()
print(now_time)
#time.struct_time(tm_year=2019, tm_mon=8, tm_mday=23, tm_hour=19, tm_min=39, tm_sec=7, tm_wday=4, tm_yday=235, tm_isdst=0)
```



```python
# 把结构化时间转换成时间戳格式
print(time.mktime(now_time))
#

1566560347.0
```



```python
# 把结构化时间转换为格式化时间
# %Y年-%m月-%d天 %X时分秒=%H时:%M分:%S秒
import time
now_time = time.localtime()
print(time.strftime("%Y-%m-%d %X",now_time))
#
2019-08-23 19:41:35


```



```python
# 把格式化时间化为结构化时间，它和strftime()是逆操作
import time
print(time.strptime("2019-08-23 19:41:35","%Y-%m-%d %X"))

#
time.struct_time(tm_year=2019, tm_mon=8, tm_mday=23, tm_hour=19, tm_min=41, tm_sec=35, tm_wday=4, tm_yday=235, tm_isdst=-1)

```



```python
# 把结构化时间表示为这种形式：'Sun Jun 20 23:21:05 1993'。
import time
print(time.asctime())
#
Fri Aug 23 19:46:21 2019

```



```python
#如果没有参数，将会将time.localtime()作为参数传入。
import time
print(time.asctime(time.localtime()))
#
Fri Aug 23 19:46:21 2019
```



```python
# 把一个时间戳转化为time.asctime()的形式。
import time
print(time.ctime())
#
Fri Aug 23 19:49:18 2019

```



```python
# 如果参数未给或者为None的时候，将会默认time.time()为参数。它的作用相当于
import time
print(time.ctime(time.time()))
#
Fri Aug 23 19:49:18 2019

```

##  

