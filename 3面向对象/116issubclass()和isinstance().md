[TOC]

# 一、issubclass()和isinstance()

# 1.1  issubclass()

判断第一个类是不是第二类的子类，返回true或者flase

```python
# 判断第一个类是不是第二类的子类，返回true或者flase


class Foo:
    pass


class A:
    pass


class Bar(Foo,A):
    pass


class Tt(Bar):
    pass

print(A.__bases__)#(<class 'object'>,)
print(Bar.__bases__)  # (<class '__main__.Foo'>,)，查看所有的父类，当继承一个类后，将不会显示object,以元组的形式表示：(<class '__main__.Foo'>,)
print(Bar.__base__)#<class '__main__.Foo'> 查看第一个父类
print(issubclass(Tt, object))  # True，object是所有类的祖宗
print(issubclass(Bar, Foo))  # True
print(issubclass(Tt, Foo))  # True
print(issubclass(Tt, A))  # False
```

# 1.2    isinstance()

判断第一个参数是不是第二个参数的对象，返回true或者flase

```python

# 判断第一个参数是不是第二个参数的对象，返回true或者flase
#isinstance()
class Foo:
    pass


class Tt():
    pass


f = Foo()
print(isinstance(f, Foo))  # True

print(isinstance(f, Tt))  # False

```

