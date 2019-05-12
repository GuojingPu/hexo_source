---
title: ubuntu16.04安装mongodb企业版
categories:
  - 技术
  - 数据库
tags:
  - mongodb
comments: true
abbrlink: 52136
date: 2019-03-24 14:13:12
img:
---

#MongoDB企业版在ubuntu16.04安装
安装方法参考官网：[https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-ubuntu/](https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-ubuntu/)

打开终端一次运行下面指令：
1.

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```

2.

```
echo "deb [ arch=amd64,arm64,ppc64el,s390x ] http://repo.mongodb.com/apt/ubuntu xenial/mongodb-enterprise/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
```

3.
```
sudo apt-get update
```
4.
```
sudo apt-get install -y mongodb-enterprise
```
5.

```
sudo apt-get install -y mongodb-enterprise=4.0.6 mongodb-enterprise-server=4.0.6 mongodb-enterprise-shell=4.0.6 mongodb-enterprise-mongos=4.0.6 mongodb-enterprise-tools=4.0.6
```

6.

```
echo "mongodb-enterprise hold" | sudo dpkg --set-selections
echo "mongodb-enterprise-server hold" | sudo dpkg --set-selections
echo "mongodb-enterprise-shell hold" | sudo dpkg --set-selections
echo "mongodb-enterprise-mongos hold" | sudo dpkg --set-selections
echo "mongodb-enterprise-tools hold" | sudo dpkg --set-selections
```

安装完成。

####默认的data和log的位置：

data的位置：

```
its data files in /var/lib/mongodb
```
Log的位置：

```
/var/log/mongodb/mongod.log 
```

#####启动与停止：

```
sudo service mongod start
sudo service mongod stop
sudo service mongod restart
```


连接Mongodb：

```
mongo
```


#### 根据配置文件启动mongodb

使用配置文件启动：


```
mongod  --config /etc/mongod.conf 

```
默认的mongodb配置文件/etc/mongod.conf 


####开启远程认证连接：

首先配置文件


```
bindIp: 127.0.0.1 改为 bindIp: 0.0.0.0 或者 bindIp: [127.0.0.1,mongo所在服务器ip]

```
注意冒号后面有空格。

之后重启MOngoDB。

```
service  mongod  restart
```

然后在浏览器输入mongodb所在的服务器地址加端口号可以有如下显示：

![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/QQ20190324-0.png)



#### 开启认证:

1.（可以省略）进入 admin数据库（默认创建）下创建一个用户：


```

use admin;
db.createUser({user:"root",pwd:"12345678",roles:[{role:"root",db:"admin"}]})

```
![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/QQ20190324-3.png)

admin是MOngoDB默认的用来管理的数据库，我们这个用户拥有所有的权限。可以不创建。

2。我们往往需要创建一个自己使用的数据库，并添加用户和权限。

```
use movies;
db.movies.insert("name":"test"});
 db.createUser({user:"pgj",pwd:"1234556",roles:[{role:"readWrite",db:"movies"}]}
```
注意这里`use movies;`之后会自动创建movies数据库，但是使用`show dbs;`指令无法显示，需要插入一条数据之后才会显示出来。

创建用户时：user和pwd表示创建的用户名和密码，role表示这个用户的操作权限，root表示最高，db表示要操作的是哪个数据库。

关键role的值可以选择下面：

![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/447370-20171117151015562-892340916.png)

创建成功后会像admin一样打印成功信息，并显示创建的信息。

创建完成之后需要重启MOngoDB，这时候注意需要加上  --auth 参数 同时 --fork 表示在后台运行：

```
mongod --config  /etc/mongod.conf --auth --fork
```

![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/ewwewq34234234324.png)


#### 使用Robot3T连接

![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/QQ20190324-181251@2x.png)

![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/QQ20190324-181311@2x.png)
![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/QQ20190324-181420@2x.png)



####配置文件说明

```

#日志文件位置
logpath=/data/db/journal/mongodb.log　　（这些都是可以自定义修改的）

# 以追加方式写入日志
logappend=true

# 是否以守护进程方式运行
fork = true

# 默认27017
#port = 27017

# 数据库文件位置
dbpath=/data/db

# 启用定期记录CPU利用率和 I/O 等待
#cpu = true

# 是否以安全认证方式运行，默认是不认证的非安全方式
#noauth = true
#auth = true

# 详细记录输出
#verbose = true

# Inspect all client data for validity on receipt (useful for
# developing drivers)用于开发驱动程序时验证客户端请求
#objcheck = true

# Enable db quota management
# 启用数据库配额管理
#quota = true
# 设置oplog记录等级
# Set oplogging level where n is
#   0=off (default)
#   1=W
#   2=R
#   3=both
#   7=W+some reads
#diaglog=0

# Diagnostic/debugging option 动态调试项
#nocursors = true

# Ignore query hints 忽略查询提示
#nohints = true
# 禁用http界面，默认为localhost：28017
#nohttpinterface = true

# 关闭服务器端脚本，这将极大的限制功能
# Turns off server-side scripting.  This will result in greatly limited
# functionality
#noscripting = true
# 关闭扫描表，任何查询将会是扫描失败
# Turns off table scans.  Any query that would do a table scan fails.
#notablescan = true
# 关闭数据文件预分配
# Disable data file preallocation.
#noprealloc = true
# 为新数据库指定.ns文件的大小，单位:MB
# Specify .ns file size for new databases.
# nssize =

# Replication Options 复制选项
# in replicated mongo databases, specify the replica set name here
#replSet=setname
# maximum size in megabytes for replication operation log
#oplogSize=1024
# path to a key file storing authentication info for connections
# between replica set members
#指定存储身份验证信息的密钥文件的路径
#keyFile=/path/to/keyfile
```


