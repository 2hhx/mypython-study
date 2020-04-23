[TOC]

# 101 numpy模块

# numpy简介

numpy官方文档：https://docs.scipy.org/doc/numpy/reference/?v=20190307135750

numpy是python的一种开源的数值计算扩展库。这种库可用来存储和处理大型的numpy数组，比python自身的嵌套列表结构要高效的多（这个结构也可以用来表示numpy数组）

numpy库有俩个作用：

1. 区别于list列表，提供了数组操作，数组的运算，以及统计分布和简单的数学模型。
2. 计算速度快，甚至要由于python内置的简单运算，使得其成为pandas、sklearn等模块的依赖包。高级的框架如TensorFlow、Pytorch等，其数组操作也和numpy非常相似。

# 二、为什么用numpy

```python
lis1 = [1,2,3]
lis2 = [4,5,6]
```

如果想让`lis1*lis2`得到一个结果为`lis = [4,10,18]`，这是非常复杂的。

# 三、创建numpy数组

numpy数组即numpy的ndarry对象，创建numpy数组就是把一个列表传入np.array()方法。



```python
import numpy as np
# np.array?相当于pycharm的ctrl+鼠标左键


import numpy as np
arr = np.array([1,2,3])
print(arr,type(arr))


#输出：
[1 2 3] <class 'numpy.ndarray'>

```



```python
#创建一个二维的ndarray对象
import numpy as np
print(np.array([[1,2,3],[4,5,6]]))


#输出：
[[1 2 3]
 [4 5 6]]

```

==三维的不使用numpy模块,使用tensorflow/pytorch模块==



```python
#创建一个三维的ndarray对象
import numpy as np
arr = [[[1,2,3],[4,5,6],[7,8,9]],[[1,2,3],[4,5,6],[7,8,9]]]
print(np.array(arr))
print(np.array(arr).shape)
print(np.array(arr).ndim)

#输出：
[[[1 2 3]
  [4 5 6]
  [7 8 9]]

 [[1 2 3]
  [4 5 6]
  [7 8 9]]]
(2, 3, 3)
3#数组的维数：3维

```

| 属性   | 解释                           |
| ------ | ------------------------------ |
| T      | 数组的装置(对高维数组而言)     |
| dtype  | 数组元素的数据类型             |
| size   | 数组元素的个数                 |
| ndim   | 数组的维数                     |
| shape  | 数组的维度大小（以元组的形式） |
| astype | 类型转换                       |

dtype种类：bool_,int(8,16,32,64),float(16,32,64)

```python
import numpy as np
arr = np.array([[1,2,3],[4,5,6]],dtype=np.float32)
print(arr)
#输出：
[[1. 2. 3.]
 [4. 5. 6.]]

```





```python
import numpy as np
arr = np.array([[1,2,3],[4,5,6]],dtype=np.float32)
print(arr.T)


#输出：
[[1. 4.]
 [2. 5.]
 [3. 6.]]
```



```python
import numpy as np
arr = np.array([[1,2,3],[4,5,6]],dtype=np.float32)
print(arr.dtype)
#输出：
float32

```



```python
import numpy as np
arr = np.array([[1,2,3],[4,5,6]],dtype=np.float32)
arr = arr.astype(np.int32)
print(arr.dtype)
print(arr)


#输出：
int32
[[1 2 3]
 [4 5 6]]
```



```python
import numpy as np
arr = np.array([[1,2,3],[4,5,6]],dtype=np.float32)
arr = arr.astype(np.int32)

print(arr.size)

print(arr.ndim)


print(arr.shape)


#输出：
6
2
(2, 3)

```

# 五、获取numpy数组的行列数

由于numpy数组是多维的，对于二维数组而言，numpy数组就是即有行又有列。

注意：对于numpy我们一般多讨论二维的数组。



```python
import numpy as np
arr = np.array([[1,2,3],[4,5,6]])
print(arr)

#输出：
[[1 2 3]
 [4 5 6]]
```



```python
#获取num数组的行和列构成的数组
import numpy as np
arr = np.array([[1,2,3],[4,5,6]])
print(arr.shape)
#获取numpy数组的行
print(arr.shape[0])
#获取numpy数组的列
print(arr.shape[1])


#输出：

(2, 3)
2
3


```

# 六、切割numpy数组

切分numpy数组类似于列表的切割，但是与列表的切割不同的是，numpy数组的切割涉及到行和列的切割，但是俩者切割的方式都是从索引0开始，并且取头不取尾。



```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr)
#输出：
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]
```



```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr[:,:])###取出全部

#输出：
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]

```



```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr[:1,:])#取出第一行
#输出：
[[1 2 3 4]]


```



```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr[:,:1])#取出第一列
#输出：
[[1]
 [5]
 [9]]
```



```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr[0,0])#取出第一行第一列
#输出：
1
```



```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr[2,1])#取出第3行第2个
#输出：
10
```



```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
# print(arr)
print(arr[arr>5])#取出大于5的数返回一个数组
#输出：
[ 6  7  8  9 10 11 12]

```



```python
#numpy数组按运算符取元素的原理，即通过arr>5生成一个布尔numpy数组
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr)
print(arr>5)#numpy数组按运算符取元素的原理，即通过arr>5生成一个布尔numpy数组
print(arr[arr>5])#将True返回
#输出：
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]


[[False False False False]
 [False  True  True  True]
 [ True  True  True  True]]

[ 6  7  8  9 10 11 12]

```

```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr)
print(arr[0:1,0:1])#第一行第一个
#[[1]]
print(arr[0:1,:1])#第一行第一个
#[[1]]
print(arr[0:1,:])#第一行
#[[1 2 3 4]]

print(arr[0:1,0:])#第一行
#[[1 2 3 4]]
print(arr[1:,0:])#第二、三行
# [[ 5  6  7  8]
#  [ 9 10 11 12]]

# arr[开始行:结束行,行开始数:结束数]都是索引


```

```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr)
print(arr[0, [0,2]])#第一行的第一个数和第三个数

#输出：
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]
[1 3]

```

```python
import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr)
print(arr[0, 0] + 1)#对取出的值进行加1
#输出：
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]
2
```



# 七、numpy数组元素的替换(修改值)

numpy数组元素的替换，类似于列表元素的替换，并且numpy数组也是一个可变类型的数据，即如果numpy数组进行替换操作，会修改原numpy数组的元素，所以下面我们用.copy()方法举例numpy数组元素的替换。

```python

import numpy as np
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr)
#输出：
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]

```



```python
# 取第一行的所有元素，并且让第一行的元素都为0
import numpy as np
arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
arr1 = arr.copy()
arr1[:1,:] = 0
print(arr1)

#
[[ 0  0  0  0]
 [ 5  6  7  8]
 [ 9 10 11 12]]
```



```python
#取所有大于5 的元素，并且让大于5 的元素为0
arr2 = arr.copy()
arr2[arr>5] = 0
print(arr2)
#
[[1 2 3 4]
 [5 0 0 0]
 [0 0 0 0]]
```



```python
# 对numpy数组进行清零
arr3 = arr.copy()
arr3[:,:] = 0
print(arr3)

#
[[0 0 0 0]
 [0 0 0 0]
 [0 0 0 0]]

```

# 八、数组的合并

```python
import numpy as np

```

```python
arr = np.array([[1,2],[3,4],[5,6]])
print(arr)
# 
[[1 2]
 [3 4]
 [5 6]]
```







```python
arr1 = np.array([[7,8],[9,10],[11,12]])
print(arr1)
#
[[ 7  8]
 [ 9 10]
 [11 12]]
```



```python
# 合并两个numpy数组的行，注意使用hstack()方法合并numpy数组，numpy数组应该有相同的行，其中hstack的h表示horizontal水平的
print(np.hstack((arr1,arr)))
# 
[[ 7  8  1  2]
 [ 9 10  3  4]
 [11 12  5  6]]
```



```python
# 合并两个numpy数组，其中axis=1表示合并两个numpy数组的行
print(np.concatenate((arr1,arr),axis=1))
#
[[ 7  8  1  2]
 [ 9 10  3  4]
 [11 12  5  6]]
```



```python
# 合并两个numpy数组的列，注意使用vstack()方法合并numpy数组，numpy数组应该有相同的列，其中vstack的v表示vertical垂直的
print(np.vstack((arr1,arr)))
#
[[ 7  8]
 [ 9 10]
 [11 12]
 [ 1  2]
 [ 3  4]
 [ 5  6]]
```



```python
# 合并两个numpy数组，其中axis=0表示合并两个numpy数组的列
print(np.concatenate((arr1,arr),axis=0))
#
[[ 7  8]
 [ 9 10]
 [11 12]
 [ 1  2]
 [ 3  4]
 [ 5  6]]
```

# 九、通过函数创建numpy数组



```python

```



```python

```



```python

```



```python

```



```python

```



```python

```



```python

```



```python

```



```python

```



```python

```



```python

```

