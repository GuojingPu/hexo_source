---
title: mac下MySQL忘记密码如何重置？
tags:
  - 数据库
  - Python
  - MySQL
categories:
  - 技术
  - 数据库
comments: true
abbrlink: 45042
date: 2018-08-28 23:20:31
---

## 忘记Mysql密码如何重置？

#### step1：

　　苹果->系统偏好设置->最下边点mysql 在弹出页面中 关闭mysql服务（点击stop mysql server）　　
#### step2：
　　进入终端输入：cd /usr/local/mysql/bin/
　　回车后 登录管理员权限 sudo su
　　回车后输入以下命令来禁止mysql验证功能 ./mysqld_safe --skip-grant-tables &
　　回车后mysql会自动重启（偏好设置中mysql的状态会变成running）　
　　　
#### step3.

　　输入命令 ./mysql
　　回车后，输入命令 FLUSH PRIVILEGES;
　　回车后，输入命令 ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';（会把密码重置为password，如果想改其他的把password替换即可）　
　　
　　完整步骤如下图:
　　![](media/15354371982475/%E5%BF%98%E8%AE%B0mysql%E5%AF%86%E7%A0%81%E5%8A%9E%E6%B3%95.png)




