---
title: Mac下Redis数据的安装、操作、配置
tags:
  - 数据库
  - Redis
categories:
  - 技术
  - 数据库
abbrlink: 15027
date: 2018-10-16 15:49:56
---
### 1.安装
使用brew安装

```
brew install redis
```
完成后会自动安装redis-server和redis-cli。

redis安装后，默认会自动启动，可以通过以下PS进程命令查看：

```
ps aux|grep redis
```
执行结果：

```
➜  ~ ps aux|grep redis
pgj              17677   0.0  0.0  4287208   1064 s001  S+    3:54下午   0:00.01 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn redis
pgj              17616   0.0  0.0  4295500   1160 s003  S+    3:44下午   0:00.02 redis-cli -h 127.0.0.1 -p 6379 -a foobared
pgj              17555   0.0  0.0  4309276   2312 s002  S+    3:40下午   0:00.62 redis-server 127.0.0.1:6379
```
可以看到进程PID为17555的就是redis-server,可以使用：

```
kill 17555
```
关闭Redis服务（不建议使用，强制杀死进程，可能导致Redis部分缓存丢失）。
#### 手动启动：
Redis安装完成后会生成`/usr/local/etc/redis.conf`文件，执行启动命令：

```
redis-server /usr/local/etc/redis.conf
```
会显示：

```
➜  ~ redis-server /usr/local/etc/redis.conf
17555:C 16 Oct 15:40:34.312 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
17555:C 16 Oct 15:40:34.313 # Redis version=4.0.4, bits=64, commit=00000000, modified=0, pid=17555, just started
17555:C 16 Oct 15:40:34.313 # Configuration loaded
17555:M 16 Oct 15:40:34.316 * Increased maximum number of open files to 10032 (it was originally set to 4864).
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 4.0.4 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 17555
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
```
说明启动成功。
#### 客户端连接
```
 redis-cli -h [ip] -p [端口]
```
-h:指定IP
-p:指定端口号（默认6379，可在redis.conf中修改）

```
redis-cli -h 127.0.0.1 -p 6379
```

###配置
配置文件具体在：``/usr/local/etc/redis.conf``
#### 1.添加密码
在配置文件中找到：

```
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared
#
```
其中`requirepass`后面的字符，就是Redis数据库密码,默认时注释掉的。如果此处指定了密码，那么以后使用`redis-cli`链接redis数据库的时候就要使用密码，否则连接Redis数据库之后,无法进行Redis数据库操作。
比如指定密码为：123456

```
# use a very strong password otherwise it will be very easy to break.
#
requirepass 123456
#
```
那么以后使用`redis-cli`连接数据库的时候就要用`-a`指定密码下面的命令:

```
redis-cli -h 127.0.0.1 -p 6379 -a 123456
```
如果你在连接redis数据库是没有使用-a指定密码，也可以在redis-cli命令行中使用：`auth password `指定密码：

```
127.0.0.1:6379> auth 123456
OK
```


#### 2.让其其他的电脑访问可以redis
在配置文件中找打bind,默认IP只有127.0.0.1：

```
bind 127.0.0.1 ::1
```
在`127.0.0.1`后面加上自己电脑的（redis-server所在的电脑）的IP。

```
bind 127.0.0.1  198.168.1.102
```
注意：添加不是Redis客户端的IP，是redis-server端的IP。
这样别的客户端就可以使用：

```
redis-cli -h 198.168.1.102 6379
```
来连接redis数据库了





