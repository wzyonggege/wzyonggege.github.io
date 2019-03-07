---
layout:     post
title:      "Python Requests"
subtitle:   "Requests is the only Non-GMO HTTP library for Python, safe for human consumption."
date:       2017-04-11 12:00:00
author:     "Jam"
header-img: "img/about-bg.jpg"
tags:
    - Code
    - Python
    - Requests
    - Python library
---


requests是python的一个HTTP客户端库，非常非常的简单易学。

>  本文Python代码版本2.7


### 1，安装
使用requests前需要来安装，win的同学点击这里进入官方文档可以下载。也可以pip安装

<pre>
pip install requests
</pre>

（pip和easy_install是python管理包的工具）
安装完了之后可以在命令行或者ide中

<pre>
import requests
</pre>

没有报错表示安装成功


### 2，游戏开始
我们先拿一个网站来试试，就你了豆瓣电影

<pre>
import requests 
url = 'https://movie.douban.com/' 
r = requests.get(url).text 
print r
</pre>

运行代码，如果看到输出一大串的html源代码，就表明成功！！

回到代码来

<pre>
r = requests.get("https://movie.douban.com/")
</pre>
这里有个http请求，就是get方式，但是http有八种方式都可以用


<pre>
url = 'https://movie.douban.com/'
r = requests.get(url) 
r = requests.post(url) 
r = requests.delete(url) 
r = requests.head(url) 
.....
</pre>

我们常用的get和post两种，post这里先不讲，大家记住get的方式就好了。

再来看看requests之后的响应内容

<pre>
# coding:utf-8 
import requests 
url = 'https://movie.douban.com/' 
r = requests.get(url) 

# 应答码 
print r.status_code 

# 网页响应内容 
print r.text 

# 二进制响应内容 
print r.content 

# 编码 
print r.encoding 

# 请求头信息 
print r.headers
</pre>