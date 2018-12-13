---
title: Python爬虫-selenium的使用（2）
tags:
  - Python
  - 爬虫
  - selenium
categories:
  - 技术
  - 爬虫
abbrlink: 56006
date: 2018-08-30 15:49:51
---

### 使用selenium打开chrome浏览器百度进行搜索
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

import time
brower = webdriver.Chrome()

try:
    brower.get('https://www.baidu.com/')
    input = brower.find_element_by_id('kw')#通过id寻找元素
    input.send_keys('Python')#在找到的input输入框内输入Python
    input.send_keys(Keys.ENTER)#输入键盘的Enter
    wait = WebDriverWait(brower,10)#设置wait的超时时间
    wait.until(EC.presence_of_element_located((By.ID,'content_left')))#等待直到出现id是conten_left的元素加载出来的时候，最大等待时间不超过10秒
    print(brower.current_url)#打印url
    print(brower.get_cookies())#打印现在的cookies
    print(brower.page_source)#打印网页源代码

    time.sleep(5)
except Exception as e:
    print(e)
finally:
    brower.close()
```

还可以使用其他浏览器，比如：Firefox、Edge、以及一些手机浏览器等

```
from selenium import webdriver


brower = webdriver.Chrome()
brower = webdriver.Firefox()
brower = webdriver.Edge()
brower = webdriver.PhantomJS()
brower = webdriver.Safari()

```
注意每个浏览器都有自己对应的类似chrome浏览器的chromedriver驱动可执行文件。

### 2使用selenium打开淘货搜索


```
brower = webdriver.Chrome()
try:
    brower.get('https://www.taobao.com/')
    search_input = brower.find_element_by_id('q')
    search_input.send_keys('iPhone')
    time.sleep(2)
    search_input.clear()
    time.sleep(1)
    search_input.send_keys('iPad')
    time.sleep(1)
    search_button = brower.find_element_by_class_name('btn-search')
    time.sleep(1)
    search_button.click()
    time.sleep(10)
except Exception as e:
    print(e)
finally:
    brower.close()
```
打开淘宝，搜索‘iPhone’然后删除，接着搜索‘iPad’


### 3.动作链，实现拖拽


```
from selenium.webdriver import ActionChains


brower = webdriver.Chrome()
try:
    url = 'http://www.runoob.com/try/try.php?filename=jqueryui-api-droppable'
    brower.get(url)
    brower.switch_to.frame('#iframeResult')#切换到子页面中'iframeResult'是子页面的id名，也可以根据name inex 甚至webElement对象定位
    source = brower.find_element_by_css_selector('#draggable')
    target = brower.find_element_by_css_selector('#droppable')
    actions = ActionChains(brower)#构建action
    actions.drag_and_drop(source,target)#
    actions.perform()#调用perform()就是执行上面的动作链
except Exception as e:
    print(e)
finally:
    brower.close()
```

ActionChains是什么呢？顾名思义，是一个动作链，如果在一个用例中只有一两个动作，那么用之前讲过的简单版的就可以了，如果动作很复杂，那么可以使用这个了。

1.ActionChains是自动执行低级交互的一种方式，例如：鼠标移动，鼠标点按，键盘操作，文本操作等。
2.当我们调用这里的方法时，这些操作会被先储存在一个队列中，当我们调用perform()方法时，队列中的操作会被按顺序执行，执行后队列被清空。
#### iframe的切换
[参看网址:https://blog.csdn.net/huilan_same/article/details/52200586](https://blog.csdn.net/huilan_same/article/details/52200586)
frame标签有frameset、frame、iframe三种，frameset跟其他普通标签没有区别，不会影响到正常的定位，而frame与iframe对selenium定位而言是一样的，selenium有一组方法对frame进行操作。

##### 1.怎么切到frame中(switch_to.frame())
selenium提供了switch_to.frame()方法来切换frame


```
switch_to.frame(reference)
```

不得不提到switch_to_frame()，很多人在这样写的时候会发现，这句话被划上了删除线，原因是这个方法已经out了，之后很有可能会不支持，建议的写法是switch_to.frame()

reference是传入的参数，用来定位frame，可以传入id、name、index以及selenium的WebElement对象，假设有如下HTML代码 index.html：


```
<html lang="en">
<head>
    <title>FrameTest</title>
</head>
<body>
<iframe src="a.html" id="frame1" name="myframe"></iframe>
</body>
</html>
```
想要定位其中的iframe并切进去，可以通过如下代码：


```
from selenium import webdriver
driver = webdriver.Firefox()
driver.switch_to.frame(0)  # 1.用frame的index来定位，第一个是0
# driver.switch_to.frame("frame1")  # 2.用id来定位
# driver.switch_to.frame("myframe")  # 3.用name来定位
# driver.switch_to.frame(driver.find_element_by_tag_name("iframe"))  # 4.用WebElement对象来定位
```

通常采用id和name就能够解决绝大多数问题。但有时候frame并无这两项属性，则可以用index和WebElement来定位：
1.index从0开始，传入整型参数即判定为用index定位，传入str参数则判定为用id/name定位
2.WebElement对象，即用find_element系列方法所取得的对象，我们可以用tag_name、xpath等来定位frame对象

比如：

```
<iframe src="myframetest.html" />
```
用xpath定位，传入WebElement对象：

```
driver.switch_to.frame(driver.find_element_by_xpath("//iframe[contains(@src,'myframe')]"))
```
##### 2.从frame中切回主文档(switch_to.default_content())
切到frame中之后，我们便不能继续操作主文档的元素，这时如果想操作主文档内容，则需切回主文档。

```
driver.switch_to.default_content()
```
##### 3.嵌套frame的操作(switch_to.parent_frame())
有时候我们会遇到嵌套的frame，如下：

```
<html>
    <iframe id="frame1">
        <iframe id="frame2" / >
    </iframe>
</html>
```
1.从主文档切到frame2，一层层切进去

```
driver.switch_to.frame("frame1")
driver.switch_to.frame("frame2")
```
2.从frame2再切回frame1，这里selenium给我们提供了一个方法能够从子frame切回到父frame，而不用我们切回主文档再切进来。

```
driver.switch_to.parent_frame()  # 如果当前已是主文档，则无效果
```
有了parent_frame()这个相当于后退的方法，我们可以随意切换不同的frame，随意的跳来跳去了。

所以只要善用以下三个方法，遇到frame分分钟搞定：

```
driver.switch_to.frame(reference)
driver.switch_to.parent_frame()
driver.switch_to.default_content()
```
另外补充一下，之前曾看到过用点分法来切入嵌套frame的方法，但我试验之后发现并不能定位到frame：

```
driver.switch_to.frame('frame1.0.frame3')
```
据说以上代码可以切到 “frame1” 下的 “第一个frame” 下的 “frame3” 中。
### Selenium官方文档

[点击查看Selenium官方文档](https://selenium-python-zh.readthedocs.io/en/latest/#)



