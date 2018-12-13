---
title: python爬虫Scrapy框架学习（1）
tags:
  - Python
  - 爬虫
  - Scrapy
categories:
  - 技术
  - 爬虫
abbrlink: 8333
date: 2018-09-07 23:13:09
---

### Scrapy安装
Mac下构建Scrapy依赖库需要C编译器及开发头文件，他一般由Xcode提供，具体命令如下：

```
xcode-select --install
```
随后利用pip安装Scrapy

```
pip install Scrapy
```
安装完成后验证安装，命令行输入：

```
Scrapy
```
运行结果如下图说明安装成功：
![WX20180907-232308@2x](http://pc59bkg3l.bkt.clouddn.com/WX20180907-232308@2x.png)

### Scrapy框架介绍
Scrapy是一个基于Twisted的异步处理框架，是纯Python实现的爬虫框架。其架构清晰，模块之间耦合程度低，可扩展性极强，可以灵活完成各种需求。我们只需要定制开发几个模版就可以轻松实现一个爬虫。

Scrapy的架构框架如下图所示
![Scrapy架构图-详细](http://pc59bkg3l.bkt.clouddn.com/Scrapy架构图-详细.png)


