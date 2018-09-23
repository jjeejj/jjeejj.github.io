---
title: 为hexo博客添加service worker的功能
tags:
  - hexo
date: 2018-09-23 10:40:48
---

----------------------------------------------------------------------------

为 `hexo` 博客添加 `Service worker` 功能,方便离线访问

----------------------------------------------------------------------------
<!--more-->

首先先说明一下,对于用在 `hexo` 上的`Service worker` 插件,现在`github` 上一共有两个插件:

1. [hexo-offline](https://github.com/JLHwung/hexo-offline)
2. [hexo-service-worker](https://github.com/zoumiaojiang/hexo-service-worker)

对于这两个插件的异同,分别是:

**相同点**
- 都是使用 [sw-precache](https://github.com/GoogleChromeLabs/sw-precache) 这个模块为基础
- 由于 `hexo-service-worker` 是参考 `hexo-offline` 所以配置语法是一致的

**不同点**
- `hexo-service-worker` 对每个 html 页面都会单独有注册 `service worker` 的入口，这样当用户访问任何页面的时候都能够进行缓存
- `hexo-service-worker` 在检测更新机制方面做了一些工作，当你的 `hexo` 站点有更新，有一种机制可以马上通知到用户端

-----------------------------------------------------------------------------------------

所以下面,就用 `hexo-service-worker` 来为 `hexo` 添加`Service worker` 功能

**安装**

`npm i hexo-service-worker --save`

然后激活插件 : `hexo clean && hexo g`

**配置**

```yml
service_worker:
  maximumFileSizeToCacheInBytes: 5242880
  staticFileGlobs:
  - public/**/*.{js,html,css,png,jpg,gif,svg,eot,ttf,woff,woff2}
  stripPrefix: public
  verbose: true
```

>但是这样会有一个问题: 菜单图表会不显示了,所有本网站的博客,没有使用`Service worker` 功能,只是看看怎么配置的 

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**


