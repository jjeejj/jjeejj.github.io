---
title: postman发送请求使用篇(二)
tags:
  - postman
date: 2017-11-13 22:34:07
---
------------------------------------------------------------

`postman` **发送请求使用篇**

-----------------------------------------------------------
<!--more-->

#### Requests

在`Builder`选项卡下面，可以让你很快的创建`Http`请求，包含四部分：`URL`,`Method`,`headers`,`body`


##### URL
这是发送一个请求需要设置的第一件事情,`URL`输入框会保存之前使用过的`URL`,当你输入的时候回自动下拉显示出来。

点击**Params**那按钮，打开输入URL参数的编辑器，你可以添加键-值对，postman会自动合并为`query string`放到URL参数上。如果URL上已经有参数，会自定分割参数到数据编辑中
![alt](/images/postman/request_params.png)

你在参数输入框中输入的数据不会自动`URL-encoded`,你可以按照下面这个做，进行`encoded`,第一步点击右边的**Bulk Eit**然后选中需要编码的数据，右键选着`EncodeURIComponent`
![alt](/images/postman/request_encodeurl.png)

>postman会自定添加`http://`在url开头，如果没有指定协议的话

一些API端点出是路径变量：
`https://api.library.com/:entity/`

为了编辑路径变量，点击**Params**,可以看到已经作为一个`key`存在，根据你的需要更新这个值。
![alt](/images/postman/request_path_variable.png)

##### Headers

点击`Headers`功能会打开`headers`键-值对的编辑框，你可以设置任意的头名称，将会自动下拉出现公共的`HTTP`头类型，而且`Content-Type`类型的值也会自动下拉出现

>受限制的`headers`:如果你使用Chrome app，一些头字段是受Chrome和XMLHttpRequest 规范限制的，你可以使用 **Interceptor extension**发送受限制的头信息

##### Cookies

Cookie可以使用`manage cookies`功能被管理在`native apps`,管理每个域名下面的`Cookie`
![alt](/images/postman/request_cookie_managed.png)

##### Header presets(预先设置)

你可以保存你经常使用的`headers`,在`header preset`中，在`Headers tab`下面。当你在`Headers`输入框中输入时，会自动下拉出来
![alt](/images/postman/header_presets.png)

##### Method

直接修改请求的方法
![alt](/images/postman/request_method.png)

##### Body

当构建一个请求的时候，你可能需要一些请求体来进行工作.请求体的编辑区域根据不同的请求体类型分为四个区域

当你通过http协议发送一个请求的时候，服务器期望接收一个`Content-Type`头信息，这个`Content-Type`头信息让服务器正确的解析请求体信息。对于`form-data`和`urlencoded `类型，postman会自动设置`Content-Type`，不需要你去设置它。对于`raw`模式会根据你选着的格式类型去设置，如果你手动设置就会覆盖postman设置的值。对于`binary`postman不设置任何的`Content-Type`类型
![alt](/images/postman/request_body.png)

>form-data----->multipart/form-data
>x-www-form-urlencoded---->application/x-www-form-urlencoded


![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**