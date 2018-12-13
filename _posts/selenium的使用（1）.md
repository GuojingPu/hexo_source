---
title: selenium的使用（1）安装及环境配置
tags:
  - Python
  - selenium
categories:
  - 技术
  - 爬虫
abbrlink: 52449
date: 2018-08-30 15:49:51
---
### 1.安装selenium

```
pip install selenium
```

### 2.安装ChromeDriver
#### 2.1查看chrome版本：
![](http://pc59bkg3l.bkt.clouddn.com/查看chrome版本.png)


我的版本是68.0 64 位，所以要下载对应的ChromeDriver版本，打开如下网址：https://chromedriver.storage.googleapis.com/index.html

![](http://pc59bkg3l.bkt.clouddn.com/22.png)

下拉找到对应自己chrome版本的ChromeDriver版本，我的Chrome版本是68.0对应的ChromeDriver版本是2.40。
![23](http://pc59bkg3l.bkt.clouddn.com/23.png)



下面的是不同的chrome版本和对应的ChromeDriver版本对照表：第一列是ChromeDriver版本，第二列是chrome版本。

v2.40 v66-68
v2.39	v66-68
v2.38	v65-67
v2.37	v64-66
v2.36	v63-65
v2.35	v62-64
v2.34	v61-63
v2.33	v60-62
v2.32	v59-61
v2.31	v58-60
v2.30	v58-60
v2.29	v56-58
v2.28	v55-57
v2.27	v54-56
v2.26	v53-55
v2.25	v53-55
v2.24	v52-54
v2.23	v51-53
v2.22	v49-52
v2.21	v46-50
v2.20	v43-48
v2.19	v43-47
v2.18	v43-46
v2.17	v42-43
v2.13	v42-45
v2.15	v40-43
v2.14	v39-42
v2.13	v38-41
v2.12	v36-40
v2.11	v36-40
v2.10	v33-36
v2.9	v31-34
v2.8	v30-33
v2.7	v30-33
v2.6	v29-32
v2.5	v29-32
v2.4	v29-32
 
下载完成后会是一个chromedriver_mac64.zip压缩文件，解压后chromedriver是一个Unix可执行文件
![](http://pc59bkg3l.bkt.clouddn.com/15356166053357.jpg)

接下来就需要配置环境变量让这个可执行文件在任意目录下可执行，你可以直接把它放到`/usr/bin`等已经存在环境变量路径的目录里，也可以新添加一条自己的路径：
首先新建一个文件目录：

```
sudo mkdir -p /usr/local/chromedriver
```
然后把‘chromedriver’这个可执行文件放到这个目录里,在‘chromedriver’这个可执行文件所在的目录下执行：

```
sudo mv chromedriver  /usr/local/chromedriver
```
可以把文件放到该目录。

接着打开配置文件添加环境变量路径：

```
vim ./.bash_profile
```
添加一行：

```
export PATH="/usr/local/chromedriver:$PATH"
```

然后执行


```
source ./.bash_profile
```
使修改生效，然后输入：
`export`查看修改结果：

```
~export
...

PATH='/usr/local/chromedriver:/usr/local/opt/imagemagick@6/bin:/Users/pgj/anaconda3/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/aria2/bin:/usr/local/share/dotnet:/Library/Frameworks/Mono.framework/Versions/Current/Commands:/Applications/Wireshark.app/Contents/MacOS:/Applications/Xamarin Workbooks.app/Contents/SharedSupport/path-bin:/usr/local/mysql/bin'
PWD=/Users/pgj
SHELL=/bin/zsh
....
```

发现PATH中有：`/usr/local/chromedriver`目录路径（此路径一定要和chromedriver可执行文件所放目录一致，务必注意拼写错误），说明加入到环境变量成功。
之后关闭命令行终端重新打开命令行终端输入：`chromedriver`
如果控制台有如下显示输出说明配置好了:

```
~ chromedriver
Starting ChromeDriver 2.40.565386 (45a059dc425e08165f9a10324bd1380cc13ca363) on port 9515
Only local connections are allowed.
```
!!!注意该上述修改之后可能在运行代码是仍然会报：NOT FOUND chromedriver 错误，所以最好的办法就是直接把`chromedriver`可执行文件放入`usr/bin/`中。
其他浏览器也是同样类似的只需要下载对应的驱动文件放入`usr/bin/`中即可，比如Firefox浏览器就需要下载`GeckoDriver`，地址如下：https://github.com/mozilla/geckodriver/releases


由于我的MAC版本是：10.13,系统开启了Rootless内核保护机制，所以需要关闭。（mac 从10.11及之后加入了Rootless内核保护机制）
关闭方法参考如下链接：http://www.pc6.com/edu/86809.html
1.重启电脑开后按住 Command-R 进入恢复分区. 然后在 实用工具 栏找到 终端启动运行.
输入:`csrutil disable; reboot`
2.你会看到系统保护被关闭的字样并且系统自动重启. 这样你就可以修改系统级别的文件了. 　
3.通过`csrutil status`来查询 Rootless 保护的状态.


```
~ csrutil status
System Integrity Protection status: disabled.
```


4.最后就是重新激活 Rootless的方法了. 终端内输入：`csrutil enable`

