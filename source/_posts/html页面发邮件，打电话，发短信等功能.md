---
title: html页面发邮件，打电话，发短信等功能
date: 2016-11-01 15:25:09
tags: [html]
---
----------------------------
如果需要在移动浏览器中实现拨打电话，调用sms，发送email等功能,可采用url href链接的方式。
在Safari，iOS，Android 浏览器，webos浏览器，塞班浏览器，IE，Operamini等主流浏览器，进行拨打电话功能。

<!--more-->

## 拨打电话

在电话号码前面可以加上 + （加号）表示国际号码。如：

**在html页面实现一键拨号的功能**
```html
<a href="tel:10086">拨打10086</a>
```
	
**使用wtai协议进行拨打电话**
```html
<a href="wtai://wp/mc;10086">mc;10086</a>
```

## 发短信

如果是需要调用短信的接口，可以将链接写成下面的格式：

	sms:<phone_number>[,<phone_number>]*[?body=<message_body>]

>格式中的 '*' 代表数量任意个

例如：
```html
<a href="sms:10086,10010?body=hello world">给 10086 和 10010 发送内容为"hello world"的短信</a>
```

## 邮件

在`href` 中 使用 `mailto`

格式为：
	
	mailto:<recipients>[,<recipients>]* [?<cc=>]* [&<bcc=>]* [&<subject=>] [&<body=>]
	
>注意：在添加这些功能时，第一个功能以"?"开头，后面的以"&"开头
>多个联系人之间可以用','或';'分割开
>格式中的 '*' 代表数量任意个
>cc:抄送地址
>bcc:密件抄送地址

* 只有收件人：
```html
<a href="mailto:test1@163.com">mail test1</a>
<a href="mailto:test1@163.com,test2@163.com">mail test1 and test2</a>
<a href="mailto:test1@163.com;test2@163.com">mail test1 and test2 ;</a>
```

* 在收件人地址后用?cc=开头，填写抄送地址
```html
<a href="mailto:test1@163.com?cc=lina@qq.com">mail test1</a>
```

* 紧跟着抄送地址之后，写上&bcc=，填上密件抄送地址
```html
<a href="mailto:test1@163.com?cc=lina@qq.com&bcc=test2@163.com">mail test1</a>
```

* 包含主题，用?subject=可以填上主题
```html
<a href="mailto:test1@163.com?cc=lina@qq.com&bcc=test2@163.com&subject=邀请函">mail subject</a>
```

* 包含内容，用?body=可以填上内容,内容包含文本，使用%0A给文本换行
```html
<a href="mailto:test1@163.com?cc=lina@qq.com&bcc=test2@163.com&subject=邀请函&body=hello world%0A你好">mail body</a>
```

* 内容包含链接，含http(s)://等的文本自动转化为链接
```html
<a href="mailto:test1@163.com?body=https://jjeejj.github.io">mail https</a>
```

* 内容包含图片，PC端不支持(不兼容),android存在兼容问题
```html
<a href="mailto:test1@163.com?body=https://www.baidu.com/img/baidu_jgylogo3.gif' width='200' height='200'>">mail images</a>
```

## Android Market
希望一个链接能够激活Android市场的功能，可以把链接写成
```html
<a href="market://search?q=[query]">Android Market link</a>
```

比如
```html
<a href="market://search?q=微信">Android Market link</a>
```

## 地图定位GPS
格式：
	
	 <a href="geopoint:[经度],[纬度]">我的位置</a>
```html
<a href="geopoint:108.954823,34.275891">我的位置</a>
```

>     注意：实际操作没有起作用，待定验证