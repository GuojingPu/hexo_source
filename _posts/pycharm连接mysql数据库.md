---
title: Pycharm连接mysql数据库
tags:
  - 数据库
  - PyCharm
  - MySQL
categories:
  - 工具
abbrlink: 33268
date: 2018-10-07 18:08:57
---
1.首先选择添加mysql数据库。
![1](http://pc59bkg3l.bkt.clouddn.com/1.png)

2.填入对应的额mysql配置信息（其中数据库orm_lookup_demo应该事先被创建好）
![2](http://pc59bkg3l.bkt.clouddn.com/2.png)

此时可以点击“Test Connections”测试是否连接成功，如果连接成功会出现:"succesfully"提示。
如果“Test Connections”按钮无法点击，说明没有安装对应的mysql java驱动（因为pyhcarm使用java语言编写）。
可以按下面的步骤快速安装：
（1）.点击“Test Connections”按钮下面的Mysql 跳转到 Driver配置界面。
（2）.点击下面途中的连接下载安装。
（3）.重新回到数据库连接界面发现可以执行“Test Connections”。

![3](http://pc59bkg3l.bkt.clouddn.com/3.png)

