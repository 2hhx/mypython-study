

[TOC]

# datetime模块

```python
# datetime模块可以看成是时间加减的模块
import datetime
```

```python
# 返回当前时间
import datetime

print(datetime.datetime.now())
#  
2019-08-23 19:53:53.742850

```

```python
#输出是那一天
import datetime
import time
print(datetime.date.fromtimestamp(time.time()))


# 
2019-08-23

```

```python
#当天时间加三日
import datetime
import time
print(datetime.datetime.now()+datetime.timedelta(3))
#
2019-08-26 19:56:13.500102

```

```python
#当天时间减三天
import datetime
import time
print(datetime.datetime.now()+datetime.timedelta(-3))
# 
2019-08-20 19:57:18.572635

```



```python
# 当天时间减3小时
import datetime
import time
print(datetime.datetime.now()+datetime.timedelta(hours=3))
#
2019-08-23 22:58:41.294881

        
```





```python
# 时间替换
import datetime
import time
c_time = datetime.datetime.now()
print(c_time.replace(minute=20,hour=5,second=13))
#
2019-08-23 05:20:13.829892

```

