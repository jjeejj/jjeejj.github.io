---
title: Base64原理解析
tags:
  - other
date: 2017-11-28 21:40:10
---

-------------------------------------------------------------

**Base64**是基于64个字符进行转换的,因为2的6次方正好为64，所以**6bit**就可以表示出64个字符，因此在转换的过程中以**6bit**表示一个字符。

-----------------------------------------------------------

<!--more-->

**原理：**

**3x8=4x6**,核心是这个公式

1. base64的编码都是按字符串长度，以每3个8bit的字符为一组
2. 针对每组，首先获取每个字符的ASCII编码值
3. 将ASCII编码转换成8bit的二进制，得到一组3*8=24bit的字节
4. 再将这24bit划分为4个6bit的字节，并在每个6bit的字节前面都填两个高位0，得到4个8bit的字节
5. 将这4个8bit的字节转换成10进制，对照Base64编码表,得到对应编码后的字符

**ASCII表**
![alt](/images/Base64原理解析/ascii_sheet.png)

**Base64编码表**
![alt](/images/Base64原理解析/base64_sheet.png)

**案例**

* 字符串长度能被3整除的:比如 wen

```
         w           e          n
ASCII:  119         101        110
8bit:  01110111   01100101   01101110
6bit:  011101  110110  010101  101110
十进制:  29      54      21      46
Base64:   d       2       V       u

所以 "wen" ---- > d2Vu
```

* 字符串长度不能被3整除时,比如：jiang

```
         j          i           a          n           g
ASCII:  106        105         97          110         103
8bit:  01101010   01101001   01100001   01101110     01100111
6bit:  011010  100110  100101  100001  011011 100110  011100  异常
十进制:  26      38      37      33       27     38    28
Base64:   a       m       l      h         b      m     c      =

所以 "jiang" ---- > amlhbmc=
```


注意：**Base64是四个字符为一组，不够的补=**

>工具网站：
>[进制转换](https://tool.lu/hexconvert/)
>[Base64转换工具](http://tool.chinaz.com/Tools/Base64.aspx)
>[图片转base64](https://c.runoob.com/front-end/59)

**汉字转Base64**

这里需要注意，汉字本身可以有多种编码，比如`gb2312、utf-8、gbk`等等，每一种编码的`Base6`4对应值都不一样。下面的例子以`utf-8`为例

`严`的`utf-8`为`E4B8A5`,写成二进制就是三字节的11100100 10111000 10100101,然后按照上面的规则转换得到`Base64` 编码为：5Lil

所以，汉字严（utf-8编码）的Base64值就是5Lil

***从上面英语字母或者汉字转换为Base64的来看，就是先转换为对应的编码的二进制，然后在进行转换***

**补充内容**

对于一张图片，我们进程看到这样的表达形式：
```
data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAAkCAYAAABIdFAMAAAA....
```
这是`这是Data URI scheme`目的是将一些小数据，直接嵌入到网页中，而不用在引用外部

在上面`data`表示取得数据的协定名称,`image/png` 是数据类型名称,`base64` 是数据的编码方法,逗号后面就是这个image/png文件base64编码后的数据

Data URI scheme支持的类型有:

* data:,文本数据
* data:text/plain,文本数据
* data:text/html,HTML代码
* data:text/html;base64,base64编码的HTML代码
* data:text/css,CSS代码
* data:text/css;base64,base64编码的CSS代码
* data:text/javascript,Javascript代码
* data:text/javascript;base64,base64编码的Javascript代码
* data:image/gif;base64,base64编码的gif图片数据
* data:image/png;base64,base64编码的png图片数据
* data:image/jpeg;base64,base64编码的jpeg图片数据
* data:image/x-icon;base64,base64编码的icon图片数据

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**