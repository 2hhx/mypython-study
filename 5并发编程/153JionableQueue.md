# JoinableQueue([maxsize])模块

创建可连接的共享进程队列。这就像是一个Queue对象，但队列允许项目的使用者通知生产者项目已经被成功处理。通知进程是使用共享的信号和条件变量来实现的。

## 方法介绍

`q.task_done()`：使用者使用此方法发出信号，表示q.get()返回的项目已经被处理。如果调用此方法的次数大于从队列中删除的项目数量，将引发ValueError异常。

`q.join()`：生产者将使用此方法进行阻塞，直到队列中所有项目均被处理。阻塞将持续到为队列中的每个项目均调用q.task_done()方法为止。

```python
from multiprocessing import JoinableQueue
#用法和Queue相似
q = JoinableQueue()
q.put('ocean')#队列放入一个任务，内存在一个计数机制，+1
q.put('can')#计数机制 +1
print(q.get())
q.task_done()#完成一次任务，计数机制-1
print(q.get())
q.task_done()#计数机制  -1
q.join()#计数机制不为0的时候，阻塞等待计数器为0后通过
##################
ocean
can
```

