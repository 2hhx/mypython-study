[TOC]

# random模块

# 一、random模块



```python
# 大于0 并且小于1之间的小数
import random
print(random.random())
```



```python
# 随机输出一个在1-3之间的整数
import random
print(random.randint(1,3))
```



```python
# 随机输出一个1到2 的整数
import random
print(random.randrange(1,3))
```



```python
# 随机输出一个大于1 小于3 的小数
import random
print(random.uniform(1,3))
```



```python
# 随机从列表内取一个元素进行输出
import random
print(random.choice([1,2,'23',[4,5]]))
```

```python
import random
lis = [1,2,3,4,5]
print(random.choices(lis))
#随机从列表内取一个元素进行输出，输出一个列表形式
```



```python
# random.sample([], n)，列表元素任意n个元素的组合，示例n=3，组合成列表
import random
print(random.sample([1,2,'24',[4,5]],3))
```



```python
#打乱顺序
import random
lis = [1,2,3,4,5]
random.shuffle(lis)
print(lis)
#[2, 3, 5, 4, 1]
```

