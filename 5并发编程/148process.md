[TOC]



# multiprocessing模块

仔细说来，multiprocess不是一个模块而是python中一个操作、管理进程的包。 之所以叫multi是取自multiple的多功能的意思,在这个包中几乎包含了和进程有关的所有子模块。由于提供的子模块非常多，为了方便大家归类记忆，我将这部分大致分为四个部分：创建进程部分，进程同步部分，进程池部分，进程之间数据共享。

# multiprocessing.Process模块

process模块是一个创建进程的模块，借助这个模块，就可以完成进程的创建。

# process模块介绍

`Process([group [, target [, name [, args [, kwargs]]]]])`，由该类实例化得到的对象，表示一个子进程中的任务（尚未启动）

强调：

1. 需要使用关键字的方式来指定参数
2. args指定的为传给target函数的位置参数，是一个元组形式，必须有逗号

参数介绍：

- group参数未使用，值始终为None
- target表示调用对象，即子进程要执行的任务
- args表示调用对象的位置参数元组，`args=(1,2,'egon',)`
- kwargs表示调用对象的字典，`kwargs={'name':'egon','age':18}`
- name为子进程的名称

## 方法介绍

- `p.start()`：启动进程，并调用该子进程中的p.run()
- `p.run()`：进程启动时运行的方法，正是它去调用target指定的函数，我们自定义类的类中一定要实现该方法
- `p.terminate()`：强制终止进程p，不会进行任何清理操作，如果p创建了子进程，该子进程就成了僵尸进程，使用该方法需要特别小心这种情况。如果p还保存了一个锁那么也将不会被释放，进而导致死锁
- `p.is_alive()`：如果p仍然运行，返回True
- `p.join([timeout])`：主线程等待p终止（强调：是主线程处于等的状态，而p是处于运行的状态）。timeout是可选的超时时间，需要强调的是，p.join只能join住start开启的进程，而不能join住run开启的进程

## 3.2 属性介绍

- `p.daemon`：默认值为False，如果设为True，代表p为后台运行的守护进程，当p的父进程终止时，p也随之终止，并且设定为True后，p不能创建自己的新进程，必须在`p.start()`之前设置
- `p.name`：进程的名称
- `p.pid`：进程的pid
- `p.exitcode`：进程在运行时为None、如果为–N，表示被信号N结束(了解即可)
- `p.authkey`：进程的身份验证键,默认是由`os.urandom()`随机生成的32字符的字符串。这个键的用途是为涉及网络连接的底层进程间通信提供安全性，这类连接只有在具有相同的身份验证键时才能成功（了解即可）

## 3.3 在windows中使用process模块的注意事项



在Windows操作系统中由于没有fork(linux操作系统中创建进程的机制)，在创建子进程的时候会自动 import 启动它的这个文件，而在 import 的时候又执行了整个文件。因此如果将process()直接写在文件中就会无限递归创建子进程报错。所以必须把创建子进程的部分使用if \_\_name\_\_ =='\_\_main\_\_ 判断保护起来，import 的时候，就不会递归运行了。

# Process的用法



## 一、jion用法

内部会调用wait(),等待执行完毕，回收pid

### 1.1 jion用法1

```python
from multiprocessing import Process

import time

def foo():
    print('process start')
    time.sleep(2)
    print('process end')

if __name__ == '__main__':
    p = Process(target=foo) #创建子进程
    p.start()#给操作系统发指令，开启子进程
    time.sleep(5)
    p.join()#阻塞住主进程再等待子进程结束，然后再往下执行，（了解的是：内部会待用wait（））
    print('主进程')


# 
process start
process end
主进程
```

### 1.2join用法2

```python
from multiprocessing import Process
import time
def foo(x):
    print('Process start')
    time.sleep(x)
    print('Process end')
if __name__ == '__main__':
    p1 = Process(target=foo,args=(1,)) #进程传参的固定模式
    p2 = Process(target=foo,args=(2,))
    p3 = Process(target=foo,args=(3,))
    start = time.time()
    p1.start()
    p2.start()
    p3.start()
    p1.join()
    p2.join()
    p3.join()
    end = time.time()
    print(end-start)
    print('主进程')
#进程是同时开启的，不确定是哪一个先开启，并且创建子进程需要时间，因此是3秒多
Process start
Process start
Process start
Process end
Process end
Process end
3.136319160461426
主进程
```

join用法2，优化

```python

from multiprocessing import Process

import time
def foo(x):
    print(f'process{x},start')
    time.sleep(x)
    print(f'process{x},end')
if __name__ == '__main__':
    strart = time.time()
    p_list = []
    for i in range(1,4):
        p=Process(target=foo,args=(i,))
        p.start()
        p_list.append(p)
    print(p_list)
    for p in p_list:
        p.join()
    end = time.time()
    print(end-strart)
    print('主进程')
    
   # 
[<Process(Process-1, started)>, <Process(Process-2, started)>, <Process(Process-3, started)>]
process1,start
process2,start
process3,start
process1,end
process2,end
process3,end
3.1335043907165527
主进程

```

### 1.3 join用法3

```python
from multiprocessing import Process
import time
def foo(x):
    print(f'process{x},start')
    time.sleep(x)
    print(f'process{x},end')

if __name__ == '__main__':
    p1 = Process(target=foo,args=(1,))
    p2 = Process(target=foo,args=(2,))
    p3 = Process(target=foo,args=(3,))
    start = time.time()
    p1.start()
    p1.join()
    p2.start()
    p2.join()
    p3.start()
    p3.join()
    end = time.time()
    print(end-start)
    print('主进程结束')
#由于每一个进程都是在执行完后才会执行下一个进程，因此，时间是多余6s,创建子进程空间是需要时间的
process1,start
process1,end
process2,start
process2,end
process3,start
process3,end
6.339004039764404
主进程结束
```

## 二、查询进程的pid

进程的pid，相当于人的id身份证一样

```python
from multiprocessing import Process,current_process
import os
import time

def task():
    print('子进程 start')
    print('在子进程中查询自己的pid>',current_process().pid)
    print('在子进程中查询父进程的pid>',os.getppid())
    time.sleep(200)
    print('子进程end')
if __name__ == '__main__':
    p = Process(target=task)
    p.start()
    print('在主进程中查看子进程的pid>',p.pid)#一定要写在start之后，在给操作系统发送开启子进程之后
    print('主进程的pid>',os.getpid())
    print('主进程的父进程的pid>',os.getppid())
    print('主进程')
    
###
在主进程中查看子进程的pid> 11112
主进程的pid> 7320
主进程的父进程的pid> 2464
主进程
子进程 start
在子进程中查询自己的pid> 11112
在子进程中查询父进程的pid> 7320
```

## 三，查询进程名name

```python
from multiprocessing import Process,current_process
import time
def foo():
    print('process start')
    print('子进程的名字',current_process().name)
    time.sleep(2)
    print('process end')
if __name__ == '__main__':
    p = Process(target=foo)
    p2 = Process(target=foo)
    p3 = Process(target=foo,name='ocean')

    p.start()
    p2.start()
    p3.start()
    print(p.name)
    print(p2.name)
    print(p3.name)
    print('主进程')
################
Process-1
Process-2
ocean
主进程
process start
子进程的名字 Process-1
process start
子进程的名字 Process-2
process start
子进程的名字 ocean
process end
process end
process end

```

## 四、判断进程的生死is_alive()

```python

from multiprocessing import Process
import time
def foo():
    print('process start')
    time.sleep(2)
    print('process end')
if __name__ == '__main__':
    p = Process(target=foo)
    p.start()
    print(p.is_alive())#True,进程开启，进程或者
    time.sleep(5)
    print(p.is_alive())#False 代码运行完毕，进程死亡（系统判定，实际情况在具体分析）
    print('主进程')
    
    
##############
True
process start
process end
False
主进程

```

## 五、杀死进程terminate()

```python

from multiprocessing import Process
import time
def foo():
    print('process start')
    time.sleep(10)
    print('process end')
if __name__ == '__main__':
    p = Process(target=foo)
    p.start()
    p.terminate()#给操作系统发送一个请求，杀死进程的请求
    print(p.is_alive())
    p.join()
    print(p.is_alive())
    print('主进程')
###############
True
False
主进程

```



## 六、通过继承Process类开启进程

```python

import os
from multiprocessing import Process


class MyProcess(Process):
    def __init__(self,name):
        # super(MyProcess,self).__init__()
        super().__init__()
        self.name=name
    def run(self):
        print(os.getpid())
        print('%s 正在和女主播聊天' %self.name)
if __name__ == '__main__':

        p1=MyProcess('wupeiqi')
        p2=MyProcess('yuanhao')
        p3=MyProcess('nezha')

        p1.start() # start会自动调用run
        p2.start()
        # p2.run()
        p3.start()


        p1.join()
        p2.join()
        p3.join()

        print('主线程')
        
####################
1516
wupeiqi 正在和女主播聊天
4864
yuanhao 正在和女主播聊天
18228
nezha 正在和女主播聊天
主线程
```

## 七、进程间的数据隔离问题

```python

from multiprocessing import Process
def work():
    global n
    n = 0
    print('子进程内：',n)
if __name__ == '__main__':
    n = 100
    p = Process(target=work)
    p.start()
    print('主进程内：',n)
##############
主进程内： 100
子进程内： 0

```



