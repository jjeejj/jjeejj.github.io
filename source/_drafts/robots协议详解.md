---
title: robots协议详解
tags: [other]
---
----------------------------------------------
Robots协议（也称为爬虫协议、机器人协议等)的全称是“网络爬虫排除标准”（Robots Exclusion Protocol）,站通过Robots协议告诉搜索引擎哪些页面可以抓取，哪些页面不能抓取。[官方网站](http://www.robotstxt.org/)

<!--more-->
#### 简介
`robots.txt`是一个文本文件，使用任何一个编辑器都可以创建它。`robots.txt`是一个协议而不是一个命令。`robots.txt`是搜索引擎访问网站时的第一个要查看的文件，告之服务器上那些文件内容是可以抓去的。

当一个搜索引擎访问一个站点的时候，首先检查该站点的**根目录**下是否有`robots.txt`，如果存在，搜索机器人就会按照该文件中的内容来确定访问的范围；如果该文件不存在，所有的搜索蜘蛛将能够访问网站上所有没有被口令保护的页面。

#### 原则
`Robots`协议是国际互联网界通用的道德准则，基于以下原则建立：
* 索技术应服务于人类，同时尊重信息提供者的意愿，并维护其隐私权；
* 网站有义务保护其使用者的个人信息和隐私不被侵犯

#### 功能
##### 文件写法
```
User-agent: * //这里的 * 代表所有的搜索引擎
Disallow: /admin/ //这里定义是禁止爬寻admin目录下面的目录
Disallow: /require/ //这里定义是禁止爬寻require目录下面的目录
Disallow: /cgi-bin/*.html //禁止访问/cgi-bin/目录下的所有以".html"为后缀的URL(包含子目录)
Disallow: /*?* //
Disallow: /.jpg$ //禁止抓取网页所有的.jpg格式的图片
Disallow: /ab/adc.html //禁止爬取ab文件夹下面的adc.html文件
Allow: /cgi-bin/ //这里定义是允许爬寻cgi-bin目录下面的目录
Allow: /tmp //这里定义是允许爬寻tmp的整个目录
Allow: .htm$ //仅允许访问以".htm"为后缀的URL
Allow: gif$ //允许抓取网页和gif格式图片
Sitemap: //网站地图 告诉爬虫这个页面是网站地图
```
>增加限制目录每个路径之前都要包含

###### 用法
1. 禁止所有搜索引擎访问网站的任何部分
```
User-agent: *
Disallow: /
```
2. 允许所有的搜索引擎访问
```
User-agent: *
Allow: /
```
>或者建一个`robots.txt`空文件
3. 禁止某个搜索引擎访问
```
User-agent: Baiduspider
Disallow: *
```
4. 允许某个搜索引擎访问
```
User-agent: Baiduspider
Allow: *
```
5. 一个综合的例子(在这个例子中，该网站有三个目录对搜索引擎的访问做了限制，即搜索引擎不会访问这三个目录)

>要注意的是对每一个目录必须分开声明，而不要写成 “Disallow: /cgi-bin/ /tmp/”

6. 分析淘宝的robots.txt文件
```
User-agent:  Baiduspider
Allow:  /article
Allow:  /oshtml
Allow:  /wenzhang
Disallow:  /product/
Disallow:  /

User-Agent:  Googlebot
Allow:  /article
Allow:  /oshtml
Allow:  /product
Allow:  /spu
Allow:  /dianpu
Allow:  /wenzhang
Allow:  /oversea
Disallow:  /

User-agent:  Bingbot
Allow:  /article
Allow:  /oshtml
Allow:  /product
Allow:  /spu
Allow:  /dianpu
Allow:  /wenzhang
Allow:  /oversea
Disallow:  /

User-Agent:  360Spider
Allow:  /article
Allow:  /oshtml
Allow:  /wenzhang
Disallow:  /

User-Agent:  Yisouspider
Allow:  /article
Allow:  /oshtml
Allow:  /wenzhang
Disallow:  /

User-Agent:  Sogouspider
Allow:  /article
Allow:  /oshtml
Allow:  /product
Allow:  /wenzhang
Disallow:  /

User-Agent:  Yahoo!  Slurp
Allow:  /product
Allow:  /spu
Allow:  /dianpu
Allow:  /wenzhang
Allow:  /oversea
Disallow:  /

User-Agent:  *
Disallow:  /
```

