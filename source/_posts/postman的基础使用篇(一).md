---
title: postman 的基础使用篇(一)
tags:
  - postman
date: 2017-11-12 22:34:07
---

------------------------------------------------------
`postman` 是接口调试的利器，之前以`chrome`插件的形式，现在也有原生APP。

---------------------------------------------------------

<!--more-->

#### 安装

##### native apps

直接官方网站下载即可

[download page](https://www.getpostman.com/apps)

##### Chrome app

去`chrome`应用商店直接搜索`postman`下载安装即可

#### 数据迁移

1. 如果你有账户，登录账户后就会自动同步
2. 如果没有账户：
    * 你可以导出全部数据，然后在导入数据
        ![alt](/images/postman/export.png)
    * 你也可以导出你想要一部分数据
        ![alt](/images/postman/export_less.png)

#### 发送请求

##### 操作步骤：
![alt](/images/postman/request_first.png)

##### 原理：
![alt](/images/postman/postman_view.png)

#### 创建收藏夹

正常情况下，每个请求都会在左边栏的`History`留下记录，但是请求多的话，你想要重复某个请求就不方便了,所以可以把你想保留的请求按照分类收藏起来，以方便以后使用。

![alt](/images/postman/postman_collection.png)

#### 设置

在App头部的导航栏，点击小扳手的图标，选中`Settings`打开设置的弹出框
![alt](/images/postman/postman_setting.png)

##### General
   
* Trim keys and values in request body(去除请求体中的键和值的空格)：如果使用`form-data`或者`url-encoded` 应该关闭开功能
* Language detection(语言判断)：设置成`JSON`将强制以`JSON`进行渲染，和响应头的`Content-Type`无关
* Request Timeout in ms(0 for infinity)：设置请求超时的时间
* Send no-cache header：发送一个`no-cache`请求头，获取服务器最新的资源
* Automatically follow redirects：防止服务返回300系列的状态，自动重定向
* Send anonymous usage data to Postman：是否循序匿名向postman发送数据

    其他的都可以默认配置
![alt](/images/postman/postman_general.png)

##### Themes
有黑白两种主题可以选择

##### Shortcuts
应用快捷键

##### Data
数据的导入导出

##### Add-ons 
postman的命令行协助扩展工具`newman`，可以让postman的collection和构建系统整合起来，
可以通过corn自动测试API接口[文档](https://www.getpostman.com/docs/postman/collection_runs/command_line_integration_with_newman)

##### Sync
同步账户数据

##### Certificates
在一个基础域名下。添加客户端证书

##### Proxy
设置API请求的代理

##### Update
应用更新

##### About
应用的信息


![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**