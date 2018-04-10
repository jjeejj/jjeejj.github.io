---
title: Object的toLocaleString方法详解
tags: [js]
---
-----------------------------------------------

`toLocalString`返回特定语言下的字符串,默认对应的是本地的语言.

这个方法平常用到的非常少,但是它的功能是非常强大的,下面就分别介绍一下在`Number`和`Date`下的用法

---------------------------------------------

<!--more-->

#### Number.prototype.toLocaleString()

该方法返回这个数字在特定语言环境下的表示字符串

##### 语法

`numObj.toLocaleString([locales[,options]])`

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString#Checking_for_support_for_locales_and_options_arguments

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString#Example:_Checking_for_support_for_locales_and_options_arguments