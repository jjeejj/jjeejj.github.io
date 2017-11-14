---
title: postman响应使用篇(三)
date: 2017-11-14 21:18:36
tags: [postman]
---
------------------------------------------------------------

`postman` 的**响应使用篇** 

-----------------------------------------------------------
<!--more-->

#### Responses

确保`API`响应正确是在使用`API`时要做的事情.
一个`API`响应包含响应体**body**，响应头**headers**,状态码**status code**
postman组织响应体和响应头在不同的`tab`.状态码,调用`API`响应的时间，响应的大小在旁边的`tab`显示,你可以把鼠标放上去显示描述的详细的信息
![alt](/images/postman/response_all.png)

##### Saving responses 保存响应

![alt](/images/postman/response_save.png)
如果一个请求已经保存在收藏夹中，你可以为这个请求保存这个响应。
一旦这个响应返回，点击保存按钮，输入一个保存响应的名称。当你加载这个请求时，所有为这个请求保存的响应都是可以使用的。点击 **Examples** 下拉按钮，可以选择你要查看的响应
![alt](/images/postman/response_save_view.png)

##### responses body 查看响应体

postman响应体**Body**工具栏提供几个工具帮你很快的查看内容。
有三种查看方式:  pretty, raw, preview
![alt](/images/postman/response_body_view.png)

**Pretty**

以漂亮的模式去格式化响应的JSON或者XML方便更好的去查看。在这个模式中响应数据内容中的链接是高亮显示并且可以点击去发送请求的。还可以点击下拉选项选择以什么方式去格式化响应的数据

----------------------------------------------------------
postman 是根据响应的`Content-Type header`的值去确定自动格式化的方式，如果postman没有自动格式化，你可以强制格式化通过`JSON`或者`XML`.你可以在`Settings`中的`General` 的 `Language detection`进行下拉选择
![alt](/images/postman/response_view_formatter.png)

--------------------------------------------------------
**Raw**

仅仅是响应体的一个大文本，可以告诉你响应体是否压缩了

--------------------------------------------------------
**Preview**

在一个沙盒的`iframe` 中渲染响应的内容。一些web框架默认的返回错误的`HTML`，这时候`Preview` 是非常有用的。由于`iframe`沙盒的限制，JS和图片是不可以用的

--------------------------------------------------------

如果`API`请求返回一个图片，postman将识别并自动渲染它.对于二进制的响应类型，你可以选择**“Send and download** 保存响应到硬盘上。你可以使用合适的方式查看他.可以灵活的测试`audio files, PDFs, zip files, or anything`
![alt](/images/postman/send_download.png)

##### responses Headers 查看响应头

展示键-值对，鼠标放到`header name` 能够给出详细的描述。
如果你发送一个`HEAD` 请求，将默认展示`Headers` tab

##### Response time 响应时间

postman自动计算服务器响应到达的时间，这对平台的初步测试时非常有用的的

##### Response size 响应大小

postman 分解响应的的大小为`body`和`headers`，响应的大小是近似计算的

##### Response Cookie 响应Cookie

服务器返回的`Cookie`在一个专门的tab中。在`native apps`你可以利用**manage cookies**的功能去管理`Cookie`,在`Chrome app` 你可以用`Interceptor extension`去管理`Cookie`

##### Response Tests 响应测试

随着你收到服务器响应请求的数据同时，你也可以看到一些测试结果。
对于该部分会放到`Scripts`脚本篇进行详解
![alt](/images/postman/response_test.png)

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**