# 8.3 预读源码必要了解的知识点

在阅读源码之前，源码中会涉及到很多python类的特殊的用法以及类写好的功能组件，所以这里我们做一个补充，以便于接下来源码的阅读

![1554342931492](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155420180-1539337740..png)

## 01 偏函数

当函数的参数个数太多，需要简化时，使用`functools.partial`可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单。

    
    
    from functools import partial
    def func(a1,a2,a3):
        print(a1,a2,a3)
    
    
    new_func1 = partial(func,a1=1,a2=2)
    new_func1(a3=3)
    
    new_func2 = partial(func,1,2)
    new_func2(3)
    
    new_func3 = partial(func,a1=1)
    new_func3(a2=2,a3=3)

**注意** ：partial括号内第一个参数是原函数，其余参数是需要固定的参数

**效果图** ：

![1553002020199](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155420400-1230142937..png)

## 02 `__add__`的使用

![1554342980138](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155420578-685782301..png)

如果一个类里面定义了 `__add__`方法，如果这个类的对象 `+`另一个对象，会触发这个类的`__add__`方法，换个说法如果 `对象1+对象2`
则会触发`对象1`的 `__add__`方法，python在类中有很多类似的方法，对象会在不同情况下出发对应的方法。

    
    
    class Foo:
        def __init__(self):
            self.num = 1
        def __add__(self, other):
            if isinstance(other,Foo):
                result = self.num + other.num
            else:
                result = self.num + other
            return result
    
    fo1 = Foo()
    fo2 = Foo()
    v1 = fo1 + fo2
    v2 = fo1 + 4
    print(v1,v2)

效果图：

![1553002690004](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155420787-1693929097..png)

## 03 chain函数

![1554343018090](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155420920-1491539549..png)

chain函数来自于itertools库，itertools库提供了非常有用的基于迭代对象的函数，而chain函数则是可以串联多个迭代对象来形成一个更大的迭代对象
。

**实例1：**

    
    
    from itertools import chain
    
    l1 = [1,2,3]
    l2 = [4,5]
    
    new_iter = chain(l1,l2) # 参数必须为可迭代对象
    print(new_iter)
    for i in new_iter:
        print(i)

效果图：

![1553003580254](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155421124-1086324095..png)

**实例2：**

    
    
    from itertools import chain
    
    
    def f1(x):
        return x+1
    
    def f2(x):
        return x+2
    
    def f3(x):
        return x+3
    list_4 = [f1, f2]
    new_iter2 = chain([f3], list_4)
    for i in new_iter2:
        print(i)

效果图：

![1553003696355](https://img2018.cnblogs.com/blog/1825659/201910/1825659-20191012155421277-1702083091..png)

