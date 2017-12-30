---
title: 为hexo博客添加RSS订阅功能
tags:
  - hexo
date: 2017-12-30 10:49:07
---

------------------------------------------------

`RSS(Really Simple Syndication)` 简易信息聚合,在互联网上被广泛采用的内容包装和投递协.

是一种描述同步网站内容的格式,使用`xml`格式. 当网站内容更新时,可以通过订阅`RSS`源在`RSS`阅读器上获取更新的信息

大多数内容提供的网站都会提供`RSS`订阅的功能,方便用户去获取最新的内容.

本篇文章主要介绍怎么给自己的`hexo`博客添加`RSS`源 

-------------------------------------------------
<!--more-->

在`hexojs`用户下的仓库中发现两个`RSS`功能的`npm`包
1. [hexo-migrator-rss](https://github.com/hexojs/hexo-migrator-rss)
2. [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)

不过第一个包是从 `RSS` 迁移所有文章到`source/_posts`文件夹中的,第二个才是生成`RSS`文件的包.

下面就介绍一下**hexo-generator-feed**的使用


##### hexo-generator-feed

首选先安装这个包:`npm install hexo-generator-feed`

然后在在`_config.yml`文件中配置该插件
```yml
feed:
    type: atom
    path: atom.xml
    limit: 20
    hub:
    content:
    content_limit:
    content_limit_delim: ' '
```

参数的含义：
* `type`: `RSS`的类型(`atom/rss2`)
* `path`: 文件路径,默认是`atom.xml/rss2.xml`
* `limit`: 展示文章的数量,使用**0**或则**false**代表展示全部
* `hub`: 
* `content`: 在`RSS`文件中是否包含内容 ,有3个值 `true/false`默认不填为`false`
* `content_limit`: 指定内容的长度作为摘要,仅仅在上面`content`设置为`false`和`没有自定义的描述出现`
* `content_limit_delim`: 上面截取描述的分隔符,截取内容是以指定的这个分隔符作为截取结束的标志.在达到规定的内容长度之前最后出现的这个分隔符之前的内容,，防止从中间截断.

>此外还有一种方法,就是在`Next`主题的`_config.yml`文件中有个`rss`的配置，直接设置为`true`就可以了
>![alt](/images/为hexo博客添加RSS订阅功能/next_rss.jpg)


配置好之后运行`hexo g`就可以找到你博客的`pubilc` 文件夹下发现`atom.xml`文件了

![alt](/images/为hexo博客添加RSS订阅功能/atom_xml.jpg)

然后运行`hexo`服务就可以在个人站点处看到`RSS`的订阅图标了,点击这个图标就可以出现`RSS`订阅的地址,就可以添加到你的`RSS`阅读器方便查看博客的最新文章
![alt](/images/为hexo博客添加RSS订阅功能/rss_logo.jpg)

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**