[TOC]



# 一、json和pickle模块

## 1.1 序列化

把对象(变量)从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling，在其他语言中也被称之为serialization，marshalling，flattening。

序列化的优点：

1. 持久保存状态：内存是无法永久保存数据的，当程序运行了一段时间，我们断电或者重启程序，内存中关于这个程序的之前一段时间的数据（有结构）都被清空了。但是在断电或重启程序之前将程序当前内存中所有的数据都保存下来（保存到文件中），以便于下次程序执行能够从文件中载入之前的数据，然后继续执行，这就是序列化。
2. 跨平台数据交互：序列化时不仅可以把序列化后的内容写入磁盘，还可以通过网络传输到别的机器上，如果收发的双方约定好实用一种序列化的格式，那么便打破了平台/语言差异化带来的限制，实现了跨平台数据交互。

## 1.2 json

json模块：

1.  用python写了一个程序，用java写了一门程序，这两个程序需要数据之间交流，规定了一种多种语言通用的数据类型
2. json串:
   1. 序列化：从python的字典（最常用）变成json串， dump
   2.  反序列化：从json串变成python的字典（最常用），load

Json序列化并不是python独有的，json序列化在java等语言中也会涉及到，因此使用json序列化能够达到跨平台传输数据的目的。

json数据类型和python数据类型对应关系表

|  Json类型  | Python类型 |
| :--------: | :--------: |
|     {}     |    dict    |
|     []     |    list    |
|  "string"  |    str     |
|   520.13   | int或float |
| true/false | True/False |
|    null    |    None    |

json模块序列化和反序列化的一个过程如下图所示

![json模块](http://www.chenyoude.com/Python%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E6%94%BE%E5%BC%83/json%E6%A8%A1%E5%9D%97.jpg?x-oss-process=style/watermark)

```python
# 序列化
with open('Json序列化对象.json', 'w') as fw:
    json.dump(struct_data, fw)
```

```python
# 反序列化
with open('Json序列化对象.json') as fr:
    data = json.load(fr)
print(data)
{'name': 'json', 'age': 23, 'sex': 'male'}
```



```python
import json
struct_data = {'name': 'json', 'age': 23, 'sex': 'male'}
print(struct_data, type(struct_data))

#
{'name': 'json', 'age': 23, 'sex': 'male'} <class 'dict'>
```



```python
data = json.dumps(struct_data)
print(data, type(data))
#
{"name": "json", "age": 23, "sex": "male"} <class 'str'>
```



```python
# 注意：无论数据是怎样创建的，只要满足json格式（如果是字典，则字典内元素都是双引号），就可以json.loads出来,不一定非要dumps的数据才能loads
data = json.loads(data)
print(data, type(data))
#
{'name': 'json', 'age': 23, 'sex': 'male'} <class 'dict'>
```



## 1.3 pickle

Pickle序列化和所有其他编程语言特有的序列化问题一样，它只能用于Python，并且可能不同版本的Python彼此都不兼容，因此，只能用Pickle保存那些不重要的数据，即不能成功地反序列化也没关系。但是pickle的好处是可以存储Python中的所有的数据类型，包括对象，而json不可以。

pickle模块序列化和反序列化的过程如下图所示

![pickle模块](http://www.chenyoude.com/Python%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E6%94%BE%E5%BC%83/pickle%E6%A8%A1%E5%9D%97.jpg?x-oss-process=style/watermark)



```python
# 序列化(注意：pickle模块需要使用二进制存储，即'wb'模式存储)
with open('Pickle序列化对象.pkl', 'wb') as fw:
    pickle.dump(struct_data, fw)
```





```python
# 反序列化
with open('Pickle序列化对象.pkl', 'rb') as fr:
    pickle = pickle.load(fr)
print(data)
#
{'name': 'json', 'age': 23, 'sex': 'male'}
```

```python
import pickle
struct_data = {'name': 'json', 'age': 23, 'sex': 'male'}
print(struct_data, type(struct_data))
#
{'name': 'json', 'age': 23, 'sex': 'male'} <class 'dict'>
```



```python
data = pickle.dumps(struct_data)
print(data, type(data))
#
b'\x80\x03}q\x00(X\x04\x00\x00\x00nameq\x01X\x04\x00\x00\x00jsonq\x02X\x03\x00\x00\x00ageq\x03K\x17X\x03\x00\x00\x00sexq\x04X\x04\x00\x00\x00maleq\x05u.' <class 'bytes'>
```



```python
data = pickle.loads(data)
print(data, type(data))
#
{'name': 'json', 'age': 23, 'sex': 'male'} <class 'dict'>
```

