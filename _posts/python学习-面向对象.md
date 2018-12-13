---
title: Python学习笔记-面向对象
tags:
  - Python
  - 面向对象
categories:
  - 技术
  - python
abbrlink: 56848
date: 2018-07-22 14:35:42
---
&#8195;&#8195;
## 面向对象
**类：**是我们抽象出来的属性的集合，是描述。
**实例：**也叫做对象，是我们用类这个属性集合具体生成的实例。
**属性：**定义在类中的方法或变量。
**实例属性：**定义在具体方法中的属性（属性可以是方法也可以是变量），只用作当前的实例。
**类属性：**类属性定义在类中且在函数体外，类属性被所有实例化的对象共有，类属性通常不作为实例属性使用。

### 类的变量属性

比如定义类变量: 
   
   
```
Class A():
    var_1 = 1 #类的变量属性，可以使用类名和实例名调用
```

其中var_1这个变量指的就是A这个类的变量，他可以通过类名调用（比如：A.var_1），也可以通过实例名调用（比如：a = A()    a.Var_1）
比如我们创建2个A类的实例：

```
a1 = A()
a2 = A()
```

我们可以使用实例名调用变量：

```
a1.var_1  #值为1
a2.var_1  #值为1
```

也可以使用类名调用变量：

```
A.var_1  #值为1
```

如果单独修改a1.var_1或a2.var_1的值，则不会影响其他实例的var_1和类本身的var_1的值，但是如果修改A.var_1（类变量）的值，则所有的实例变量都会被改变。比如,如果把a.var_1的值修改为2，则b.var_1和A.var_1的值均不会改变，仍为1。但是如果把A.var_1的值修改为2则a.var_1和b.var_1的值都会变成2。

### 实例的变量属性
    
```
Class A():
    def __init__(self)
        self.var_1 = 1 #实例的变量属性，只可以使用实例名调用
```

创建2个实例：

```
a1 = A()
a2 = A()
```

使用实例名调用变量：

```
a1.var_1  值为1
a2.var_1  值为1
```

使用类名调用变量：

```
A.var_1   #不可以使用
```

### 类属性和实例属性名不能设置成一样

```

>>> class Student(object):
...     name = 'Student'
...
>>> s = Student() # 创建实例s
>>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
>>> print(Student.name) # 打印类的name属性
Student
>>> s.name = 'Michael' # 给实例绑定name属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student
>>> del s.name # 如果删除实例的name属性
>>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student

```

从上面的例子可以看出，在编写程序的时候，千万不要对实例属性和类属性使用相同的名字，因为相同名称的实例属性将屏蔽掉类属性，但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性。

### 实例的函数属性
实例的函数属性一般函数的第一个参数是*self*,Pyhon会默认把函数的第一个参数当做指向类实例的参数，一般使用*self*，当然你也可以换成其他的名字，毕竟这个地方只是形参，但是一般从程序的可读性上而言我们不会那么做。

```
Class A():
    def func(self,a,b):
        print(a+b)

```

如果我们想要调用func,则需要先实例化A这个类，然后使用实例化出来的对象调用：

```
a = A()
a.fun(2,3)
```

但是这里我们可不可以使用类名调用这个func函数呢，比如使用`A.func(2,3)`行不行呢,这当然是不行的。

>`“self，这里的self可以理解为C++中的this指针。遵循的原则就是：哪个实例调用我这个含有self的函数或变量，我这个函数或变量的self就代表哪个实例（指向哪个实例）。总结一句话：“谁调用就指向谁”`

### 类方法
类方法顾名思义就是指类的方法，而不是实例的方法，下面我们来看2个例子：

```
Class A():
    def func(a,b):
        print(a+b)
```

我们注意到这里的func没有使用self这个参数，那如果我们使用`a.fun(2,3)`这种调用会怎么样呢？答案是会报参数出错，因为在使用实例调用一个函数的时候，第一个参数会被默认是self，然而我们这里并没有定义这个参数。这里我们可以通过类名来调用这个参数`A.func(2,3)`,这样会成功。这里我们好像就是要使用类名调用函数了，看起来像是类的方法，但是通常我们不会这样做，而是采用@clasmethod装饰器的方法来定义类方法：

```
Class A():
    @classmethod
    def func(cls,a,b):
        print(a+b)
```

首先我们可以使用实例对象调用这个方法：

```
a = A()
a.func(2,3)
```

也可以使用类名来调用这个方法：

```
A.func(2,3)
```

这里的方法默认会有一个cls的参数，用来表示当前的类（注意不是实例）。

如果想在类的方法里面调用类的变量，可以使用cls:

```
class A():

    num = 1
    @classmethod
    def func(cls):
        cls.num += 1#使用cls.num，而不能直接使用num
        print('cls.num:',cls.num)
```

注意不管是使用类名调用类方法还是使用实例调用类方法所产生的结果都会相应影响：

```

a = A()

print('A.num:',A.num)
a.func()
print('A.num:',A.num)
A.func()
print('A.num:',A.num)
```

输出的结果：

```
A.num: 1
cls.num: 2
A.num: 2
cls.num: 3
A.num: 3
```

而且类方法中不能调用实例变量属性，比如：

```

class A():
    def __init__(self):
        self.var = 0

    @classmethod
    def func(cls):
        cls.var += 1
        print(cls.var)
```

会报错:`AttributeError: type object 'A' has no attribute 'var'`

### 静态方法
定义静态方法使用`@staticmethod`，可以使用类名调用。静态方法不需要默认的任何参数,跟一般的普通函数类似.**通过这样的定义方式，我们可以在多个实例之间共享这个函数中的数据和内容。**静态方法无法访问实例变量。

**类方法于静态方法**：
>
1.类方法需要传递cls参数，静态方法不需要；
2.静态方法和类方法都不可以访问实例变量；
3.类方法可以通过cs访问类变量，静态方法不可以。


```
class A():

    num = 1

    @staticmethod
    def func():#func中无法使用num
        a = 1
        print('',a)

```

可以使用实例和类名调用：

```
a = A()
A.func()
```

### 继承

继承分为单继承和多继承。**单继承：**子类只能继承一个父类。**多继承：**子类继承多个父类。
#### 单继承：

```
class A1(A):
    pass
```

可以使用`A1.__base__`查看A1的基类

```
class A1(A):
    def __init__(self):
        self.var = 1
        print(self.var)
```

A1是A的子类，如果A1中没有__init__（self）函数，默认会调用父类的__init__(self),如果子类重写了__init__(self),则不会调用父类的__init___(self)。


#### 多继承：
定义两个基类：

```
class A():
    def __init__(self):
        print('this is A')


class B():
    def __init__(self):
        print('this is B')
```

定义一个子类C：

```
class C(A,B):#注意继承顺序
    pass
```

实例化C这个类：

```
c = C()
```

输出是：

```
this is B
```

说明调用的是B的**__init__(self)**

如果定义C这个类

```
class C(B,A):
    pass
```

实例化C这个类输出是：

```
this is A
```

说明调用的是A的**__init__(self)**

### Python 多继承调用父类的顺序：
python调用父类构造的顺序是:*从左往右，从下往上*。采用的是广度优先搜索算法，总会先找离自己最近的一个节点。具体方法是:首先访问i这个节点，然后访问i所有未被访问的相邻节点。

![14](http://pc59bkg3l.bkt.clouddn.com/14.png)


如图，访问顺序是：`9->7->8->3->4->5->6->1->2`

```
#!/usr/bin/python3
# -*- coding:utf-8 -*-

class A(object):
    def __init__(self):
        print('A')
        super(A, self).__init__()
class B(object):
    def __init__(self):
        print('B')
        super(B, self).__init__()
class C(A):
    def __init__(self):
        print('C')
        super(C, self).__init__()
class D(A):
    def __init__(self):
        print('D')
        super(D, self).__init__()
class E(B,C):
    def __init__(self):
        print('E')
        super(E, self).__init__()
class F(C,B,D):
    def __init__(self):
        print('F')
        super(F, self).__init__()
class G(D,B):
    def __init__(self):
        print('G')
        super(G, self).__init__()

if __name__ == '__main__':
    g = G()
    print(G.mro())
    f = F()
    print(F.mro())
```

输出结果：

```
G
D
A
B
[<class '__main__.G'>, <class '__main__.D'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>]
F
C
B
D
A
[<class '__main__.F'>, <class '__main__.C'>, <class '__main__.B'>, <class '__main__.D'>, <class '__main__.A'>, <class 'object'>]

```

1.如果每个类都有正确的写super(X, self).__init__()，那么mro顺序的所有的类的初始化方法都会执行一遍 
2.如果中途有的类没有写或错误的写了super(X, self).__init__()，那么会按mro顺序执行到执行完没写的那个类，执行结束。 
3.super(X, self)不是必须点__init__()，可以点别的，但一般没有这么做的 
4.类名.mro可以得到mro的顺序 
5.这种写法可以实现每个类都被执行有且仅有一次。

####  静态语言VS动态语言的继承

对于静态语言（例如Java）来说，如果需要传入Animal类型，则传入的对象必须是Animal类型或者它的子类，否则，将无法调用run()方法。

对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了：

```
class Timer(object):
    def run(self):
        print('Start...')
```

>
这就是动态语言的“**鸭子类型**”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。

Python的“file-like object“就是一种鸭子类型。对真正的文件对象，它有一个read()方法，返回其内容。但是，许多对象，只要有read()方法，都被视为“file-like object“。许多函数接收的参数就是“file-like object“，你不一定要传入真正的文件对象，完全可以传入任何实现了read()方法的对象。

>
继承可以把父类的所有功能都直接拿过来，这样就不必重零做起，子类只需要新增自己特有的方法，也可以把父类不适合的方法覆盖重写。

>动态语言的鸭子类型特点决定了继承不像静态语言那样是必须的

#### 获取对象的信息

当我们拿到一个对象的引用时，如何知道这个对象是什么类型、有哪些方法呢？

##### 用dir()
如果要获得一个对象的所有属性和方法，可以使用dir()函数，它返回一个包含字符串的list，比如，获得一个str对象的所有属性和方法：

```
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```

类似__xxx__的属性和方法在Python中都是有特殊用途的，比如__len__方法返回长度。在Python中，如果你调用len()函数试图获取一个对象的长度，实际上，在len()函数内部，它自动去调用该对象的__len__()方法，所以，下面的代码是等价的：

```
>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```

我们自己写的类，如果也想用len(myObj)的话，就自己写一个__len__()方法：

```
>>> class MyDog(object):
...     def __len__(self):
...         return 100
...
>>> dog = MyDog()
>>> len(dog)
100
```

剩下的都是普通属性或方法，比如lower()返回小写的字符串：

```
>>> 'ABC'.lower()
'abc'
```

##### getattr()、setattr()以及hasattr()

仅仅把属性和方法列出来是不够的，配合getattr()、setattr()以及hasattr()，我们可以直接操作一个对象的状态：

```
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
...
>>> obj = MyObject()
```

紧接着，可以测试该对象的属性：

```
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19

```

如果试图获取不存在的属性，会抛出AttributeError的错误：

```
>>> getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```

可以传入一个default参数，如果属性不存在，就返回默认值：

```
>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
```

也可以获得对象的方法：

```
>>> hasattr(obj, 'power') # 有属性'power'吗？
True
>>> getattr(obj, 'power') # 获取属性'power'
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
>>> fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn() # 调用fn()与调用obj.power()是一样的
81


```

#### 使用__slots__
如果我们想要限制实例的属性怎么办？比如，只允许对Student实例添加name和age属性。

为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的__slots__变量，来限制该class实例能添加的属性：


```
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
然后，我们试试：

```
>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```


由于'score'没有被放到__slots__中，所以不能绑定score属性，试图绑定score将得到AttributeError的错误。

使用__slots__要注意，__slots__定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：

```
>>> class GraduateStudent(Student):
...     pass
...
>>> g = GraduateStudent()
>>> g.score = 9999
```

除非在子类中也定义__slots__，这样，**子类实例允许定义的属性就是自身的__slots__加上父类的__slots__。**


#### 使用@property
在绑定属性时，如果我们直接把属性暴露出去，虽然写起来很简单，但是，没办法检查参数，导致可以把成绩随便改：

```
s = Student()
s.score = 9999
```

这显然不合逻辑。为了限制score的范围，可以通过一个set_score()方法来设置成绩，再通过一个get_score()来获取成绩，这样，在set_score()方法里，就可以检查参数：

```
class Student(object):

    def get_score(self):
         return self._score

    def set_score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

现在，对任意的Student实例进行操作，就不能随心所欲地设置score了：

```
>>> s = Student()
>>> s.set_score(60) # ok!
>>> s.get_score()
60
>>> s.set_score(9999)
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!

```


但是，上面的调用方法又略显复杂，没有直接用属性这么直接简单。

有没有既能检查参数，又可以用类似属性这样简单的方式来访问类的变量呢？对于追求完美的Python程序员来说，这是必须要做到的！

还记得装饰器（decorator）可以给函数动态加上功能吗？对于类的方法，装饰器一样起作用。Python内置的@property装饰器就是负责把一个方法变成属性调用的：


```
class Student(object):

    @property
    def score(self):#变成属性调用读取属性
        return self._score

    @score.setter
    def score(self, value):#设置属性,使用：@方法名.setter
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

@property的实现比较复杂，我们先考察如何使用。把一个getter方法变成属性，只需要加上@property就可以了，此时，@property本身又创建了另一个装饰器@score.setter，负责把一个setter方法变成属性赋值，于是，我们就拥有一个可控的属性操作：

```
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

注意到这个神奇的@property，我们在对实例属性操作的时候，就知道该属性很可能不是直接暴露的，而是通过getter和setter方法来实现的。

还可以定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性：


```
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2015 - self._birth
```

上面的birth是可读写属性，而age就是一个只读属性，因为age可以根据birth和当前时间计算出来。

小结
@property广泛应用在类的定义中，可以让调用者写出简短的代码，同时保证对参数进行必要的检查，这样，程序运行时就减少了出错的可能性。



#### 枚举类

```
from enum import Enum

#获得了Month类型的枚举类，可以直接使用Month.Jan来引用一个常量，或者枚举它的所有成员
Month = Enum('Month',('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))#


for name, member in Month.__members__.items():
    print(name, member, member.value)
```

输出:

```
Jan Month.Jan 1
Feb Month.Feb 2
Mar Month.Mar 3
Apr Month.Apr 4
May Month.May 5
Jun Month.Jun 6
Jul Month.Jul 7
Aug Month.Aug 8
Sep Month.Sep 9
Oct Month.Oct 10
Nov Month.Nov 11
Dec Month.Dec 12
```

如果想自己控制枚举的值，可以是使用派生类，继承Enum:

```
from enum import Enum,unique
#自定义控制枚举数值，使用派生类，继承Enum
@unique
class WeekDay(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6

#有多种方法可以访问枚举
print(WeekDay.Sun)#WeekDay.Sun
print(WeekDay.Tue.value)#2
print(WeekDay['Wed'])#WeekDay.Wed
print(WeekDay(0))#WeekDay.Sun

for name, member in WeekDay.__members__.items():
    print(name, member, member.value)
```

输出结果:

```
WeekDay.Sun
2
WeekDay.Wed
WeekDay.Sun
Sun WeekDay.Sun 0
Mon WeekDay.Mon 1
Tue WeekDay.Tue 2
Wed WeekDay.Wed 3
Thu WeekDay.Thu 4
Fri WeekDay.Fri 5
Sat WeekDay.Sat 6
```

### 元类
参考[[廖雪峰Python教程]](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319106919344c4ef8b1e04c48778bb45796e0335839000)

