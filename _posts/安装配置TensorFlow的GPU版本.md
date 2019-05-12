---
title: 安装配置TensorFlow的GPU版本
categories:
  - 技术
  - 深度学习
tags:
  - TensorFlow
  - 机器学习
comments: true
abbrlink: 39077
date: 2019-01-07 17:01:47
img:
---

###win10下tensorflow、Kera的GPU版本配置


#### 介绍
你准备配置TensorFlow GPU版本的之前，你肯定已经十分熟悉了python的开发环境的配置。一个Python的初学者，通常不会上来就做那么复杂的工作。Python、pip、anacoda、virtualenv、virtualenvwrapper...分别是什么？什么是python虚拟环境、为什么需要虚拟环境？如何在windows/linux/mac下安装配置python的开发环境、你可以参考我的[【这篇文章】]()或者自己查阅资料。

你已经安装好了Python的开发环境，不管你是不使用anaconda还是pip，virtuaenv，这里我用的是“**python3+pip+virtualenvwrapper**”的环境,接下来我们来安装CUDA和cuDNN。


####1.该如何选择CUDA的版本？
这里建议选择CUDA9.0的版本，至于为什么选择CUDA9.0而不选择最新的CUDA10.0这里讲一下。

因为现在tensorflow-gpu的版本更新到tensorflow-gpu==1.12版本，它不支持CUDA10.0版本，即使你正确安装了CUDA10.0版本和对应的cuDNN版本，你在```import tensorflow``` 时，也会遇到 “找不到对应的DLL模块的错误”。

当然如果你非要安装CUDA 10.0的版本，我这里有一个被修改过的tensorflow-gpu本地安装版本，他可以在CUDA10.0的版本下正常运行。这里是下载链接：[xxxx.whl]()

下载完成后通过

```
pip install  xxxx.whl
```

安装，当然在安装前,如果你已经安装过tensorflow包或者tensorflow-gpu包的话，你需要先卸载他们。

而至于更老的8.1的版本，一方面是因为他看起来有点过时了，另一方面是因为他在安装和使用的时候会产生一些不兼容问题，所以也不建议使用。


#### 2.安装
这里的参考安装版本组合：python3.6 + tensorflow 1.12 + keras 2.2.4 + CUDA 10.0 + cuDNN 7.4

首先来看一下他们几个之间的关系

![QQ20190104-165158@2x](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/QQ20190104-165115%402x.png)


在安装之前请查看一下自己的显卡是否支持CUDA。GTX 1060 1070 1080/1080i都支持，剩下的可以参考下面的网站查看：[CUDA显卡查询]()

##### 1.安装CUDA

这里是下载连接：
[CUDA10.0下载连接](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exelocal) 

![WX20190110-173234@2x](media/15465901117286/WX20190110-173234@2x.png)

[CUDA9.0下载链接](https://developer.nvidia.com/cuda-90-download-archive?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exelocal)

![WX20190110-173122@2x](media/15465901117286/WX20190110-173122@2x.png)


下载完成后尽量以管理员模式运行安装。

他会提示你选安装方式是精简还是自定义。这里你可以直接选精简，如果你选择自定义，可以勾选掉visualstudio组件的部分，如果你没有使用visualstudio的IDE的话。

安装完成后，你可以在[此电脑]->[鼠标右键]->[属性]->[高级]->[环境变量]->[系统环境变量]中看到2个关于CUDA_PATH的环境变量设置，说明你安装成功。

##### 2.安装cuDNN

cuDNN的安装需要你先注册一个账户登录，然后才让你下载，安装时注意，请安装对应的CUDA版本的cuDNN版本，

[cuDNN for CUDA下载链接](https://developer.nvidia.com/cudnn) 

注册完成下载时里面会有：
![A985FE99-7CDB-4b53-88A0-B6DD8BD5A6A7](media/15465901117286/A985FE99-7CDB-4b53-88A0-B6DD8BD5A6A7.png)


下载完成后他是一个压缩包，解压出来是一个cuda文件夹，里面有bin、lib、include等文件夹及一些.DDL文件，你可以把它放到任意地方（甚至和CUDA的安装路径放在一起），只要你配置的环境变量能引用到他。这里我们参考建议做法，把它放到C:\Program Files盘里面,路径为：C:\Program Files\cuda   下面我们来配置cuDNN的环境变量。

![](http://puguojing-hexo.oss-cn-shanghai.aliyuncs.com/NZMTTP%259%7B%244ZV%29%40I86AXVHV.png)

最主要的是要把cuDNN中的bin中的 XXXX.DLL文件能够通过系统环境变量Path中找到
下面是环境变量的内容在系统环境变量的Path中添加：




网上有一些复杂的多个环境变量配置，你不需要参考他们，我也不清楚他们为什么会把环境变量配置的那么复杂，你可看看tensorflow官网的介绍：https://tensorflow.google.cn/install/gpu


###### 3.安装tensorflow-gpu
直接通过pip安装：

```
pip install tensorflow-gpu
```
如果是10.0的话记得使用上面我提供的whl安装包安装。

###### 4.安装keras

```
pip install keras
```



安装好之后Keras会自动使用tensorflow的GPU版本，不需要其他配置。

重新运行你的程序你会发现，已经使用了GPU的版本并且会显示你的显卡型号、大小等信息。




