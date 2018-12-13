---
title: Python多线程（2）
tags:
  - Python
  - 线程
categories:
  - 技术
  - python
abbrlink: 18405
date: 2018-09-09 23:11:06
---

##### Lock、BoundedSemaphore（和Semaphore类似，不过不允许超过初始值）的使用：


```
#!/usr/bin/python3
# -*- coding:utf-8 -*-
from atexit import register
from random import randrange
from threading import BoundedSemaphore,Lock,Thread
from  time import sleep,ctime

lock = Lock()

MAX = 5

candytray = BoundedSemaphore(MAX)

def producer(loops):

    print('producer loops:',loops)
    lock.acquire()

    for i in range(loops):
        try:
            candytray.release()#realse后Semaphore会增多相当于产生
        except ValueError:
            print('release fail all Semaphore ')
        else:
            print('release(produce) ok')

        sleep(randrange(3))

    lock.release()

def consumer(loops):
    print('consumer loops:', loops)
    lock.acquire()

    for i in range(loops):

        if candytray.acquire(False):#acquire后 Semaphore会减少
            print('acquire(consumer) ok')
        else:
            print('acquire fail  empty')

        sleep(randrange(3))

    lock.release()



def main():
    n=randrange(2,6)

    Thread(target=consumer,args=(randrange(n,n+MAX+2),)).start()

    Thread(target=producer, args=(n,)).start()


@register
def _atexit():
    print('DONE :',ctime())



if __name__ == '__main__':
    main()

```


