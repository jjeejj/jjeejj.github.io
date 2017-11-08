---
title: 一个由重定向到https引发的问题
tags: [other]
---
-------------------------------------------------

今天同事问了我一个问题，爬取一个网页搜索的数据，`POST`方法请求一个接口的数据，请求成功了，返回的数据始终是空数组。但是在网页上直接访问是可以查出数据的。参数什么都是正确的。。。。这是为什么呢？

-------------------------------------------------
<!--more-->

对于上面的问题，我亲自调试了一下，发现请求代码的`API`调用时没有问题的，请求参数也是正确的，必要的`header`也的确添加上了。
打断点看返回的`res`,突然发现`req.method`为`Get`,这下感觉有思路了，我明明发送的是`POST`请求，为什么`res`返回的结果请求的方法是`Get`呢，
这应该是发生重定向了`301或302`了，最后发现协议写成`http`了，去请求的时候跳转到对应的`https`了。

那么问题来了：
1. 怎么才能让`http`自动跳转到`https`呢？
2. 为什么跳转后请求的方法改变了？

下面就分别来解答一下的。

:point_right:  `http`自动跳转到`https`

下面以`nginx`做为例子：
```nginx
server {
    listen 80;
    listen 443;
    ssl on;
    ssl_certificate /PATH_TO_CRT;
    ssl_certificate_key /PATH_TO_key;
    server_name test.com;
    if($scheme!=https){
        rewrite ^/(.*)https://$server_name/$1 permanent;
    }
}
```