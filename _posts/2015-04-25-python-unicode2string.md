---
layout: post
title: "python string2unicode unicode2string"
subtitle:   "decode() encode()"
date:       2015-04-25 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

* string2unicode:decode()
* unicode2string:encode()

### [关于requests库](http://python.jobbole.com/81244/)

requests是一个很实用的Python HTTP客户端库，编写爬虫和测试服务器响应数据时经常会用到。

其中的Request对象在访问服务器后会返回一个Response对象，这个对象将返回的Http响应字节码保存到content属性中。

但是如果你访问另一个属性text时，会返回一个unicode对象，乱码问题就会常常发成在这里。

因为Response对象会通过另一个属性encoding来将字节码编码成unicode，而这个encoding属性居然是responses自己猜出来的。

官方文档：
  
  * Content of the response, in unicode.
  
  * If Response.encoding is None, encoding will be guessed using chardet.
  
  * The encoding of the response content is determined based solely on HTTP headers, following RFC 2616 to the letter. If you can take   advantage of non-HTTP knowledge to make a better guess at the encoding, you should set r.encoding appropriately before accessing this   property.
所以要么你直接使用content(字节码)，要么记得把encoding设置正确，比如我获取了一段gbk编码的网页,就需要以下方法才能得到正确的unicode。

~~~python
import requests
url = "http://xxx.xxx.xxx"
response = requests.get(url)
response.encoding = 'gbk'
 
print response.text
~~~
