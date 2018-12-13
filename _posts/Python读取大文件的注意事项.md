---
title: Python读取大文件的注意事项
tags:
  - python
  - 内存溢出
categories:
  - 技术
  - 数据库
comments: true
abbrlink: 3382
date: 2018-12-12 11:20:31
---

python读写文件的都很简单，直接一个with open就解决了，但是这也只是在学习基础知识时候的小案例，不能轻易放入项目中。

#### 1.read()与readlines()


```
with open(file_path, 'rb') as f:
    sha1Obj.update(f.read())
or
with open(file_path, 'rb') as f:
    for line in f.readlines():
        print(line)

```
这对方法在读取小文件时确实不会产生什么异常，但是一旦读取大文件，很容易会产生MemoryError，也就是内存溢出的问题。



####Why Memory Error？

我们首先来看看这几个方法：

##### read方法
read([size])方法从文件当前位置起读取size个字节，若无参数size（size=-1），则表示读取至文件结束EOF为止，当文件大小大于可用内存时，自然会发生内存溢出的错误。

##### readline方法
该方法每次读出一行内容，所以，读取时占用内存小，比较适合大文件，该方法返回一个字符串对象。

可选参数 size 的含义同上。它是以行为单位返回字符串，也就是每次读一行，依次循环，如果不限定 size，直到最后一个返回的是空字符串，意味着到文件末尾了(EOF).


##### readlines方法
size参数 同上。它返回的是以行为单位的列表，即相当于先执行 readline() ，得到每一行，然后把这一行的字符串作为列表中的元素塞到一个列表中，最后将此列表返回。
readlines构造的是list|而不是iter，所以所有的内容都会保存在内存之上，文件内容过大同样也会发生内存溢出的错误。





##### 2.正确的用法
在实际运行的系统之中如果写出上述代码是十分危险的，。所以接下来我们来了解一下正确用法。

如果是**二进制**文件推荐用如下这种写法，可以自己指定缓冲区有多少byte。显然缓冲区越大，读取速度越快。


```

with open(file_path, 'rb') as f:
    while True:
        buf = f.read(1024)
        if buf:    
            Obj.update(buf)
        else:
            break

```

而如果是文本文件，则可以用readline方法或直接迭代文件（python这里封装了一个语法糖，二者的内生逻辑一致，不过显然迭代文件的写法更pythonic ）每次读取一行，效率是比较低的。笔者简单测试了一下，在3G文件之下，大概性能和前者差了20%.

```
with open(file_path, 'rb') as f:
    while True:
        line = f.readline()
        if buf:    
            print(line)
        else:
            break
            
with open(file_path, 'rb') as f:
    for line in f:
        print(line)
```

所以在使用python读取文件的时候如果是大文件（接近内存或大于1G）建议使用read(size=1024)（二进制文件）或者readline()（文本文件）。如果文件不大，而且需要经常使用文件数据，则可以直接使用read()和readlines()把数据全加载到内存里。

#### 3.内存检测工具的介绍
对于python代码的内存占用问题，对于代码进行内存监控十分必要。这里推荐两个小工具来检测python代码的内存占用。



####memory_profiler

首先先用pip安装memory_profiler


```
pip install memory_profiler
```

memory_profiler是利用python的装饰器工作的，所以我们需要在进行测试的函数上添加装饰器。

```
from hashlib import sha1
import sys
@profile
def my_func():
    sha1Obj = sha1()
    with open(sys.argv[1], 'rb') as f:
        while True:
            buf = f.read(10 * 1024 * 1024)
            if buf:
                sha1Obj.update(buf)
            else:
                break
    print(sha1Obj.hexdigest())
if __name__ == '__main__':
    my_func()

```
之后在运行代码时加上"` -m memory_profiler`"

就可以了解函数每一步代码的内存占用了

```
pyhton -m memory_profiler  test.py
```


#### guppy
依仍然是通过pip先安装guppy


```
pip install guppy
```

之后可以在代码之中利用guppy直接打印出对应各种python类型（list、tuple、dict等）分别创建了多少对象，占用了多少内存。

```
from guppy import hpy
import sys
def my_func():
    mem = hpy()
    with open(sys.argv[1], 'rb') as f:
        while True:
            buf = f.read(10 * 1024 * 1024)
            if buf:
                print(mem.heap())
            else:
                break
```

运行后，可以看到打印出对应的内存占用数据。


通过上述两种工具guppy与memory_profiler可以很好地来监控python代码运行时的内存占用问题。



