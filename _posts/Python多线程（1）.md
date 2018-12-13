---
title: Python多线程（1）
tags:
  - Python
  - 线程
categories:
  - 技术
  - python
abbrlink: 997
date: 2018-09-09 22:11:06
---
&#8195;&#8195;
#### 1.进程
&#8195;&#8195;计算机程序知识存储在磁盘上的可执行二进制（或其他类型）文件，只有把他们加载到内存中并被操作系统调用，才拥有生命周期，进程是一个执行中的程序，每个进程都拥有自己的地址空间、内存、数据栈以及其他用于跟踪的辅助数据。由于进程之间相互独立，所以只能采用进程间通信(IPC)的方式共享信息。
#### 2.线程
&#8195;&#8195;线程与进程类似，不过他是在同一个进程下执行的，所以共享上下文。

#### 3.全局解释锁(GIL)
&#8195;&#8195;Python代码的执行是由Python解释器（解释器主循环）进行控制的，在主循环中同时只能有一个线程在执行，就像单核CPU的多进程一样。尽管Python解释器可以运行多个线程，但是在任意给定的时刻只能有一个线程会被解释器调用。
Python解释器将按照下面的过程执行：
1.设置GIL。
2.切换金一个线程去运行。
3.执行下面的操作之一 ：
    &#8195;&#8195;a.指定数量的字节码指令。
    &#8195;&#8195;b.线程主动让出控制权（可以调用time.sleep(0)来完成。
4.把线程切换回睡眠状态（切换出线程。
5.解锁GIL。
6.重复上述步骤。
    
&#8195;&#8195;在当调用外部代码（即任意C/C++扩展D额内置函数）时，GIL会保持锁定，直至函数执行结束(因为在这期间没有Python字节码计数）。编写扩展函数的程序员有能力解锁GIL。然而，作为Python开发者，你并不需要担心Pyhon代码会在这些情况下被锁住。

&#8195;&#8195;例如，对于任意面向I/O的Python例程(调用了内置操作系统的C代码那种)GIL会在I/O调用前被释放，以允许其他线程在I/O执行的时候运行。而对那些没有太多I/O操作的代码而言，更加倾向于在该线程整个
时间片内始终占有处理器。换句话说就是，**I/O密集型的Python程序要比计算密集型的代码能够更好的利用多线程环境。**

&#8195;&#8195;关于解释器主循环和GIL可以才看源码*Python/ceval.c*

#### 4.在Python中使用线程

&#8195;&#8195;Python提供了多个模块来支持多线程编程，包括thread、threading，queue模块等。thread提供了基本的线程和锁定支持。threading模块提供了更高级别功能，更全面面的线程管理。queue模块，用户可以创建一个队列数据结构，用于在多线程之间进行共享。

**推荐使用更高级别的锁定模块threading，而不是使用thread模块。**

1.一个原因是低级别的thread模块入的同步原语很少只有Lock，而threading模块拥有Lock,Semephore、Condition、Event等多种同步机制。
2.另一个原因是thread对于进程何时退出没有控制，当主线程结束时，所有其他线程也都强制结束，不会发出警告或者进行适当的清理，所以他也不支持**守护线程**。


 **守护线程**
>
&#8195;&#8195;守护线程一般是一个等待客户端请求服务的服务器，如果没有客户端请求守护线程就是空闲的.如果把一个线程置为守护线程，就表示这个线程是不重要的。进程退出时不需要等待这个线程执行完成。

>&#8195;&#8195;如果主线程准备退出时不需要等待某些子线程完成，就可以为这些子线程设置守护线程标记。该标记值为真时，表示线程是不重要的，或者说线程只是用来等待客户端请求而不做任何其他事情。设置一个线程为守护线程需要在启动线程之前执行如下赋值语句：`thread.daemon = True`

#### 5.threaing模块下的Thread类



| 属性 | 描述 |
| --- | --- |
| Thread对象的数据属性 |  |
| Name | 线程名 |
| Ident | 线程标志符 |
| Daemon | 守护线程标志 |
| Thread对象方法 |  |
| __init__(group=NONE,target=None,name=None, args=(),kwargs{},verbose=None,daemon=None) | 实例化一个线程对象，需要有一个可调用的target，以及其参数args或kwargs，还可以传递name或group参数，daemon会设置thread.daemon属性标志 |
| Start() | 开始执行线程 |
| run() | 定义线程功能的方法（通常在子类中进行重写） |
| jion() | 直至启动的线程终止之前一直挂起。除非给出了timeout。否则会一直阻塞。 |

代码案例一，创建Thread实例传给他一个可调用的函数：

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
import threading
from time import sleep,ctime

loops_time = [4,2]

def loop(nloop,nsec):
    print('start loop :',nloop,'at:',ctime())
    sleep(nsec)
    print('end   loop :', nloop, 'at:', ctime())


def main():
    print('starting at----------------- :',ctime())

    test_thresds = []

    for i in range(len(loops_time)):
        t = threading.Thread(target=loop,args=(i,loops_time[i]))
        test_thresds.append(t)

    for i in range(len(loops_time)):
        test_thresds[i].start()

    for i in range(len(loops_time)):
        test_thresds[i].join()

    print('end at :-------------------', ctime())

if __name__ == '__main__':
    main()
```

代码案例二：创建Thread实例传给他一个可调用的**类实例**：

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
import threading
from time import sleep,ctime



loops_time = [4,2]


class ThreadFunc(object):
    def __init__(self,func,args,name=''):
        self.name = name
        self.args = args
        self.func = func


    def __call__(self, *args, **kwargs):
        self.func(*self.args)


def loop(nloop,nsec):
    print('start loop :',nloop,'at:',ctime())
    sleep(nsec)
    print('end   loop :', nloop, 'at:', ctime())



def main():
    print('starting at----------------- :',ctime())

    test_thresds = []


    for i in range(len(loops_time)):
        t = threading.Thread(target=ThreadFunc(loop,args=(i,loops_time[i]),name=loop.__name__))
        test_thresds.append(t)

    for i in range(len(loops_time)):
        test_thresds[i].start()

    for i in range(len(loops_time)):
        test_thresds[i].join()

    print('end at :-------------------', ctime())

if __name__ == '__main__':
    main()
```
代码案例三，派生Thread子类，并创建子类实例：


```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
import threading
from time import sleep,ctime

loops_time = [4,2]

class MyThread(threading.Thread):
    def __init__(self,func,args,name=''):
        threading.Thread.__init__(self)
        self.name = name
        self.args = args
        self.func = func


    def getResult(self):
        return self.res


    def run(self):
        self.res = self.func(*self.args)


def loop(nloop,nsec):
    print('start loop :',nloop,'at:',ctime())
    sleep(nsec)
    print('end   loop :', nloop, 'at:', ctime())


def main():
    print('starting at----------------- :',ctime())

    test_thresds = []

    for i in range(len(loops_time)):
        t = MyThread(func=loop,args=(i,loops_time[i]),name=loop.__name__)
        test_thresds.append(t)

    for i in range(len(loops_time)):
        test_thresds[i].start()
        print(threading.enumerate())  # 当前活动的Thread对象列表


    for i in range(len(loops_time)):

        test_thresds[i].join()

    print('end at :-------------------', ctime())

if __name__ == '__main__':
    main()



```

