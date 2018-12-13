---
title: memcached（1）- 调试使用
tags:
  - 数据库
  - memcached
categories:
  - 技术
  - 数据库
abbrlink: 52070
date: 2018-10-12 21:07:03
---
#### Mac memcached的调试

##### 1.安装：

```
brew install memcached
```


##### 2.启动：

```
memcached -d -p 11211 -u nobody -c 1024 -m 64
```


```
-d：这个参数是让memcached在后台运行（启动一个守护进程）。
-m：指定占用多少内存。以M为单位，默认为64M。
-u是运行Memcache的用户，我这里是nobody
-p：设置Memcache监听的端口,最好是1024以上的端口，默认端口是11211。
-l：别的机器可以通过哪个ip地址连接到我这台服务器。如果是通过service memcached 
start的方式，那么只能通过本机连接。如果想要让别的机器连接，就必须设置-l 0.0.0.0。
-c选项是最大运行的并发连接数，默认是1024，按照你服务器的负载量来设定

```


##### 3.Telnet连接

```
telnet 127.0.0.1 11211
```


##### telnet操作memcached:


memcached的数据是以 key:value 方式存储在内存中，所以我们一般对key 进行操作，而value保存实际的值。
###### 1.set：添加或更新数据：

```
语法：
  set key flas(是否压缩) timeout(存储的时间,0代表一直存储) value_length(value的长度)
  value
示例：
  set username 0 60 7
  zhiliao
```

注意指定的value长度和实际输入value的长度必须一致，否则按Enter键后会出现ERROR(如果实际的value长度大于指定的长度)或者一直等到你输入完指定的字符数。

###### 2.add：添加数据

```
语法：
  add key flas(是否压缩) timeout(存储的时间,0代表一直存储) value_length(value的长度)
  value
示例：
  add username 0 60 7
  xiaotuo

```
set和add的区别：add是只负责添加数据，不会去修改数据。如果添加的数据的key已经存在了，则添加失败，如果添加的key不存在，则添加成功。而set不同，如果memcached中不存在相同的key，则进行添加，如果存在，则替换。

set、add执行成功后会显示：STORED，未执行成功会显示：NOT STORED 

###### 3.get：获取数据

```
语法：
  get key
示例：
  get username
  
```
###### 4.delete:删除数据

```
语法：
  delete key
示例：
  delete username
  
```

###### 5.flush_all：删除memcached中的所有数据。

###### 6.incr:给memcached中数字类型的值进行相加操作，相加的项必须也是数字类型。

```

语法：
	incr key num(num表示要加的值，必须是数字)

示例：
	incr age 2   给key是age的值加上2

```

###### 7.decr:给memcached中数字类型的值进行减操作，相减的项必须也是数字类型。

```
语法：
	decr key num(num表示要减的值，必须是数字)

示例：
	devr age 2   给key是age的值减去2
```

###### 8.stats：查看memcached的当前状态，可以查看自己的操作记录和正确执行数据，以分析memcached的命中率（正确率）

```
示例：
	stats
```

结果：

```
STAT pid 5286
STAT uptime 3741
STAT time 1539349048
STAT version 1.5.3
...
STAT cmd_get 9   #表示进行实际get的次数
STAT cmd_set 16  #表示进行实际get的次数
STAT cmd_flush 1
STAT cmd_touch 0
STAT get_hits 7  #表示get命中(有效)的次数
...
END
```

常见的：
STAT cmd_get 9  表示进行实际get的次数
STAT cmd_set 16  表示进行实际set的次数
STAT get_hits 7  表示get命中(有效)的次数
STAT curr_items 4 表示当前有多少条数据

stats之后会显示很多结果常用的就是查看set、get等操作的cmd_get/set 数量和 get_hits 数量  用类似: get_hits/cmd_get  的结果反映当前memcached的状态，如果命中率良好，说明memcached的状态良好，反之说明memcached的状态较差，比如可能碎片较多。

