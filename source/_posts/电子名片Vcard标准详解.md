---
title: 电子名片Vcard标准详解
tags:
  - tools
date: 2017-07-23 21:02:59
---

----------------------------------------------
**vCard是电子名片的文件格式标准，vCard 可包含的信息有：姓名、地址资讯、电话号码、URL，logo，相片等。**

**现在的手机通讯录都是利用的这个标准,可以生成二维码名片，然后扫描，自动对应保存到手机通讯录**

----------------------------------------------

<!--more-->

#### 基本信息

* 扩展名： `.vcf, .vcard`
* MIME type/content type : `text/x-vcard ,text/directory;profile=vCard,text/directory`
* 格式的拥有权: 互联网邮件联盟
* 统一类型标识: `public.vcard`
* 类型代码: `vCrd`

#### 实例详解
参考一个vcard的例子内容:

```vcf
BEGIN:VCARD
VERSION:2.1
N:Gump;Forrest
FN:Forrest Gump
NICKNAME: dahelle
ORG:Gump Shrimp Co.
TITLE:Shrimp Man
LOGO;ENCODING=b;TYPE=JPEG:MIICajCCAdOgAwIBAgICBEUwDQYJKoZIhvcN AQEEBQAwdzELMAkGA1UEBhMCVVMxLDAqBgNVBAoTI05ldHNjYXBlIENvbW11bm
TEL;WORK;VOICE:(111) 555-1212
TEL;TYPE=HOME,VOICE:(404) 555-1212
ADR;WORK:;;100 Waters Edge;Baytown;LA;30314;United States of America
LABEL;TYPE=WORK;ENCODING=QUOTED-PRINTABLE:100 Waters Edge=0D=0ABaytown, LA 30314=0D=0AUnited States of America
ADR;TYPE=HOME:;;42 Plantation St.;Baytown;LA;30314;United States of America
LABEL;TYPE=HOME;ENCODING=QUOTED-PRINTABLE:42 Plantation St.=0D=0ABaytown, LA 30314=0D=0AUnited States of America
EMAIL;TYPE=PREF,INTERNET:forrestgump@walladalla.com
REV:20080424T195243Z
END:VCARD
```

**Vcard数据格式行为：类型[;参数]:值**

`Vcard`内容必须以`BEGIN:VCARD`开头，以`END:VCARD`结尾
`VERSION`: 指定Vcard使用的vcard规范的版本
`NICKNAME`: 别名
`FN`: vcard对象的名称，一个vcard对象必须包含FN类型,表示姓名
`N`: FN表示一个vcard对象的名称，N表示这个对象名称的组成部分,各个组成部分可以用分号分号，每个组成部分可以用逗号
`ORG`: 表示一个组织的名称,公司的名称
`LOGO`: 公司logo，是一个图像信息,TYPE知道图像的格式，ENCODING=b表示是二进制的数据流
`TITLE`: 职称
`TEL`: 指定一个电话号码,`TYPE`参数类型为：

     home :表示家庭电话
     work：工作电话
     msg：表示这个号码支持语音
     pref：表示多个电话中最喜欢使用的电话
     voice：声音电话号码(缺省值)
     fax：传真号码
     cell：表示手机电话
     video：视频电话
     pager：传呼机
     modem： 调制解调器电话
     car：车载电话
     isdn：ISDN连接电话号码
     pcs：个人通信服务电话

`ADR`:是一个组合，用来表示一个地址信息，值类型是一个用分号分开的文本值,缺省的`"TYPE=intl,postal,parcel,work"`，可以替换,`TYPE`参数类型为：

     home :居住地址
     work：工作地址
     dom：国内地址
     intl：国际地址
     pref：有多个地址的时候，优先的地址
     parcel：包裹递送地址
     postal：邮政地址

值组合由一下部分顺序的组成：
`the post office box(邮政信箱);the extended address(扩展地址);the street address(街道地址);the locality (e.g., city)(地方（如城市));the region (e.g., state or province)(该地区（如州或省）);the postal code(邮政编码);the country name(国家名称)`
**七个部分组成，如果，其他的一个部分没有，必须用分号分开**

`LABEL`: 是一格式化的文本值，表示一个地址

基本上和`ADR`一致,不同的是： **ADR的值是用分号分开的数据，LABEL就是一个格式化的文本**

`EMAIL`: 电子邮件，`TYPE`参数类型为：

     internet :表示一个internet 类型地址（缺省值）
     x400：表示是一个 X.400 地址
     pref：最喜欢使用的邮件电子

`REV`: 指定当前Vcard的修改信息,比如修改时间

下面的二维码，是上面的 `vCard`内容生成的二维码可以体验一下
![alt](/images/电子名片Vcard标准详解/code.png)

***更多的标识类型，需要查看下面的参考文档***

>参考文档：
>  [百度百科vCard](https://baike.baidu.com/item/vCard)
>  [维基百科vCard](https://zh.wikipedia.org/wiki/VCard) 


