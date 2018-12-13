---
title: python爬虫-爬取豆瓣书籍（1）
tags:
  - Python
  - 爬虫
  - Requests
  - Xpath
categories:
  - 技术
  - 爬虫
abbrlink: 41965
date: 2018-08-26 14:10:20
---

本篇记录使用Requests + Xpath爬取豆瓣书籍
## 1.Requests
Requests是python的一个HTTP库，里面包含了常见的一些HTTP操作，下面是Requests常用的七种方法：




| 方法 | 说明 |
| :-: | :-: |
| requests.request() | 构造一个请求，支撑以下方法的基础方法 |
| requests.get() | 获取HTML网页的主要方法，对应于HTTP的GET |
| requests.head() | 获取HTML网页的信息头的方法，对应于HTTP的HEAD |
| requests.post() | 向HTML网页提交POST请求的方法，对应于HTTP的POST |
| requests.put() | 向HTML网页提交PUT请求的方法，对应于HTTP的PUT |
| requests.patch() |   向HTML网页提交局部修改请求的方法，对应于HTTP的PATCH |
| requests.delete() | 向HTML网页提交请删除求的方法，对应于HTTP的DELETE |

我们最常用的就是request.get()方法。
## Requests.get()的用法

```
import requests#导入Requests库
r = requests.get(url)#使用get方法发送请求，返回包含网页数据的Response并存储到对象r中
```
### Response对象的属性
**r.statues.code:** http请求的返回状态，200表示连接成功，404表示连接失败

**r.text：**http响应内容的字符串形式，url对应的页面内容

**r.encoding：**从HTTP header中猜测的响应内容编码方式

**r.apparent_encoding：**从内容分析出的响应内容的编码方式（备选编码方式）

**r.content:**HTTP响应内容的二进制形式

**r.headers：**http响应内容的头部内容


```
resp.text返回的是Unicode型的数据。

resp.content返回的是bytes型也就是二进制的数据。

也就是说，如果你想取文本，可以通过r.text。

如果想取图片，文件，则可以通过r.content。

（resp.json()返回的是json格式数据）
```


[**查看Requestes开发文档**](http://docs.python-requests.org/zh_CN/latest/#)

## Xpath
xpath即为XML路径语言（XML Path Language），他是一种用来确定XML文档中某部分位置的语言。
Xpath基于XML的树状结构，提供在数据结构树中找寻节点的能力。起初Xpath的提出初衷是将其做为一个通用的、介于Xpointer与XSL间的语法模型，但是很快Xpath被开发者采用来当作小型的查询语言。

### Xpath解析网页的流程
1.首先通过Requests库获取网页数据。
2.通过网页解析得到想要的数据或者新的链接。
3.网页解析可以通过Xpath或者其他的解析工具进行。Xpath，是一个非常友好的网页解析工具。

### lxml库
lxml库结合libxml2快速强大的特性，使用xpath语法来进行文件格式解析，与Beautiful相比，效率更高。
#### 安装

```
pip install lxml
```
#### 使用


```
from lxml import etree #导入库
data = requests.get(url).text#获取网页内容
s = etree.HTML(data)#解析网页节点
```
代码：


```
def craw_douban_book_top250():
    msg_num = 0 #数量统计
    for page in range(10):  #根据对网站分析，每页有25条信息，所有爬取10页共250条
        url = 'https://book.douban.com/top250?start={}'.format(page*25)#每页的url地址最后刚好差25

        data = requests.get(url).text #获取网页的内容
        s = etree.HTML(data)   #解析HTML节点转换成对象

        divs = s.xpath('//*[@id="content"]/div/div[1]/div/table') #每一本书籍最外部的节点


        for div in divs:
            msg_num += 1
            title = div.xpath('./tr/td[2]/div[1]/a/@title')[0]#从最外部节点找到书籍名，后面加[0]是引用列表元素。
            href = div.xpath('./tr/td[2]/div[1]/a/@href')[0]#从最外部节点找到书籍详细链接
            score = div.xpath('./tr/td[2]/div[2]/span[2]/text()')[0]#从最外部节点找到书籍评分
            score_people_num = div.xpath('./tr/td[2]/div[2]/span[3]/text()')[0]\
                                        .strip("(").strip().strip(")")#从最外部节点找到书籍评价人数，并去除左右两边的括号和中间的空格
            print("{}: {} {} {} {}".format(msg_num, title, href, score, score_people_num))
            time.sleep(1)#防止抓取信息过于频繁导致IP被封及尽量不影响别人的网站

```

通过对豆瓣TOP250书籍的url分析对比如下：


```
<a href="https://book.douban.com/top250?start=25">2</a>
<a href="https://book.douban.com/top250?start=50">3</a>
<a href="https://book.douban.com/top250?start=75">4</a>
<a href="https://book.douban.com/top250?start=100">5</a>
....
```
我们发现区别就在于最后"start="的数值每次相差25，所以 可以使用：

```
for page in range(10): 10页共250条
        url = 'https://book.douban.com/top250?start={}'.format(page*25)
```
来获取网页地址。

我们在获取每一本书的Xpath的时候也发现相应的规律，比如第一本书：


```
书籍的Xpath：‘//*[@id="content"]/div/div[1]/div/table[1]/tbody/tr/td[2]’
书名的Xpath: '//*[@id="content"]/div/div[1]/div/table[1]/tbody/tr/td[2]/div[1]/a'
评分的Xpath：‘//*[@id="content"]/div/div[1]/div/table[1]/tbody/tr/td[2]/div[2]/span[2]’
人数的Xpath：‘//*[@id="content"]/div/div[1]/div/table[1]/tbody/tr/td[2]/div[2]/span[3]’
```
我们可以看到的是书名、评分、人数的Xpath都是在整本书的`Xpath=‘//*[@id="content"]/div/div[1]/div/table[1]/tbody/tr/td[2]’`的基础上在多加一点内容。所以我们可以这样写：

```
divs = s.xpath('//*[@id="content"]/div/div[1]/div/table') #每一本书籍最外部的节点

        for div in divs:
            msg_num += 1
            title = div.xpath('./tr/td[2]/div[1]/a/@title')[0]#从最外部节点找到书籍名
            href = div.xpath('./tr/td[2]/div[1]/a/@href')[0]#从最外部节点找到书籍详细链接
            score = div.xpath('./tr/td[2]/div[2]/span[2]/text()')[0]#从最外部节点找到书籍评分
            score_people_num = div.xpath('./tr/td[2]/div[2]/span[3]/text()')[0]\
                                        .strip("(").strip().strip(")")#从最外部节点找到书籍评价人数，并去除左右两边的括号和中间的空格
```
每一个信息都是在最外部的节点路径上加上更详细的路径。
>注意：
>
>1.当我们复制Xpath是里面会有一个”tbody/“内容，我们在下载时需要把这个内容删除掉，不然无法获取对应的信息。
>2.关于在Xpath内容后加上”text()“,"@title"、”@href“等内容是Xpath语法详见XPath总结的内容。

下面是另一个爬取小猪短租南京在指定时间内的信息

```
def Craw():
    msg_num = 0
    for p in range(1,6):
        url = 'http://nj.xiaozhu.com/search-duanzufang-p{}-0/?startDate=2018-08-27&endDate=2018-09-30'.format(p)
        data = requests.get(url).text
        s = etree.HTML(data)
        #print(data)

        divs = s.xpath('//*[@id="page_list"]/ul/li')

        for div in divs:
            msg_num += 1
            titles = div.xpath('./div[2]/div/a/span/text()')[0]
            details = div.xpath('./div[2]/div/em/text()')[0].strip()
            prices = div.xpath('./div[2]/span[1]/i/text()')[0]
            href = div.xpath('./a/@href')[0]
            time.sleep(1)
            print("{}: {}  {}  {}  {}".format(msg_num,titles,details,prices,href))
```
加入存储数据到文件的内容：


```
def Craw():
    msg_num = 0
    with open(r'/Users/pgj/Desktop/data.csv', 'w', encoding='utf-8') as f:
        for p in range(1,3):
            url = 'http://nj.xiaozhu.com/search-duanzufang-p{}-0/?startDate=2018-08-27&endDate=2018-09-30'.format(p)
            data = requests.get(url).text
            s = etree.HTML(data)
            #print(data)

            divs = s.xpath('//*[@id="page_list"]/ul/li')

            for div in divs:
                msg_num += 1
                titles = div.xpath('./div[2]/div/a/span/text()')[0]
                details = div.xpath('./div[2]/div/em/text()')[0].strip()
                prices = div.xpath('./div[2]/span[1]/i/text()')[0]
                href = div.xpath('./a/@href')[0]
                time.sleep(1)
                f.write("{},{},{},{},{}\n".format(msg_num,titles,details,prices,href))
                print("{}: {}  {}  {}  {}".format(msg_num,titles,details,prices,href))

Craw()
```


