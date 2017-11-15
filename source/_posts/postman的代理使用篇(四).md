---
title: postman的代理使用篇(四)
date: 2017-11-15 22:18:36
tags: [postman]
---
------------------------------------------------------------
`postman` 的**代理使用篇**(四)

-----------------------------------------------------------
<!--more-->

#### Proxy

##### proxy（代理） 是什么

在一个基本的网络会话中，一个客户端发送一个请求到服务器，服务器返回一个响应
![alt](/images/postman/standard_request.png)

一个代理服务是一个应用程序或者系统,作为一个中介在你的电脑和互联网之间。
客户端和服务器，代理以你的名义向网站，服务发送请求
![alt](/images/postman/standard_request_proxy.png)

代理服务可以在你本地的机器上，或者你的网络上的一些地方，或者在你的客户端和目标服务之间的任何地方

##### Configuring proxy settings 配置设置你的代理

下面讲的是如何在postman上配置代理,让所有的请求通过一个代理服务区请求对应的服务器。

`postman native app` 支持配置代理，你可以指定用**custom proxy**或者使用在操作系统定义的**system proxy**
![alt](/images/postman/proxy_setting.png)

如果你想在你应用使用相同的代理，你可以使用**system proxy**;
如果你想让来自postman的请求通过自定义代理服务器，请使用**custom proxy**

###### custom proxy

postman 允许你自定义`HTTP`或者`HTTPS`代理在postman上.
换句话说：在postman上发送的所有请求都会通过你选择的代理服务器上.

过程：
    1. postman客户端发送一个请求到你选的代理服务器
    2. 代理服务器发送请求到目标服务器
    3. 目标服务器通过代理服务器进行响应
![alt](/images/postman/custom_proxy_view.png)

自定义代理服务设置默认是关闭的，你可以通过右边的开关打开

适当的选择代理服务器类型，默认的`HTTP`和`HTTPS`都被选中的，这就意味着所有的`HTTP`和`HTTPS`请求都会经过代理服务器

>注意：输入代理服务地址的时候不带`protocol`

###### system proxy

如果你的所有的应用都使用相同的 代理，那么你就就有可能在操作系统级别有一个默认的代理配置。

使用**system proxy settings** 转发你在postman上`HTTP`或者`HTTPS`的请求通过你的操作系统默认的代理配置

过程：
    1.postman客户端发送一个请求通过操作系统默认配置，它转发你的请求到代理服务器
    2. 系统代理服务器发送请求到目标服务器
    3. 目标服务器通过系统配置代理服务器进行响应
![alt](/images/postman/system_proxy_view.png)

系统代理配置默认是开启的，所有的postman请求都会通过系统代理


>如果**System Proxy**和**Custom Proxy**都开启了，会优先考虑**Custom Proxy**

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**