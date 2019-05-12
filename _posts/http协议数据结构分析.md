---
title: http协议数据结构分析
categories:
  - 技术
tags:
  - HTTP
comments: true
abbrlink: 53657
date: 2019-05-12 18:00:45
img:
---


### HTTP协议数据结构分析
一：网络协议HHTP

　　超文本传输协议

　　RFC2616

二：HTTP报文主要结构

　　1)Request

　　　　Method(get,post) ---请求方式

　　　　URL-------请求地址

　　　　Header------请求头

　　　　Body--------请求体

　　2)Response

　　　　Status Code-------状态码

　　　　Header--------响应头

　　　　Body--------响应体

三：HTTP状态码

　　200：成功，这个成功只是表示服务器正常处理完成了，并不能表示逻辑的正确性

　　301，320：跳转，一般可以在header中看到location，即跳转地址，区别是一个是临时跳转一个是固定跳转

　　304：未修改，服务器发现资源文件标识未变动，通知客户端读取本地缓存文件即可

　　400：客户端请求信息格式问题

　　403：一般是禁止访问，比如文件，目录等存在，但做了访问限制

　　404：一般为文件，目录不存在，但也可以将其他情况伪装成为不存在

　　500：出现这个一般都是服务端的代码直接抛出异常导致

　　502，503，504：这个类似，在网络异常等情况下都可以出现，也有很多代码抛出错误时候出现

 

四：HTTP常规Header信息与作用（Request）

　　Host：必须存在，域名指定（类似与分类，但端口用于区分访问那个域名）

　　Accept：表示自身可接受的信息类容，类似建议，有子项

　　User-Agent：客户端标识信息（系统版本，浏览器，内核等）

　　Cookie：特殊的信息存储位置，用于自动交互，无需代码干涉

　　Referer：来源，即通过什么页面或文件触发的请求，如果是浏览器地址栏回车则没有该值

　　Connection：控制长短链接，告诉对方当前链接状态（Keep-Alive，Close）

　　Range：指定返回信息范围（断点持续子类使用）

　　Content-Type：请求正文的类型，编码等信息

　　Content-Length：请求正文长度

　　If-Modifiled-Since：缓存相关，本地文件的标识有效期

　　If-None-Match：缓存相关，本地文件的特征码，对应返回信息中的ETag

 

五：HTTP常规Header信息与作用（Reaponse）

　　Date：时间，一般是服务器当前时间

　　Content-Encoding：返回正文的压缩编码类型

　　Content-Length：返回正文的长度

　　Content-Type：返回正文的类型，编码等信息

　　Cache-Control：缓存机制以及策略，时间，方式等

　　Etag：返回文件信息的特征码

　　Expires：返回文件信息的缓存有限期

　　Set-Cookie：要求设置的Cookie，可以多次出现的头信息

　　Location：自动重定向到其他新的地址，一般状态301，302时会出现

　　Connection：控制长短链接，告诉对方当前链接状态，默认Keep，当双方都为Keep时则链接会在下次沿用

例子：

```
General:
Request URL: https://www.runoob.com/wp-content/themes/runoob/assets/images/qrcode.png
Request Method: GET
Status Code: 200 OK (from memory cache)
Remote Address: 211.138.124.224:443
Referrer Policy: no-referrer-when-downgrade

Resoponse-Headers:
Accept-Ranges: bytes
Age: 33743
Ali-Swift-Global-Savetime: 1556273201
Cache-Control: max-age=86400
Connection: keep-alive
Content-Length: 12595
Content-Type: image/png
Date: Sat, 11 May 2019 17:08:02 GMT
EagleId: d38a7cce15576282256397552e
Etag: "59f41d3a-3133"
Expires: Sat, 11 May 2019 10:41:15 GMT
Last-Modified: Sat, 28 Oct 2017 06:01:30 GMT
Server: Tengine
Timing-Allow-Origin: *
Vary: Accept-Encoding
Via: cache1.l2cm9[111,304-0,C], cache46.l2cm9[113,0], cache6.cn456[0,200-0,H], cache6.cn456[1,0]
X-Cache: HIT TCP_MEM_HIT dirn:5:87260959
X-M-Log: QNM:xs1167;QNM3
X-M-Reqid: byYAAFmhB_Xtr50V
X-Qnm-Cache: Hit
X-Swift-CacheTime: 86400
X-Swift-SaveTime: Sat, 11 May 2019 17:08:02 GMT

Request-Headers:
Provisional headers are shown
Referer: https://www.runoob.com/html/html-tutorial.html
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537
```


