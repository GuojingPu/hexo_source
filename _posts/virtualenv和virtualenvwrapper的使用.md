---
title: 虚拟环境virtualenv和virtualenvwrapper的使用
tags:
  - virsualenv
  - virtualenvwrapper
  - 虚拟环境
categories:
  - 工具
  - python
abbrlink: 31810
date: 2018-09-29 14:11:44
---
### 1.virtualenv虚拟环境的使用
##### 1.安装虚拟环境：

```
pip install virtualenv
```
##### 2.创建虚拟环境

```
virtualenv [虚拟环境的名称]
```
会在当前路径中创建虚拟环境文件夹

##### 3.进入虚拟环境

```
Windows:cd 到Script文件夹
Linux/Mac： source path/虚拟环境/bin/activate
```


##### 4.退出虚拟环境


```
deactivate
```

##### 5.创建虚拟环境的时候指定Python解释器：
当电脑有多个Python版本时，可以使用 -p 参数来指定Python解释器：

```
virtualenv -p  [Python解释器路径]
```

## 建议使用 virtualenvwrapper

使用virtualenv时每一次创建虚拟环境都会在当前目录下创建一个虚拟环境文件夹，如果创建太多会很混乱，使用virtualenvwrapper只会下用户文件夹下创建一个Env文件夹，所有的虚拟环境都放在这个Env里面，所以非常易于维护。
##### 1.安装

```
pip install virtualenvwrapper
```
进行下面步骤配置：
1.创建目录用来存放虚拟环境

```
mkdir $HOME/.virtualenvs
```
    
2.在~/.bash_profile中添加行：

```
#声明虚拟环境的文件夹
export WORKON_HOME=$HOME/.virtualenvs
#声明虚拟环境使用的python版本（多pyhton版本情况，请填写自己的python版本解释器路径）
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/local/python3.6 
#执行虚拟环境脚本生效
source /usr/local/bin/virtualenvwrapper.sh
```

3.运行以下命令使修改生效:

```
source ~/.bash_profile
```
执行：

```
➜~ virtualenvwrapper
```
如果提示：`not found command`或者`/usr/bin/python: No module named virtualenvwrapper`，说明未在正确的版本上安装virtualenv。
##### 1.创建虚拟环境

```
mkvirtualenv [虚拟环境名称]
```
会在当前用户文件夹下创建一个Env文件夹，然后将虚拟环境安装到这个目录下。

##### 2.切换到虚拟环境
    
```
workon  my_env
```
#### 退出当前虚拟环境

```
deactivate
```
##### 3.删除某个虚拟环境

```
rmvirtualenv my_env
```
##### 4.列出所有虚拟环境

```
lsvirtualenv
```
##### 5.进入某个虚拟环境

```
cdvirtualenv my_env
```

