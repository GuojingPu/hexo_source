---
title: Mac下购买配置和访问阿里云服务器的详细步骤
categories:
  - 技术
  - 阿里云
tags:
  - 阿里云服务器
  - 部署
comments: true
abbrlink: 11894
date: 2018-11-06 17:08:02
img:
---

最近一直想买一个阿里云服务器玩玩，听说学生版的非常便宜一个月只要9.9元，但是我已经毕业了啊，所以忍痛买了一个入门一70.2元/月。

### 阿里云注册
首先你需要注册一个阿里云账户或者不想注册直接使用自己的淘宝、支付宝账户登录也可以。
你可以在这里注册 [阿里云注册](https://account.aliyun.com/register/register.htm?spm=5176.8142029.388261.26.a7236d3ewunhyE&oauth_callback=https%3A%2F%2Fwww.aliyun.com%2F%3Futm_medium%3Dtext%26utm_source%3Dbdbrand%26utm_campaign%3Dbdbrand%26utm_content%3Dse_32492)

### 阿里云翼计划
如果你还是学生的话，那么恭喜你可以在这里加入[阿里云翼计划](https://promotion.aliyun.com/ntms/act/campus2018.html)一个月只需要9.5元！😍

![2018110601](http://pc59bkg3l.bkt.clouddn.com/2018110601.png)

### 阿里云服务器ECS购买
那如果你和我一样不满足条云翼计划的条件那就只能在 【阿里云】-【产品】-【精选】-【云服务器ECS】(ECS是阿里云服务器的名字，腾讯的叫CVM，仅仅是一个名字而已)

![QQ20181106-172613@2x23](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-172613@2x23.png)

在点击进入之后你会看到如下的界面：
![QQ20181106-173200@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-173200@2x.png)

1.点击立即购买，：
![QQ20181106-173229@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-173229@2x.png)
2.你会看到一大堆你并不能看懂的配置，没关系，你可以直接选择【一键购买】
![QQ20181106-173311@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-173311@2x.png)

3.是不是开起来清爽多了？那这些配置分别代表什么意思呢？
地域：表示你服务器的地理位置，具体不同的地理位置有什么区别，这个主要是访问者的距离服务器越远一般情况访问速度越慢，所以你看到发达的华东地区服务器稍微贵一点。你可以根据你的具体地理位置情况和money多少自由选择，哪一个区域都可以。但是要记住你自己所选择的区域，因为购买完成后我们要到这个区域的管理控制台下管理我们的服务器。

实例：就是购买的电脑主机了，具体配置怎么选择，关键看你的需求了，就像我们买电脑一样CPU、内存、磁盘大小，具体要看你来干什么，一般我就选最小的，因为我的网站就是个人小网站啊。

![QQ20181106-173422@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-173422@2x.png)

镜像：这个主要是我们购买的主机你想让他装什么样的操作系统和操作系统的版本，有Windows、linux两大类。至于你该如何选择主要参考你网站采用的技术，由于本人使用的是Python开发的网站,所以选择是Ubuntu，一款强大而简单的Linux发行版。

公网带宽：这个要取决你网站的访问量了，我当然选最低的了1Mbps对于我来说够了。

购买量：就是购买时间，你可以购买一周，这里楼主买了一个月。

4.选择完配置之后直接点击【立即购买】

![QQ20181106-173504@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-173504@2x.png)

5.勾选【云服务器ECS服务条款】-【去下单】

![QQ20181106-173531@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-173531@2x.png)

6.【确认支付】，然后使用支付宝扫码支付即可。

### 查看购买云服务器ECS
1.在你购买完成之后一般会自动跳转到云服务器ECS管理控制台，你可以通过点击主页右上角的【控制台】进入控制台管理页面。
![QQ20181106-180207@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-180207@2x.png)

2.进入之后在左侧选择【云服务器ECS】
![QQ20181106-180306@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-180306@2x.png)

3.进入之后选择 左侧【实例】查看购买的云服务器。
![QQ20181106-180516@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-180516@2x.png)

4.如果在【实例】下没有找到云服务器的话，说明你可能没有切换到自己对应地理位置的控制台。你可以通过做左上角下拉选择自己购买时选择的服务器地理区域。
![QQ20181106-180542@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-180542@2x.png)

4.在这里可以看到【基本信息】【配置信息】【付费信息】【监控信息】等等。

### 连接阿里云服务器
在这里面可以进行「远程连接」到你的服务器，但是在进行「远程连接」之前我们还需要配置一下我们的服务器用户密码：

1.点击【更多】【密码/密钥】【重置密码】，就可以自己设置登录密码了。
![QQ20181106-180630@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-180630@2x.png)

2.在设置完登录密码后需要重启服务器，你可以在【管理】中重启服务器：
![QQ20181106-182205@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-182205@2x.png)

然后我们就可以点击【远程连接】，第一次远程连接时会给你一个默认的6位密码，你也可以在上面的【重置密码】中找到【修改远程连接密码】进行修改自己的密码。
在输入远程连接密码后会进入下面的界面，输入用户名和密码就可以登录了。

![QQ20181106-182457@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-182457@2x.png)

用户名默认是root,密码就是刚才我们重置的密码（不是远程连接密码）。

### MAC使用ssh远程连接阿里云服务器
不过阿里云提供的这个「远程连接」我们可肯定不太使用，所以我们会使用自己的ssh客户端登录阿里云主机。

但是在使用shh连接阿里云服务器之前我们还需要做一些安全设置，否则你用ssh连接或者ping公网ip都是会失败的，所以我们需要先进行设置。

1.点击「更多」-「安全组配置」-「配置规则」

![QQ20181106-183922@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-183922@2x.png)

2.点击「添加安全组规则」然后就开始添加规则了，可参考我的配置
![QQ20181106-184026@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-184026@2x.png)

然后我们来ping一下和尝试使用ssh登录：
先看一下IP地址，在【实例】信息中查看
![QQ20181106-184735@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-184735@2x.png)

然后尝试ping和ssh登录：
![QQ20181106-184537@2x](http://pc59bkg3l.bkt.clouddn.com/QQ20181106-184537@2x.png)

查看远程网络是否正常连接：

```
ping 47.93.53.97
```

远程登录：root用户名，按提示输入上面重置的登录密码

```
ssh root@47.93.53.97
```

连接成功后就可以干自己的事情了！

