---
title: mongodb服务msi的配置使用
tags:
  - mongodb
date: 2016-12-06 08:03:56
---


window 上 **mongodb**安装的时候有**msi**安装和**编译zip源码**解压安装。**msi**安装后需要配置下才能使用

<!--more-->

## 下载mongodb.msi安装包

[下载地址](https://www.mongodb.com/download-center?jmp=nav#community)

这里只需要一直点击下一步就可以，还可以自定义安装目录

## 配置服务

1. 配置环境变量

右键计算机->属性->高级系统设置->环境变量；在系统变量里添加MD_HOHE环境变量，变量值为MongoDB的根目录

`MD_HOHE= D:\MongoDB Server3.4`

然后在path中配置

在后面添加：`;%MD_HOHE%\bin`

2. 创建需要的文件和配置信息

进入到安装目录中:

首先：

创建文件夹 `data`用来存储数据文件
创建文件夹`log`用来存储日志文件
创建文件夹`conf`用来存储启动配置文件

然后启动服务：

* 可以直接简单启动：

`mongod --dbpath "D:\MongoDB Server3.4\data" --logpath "D:\MongoDB Server3.4\log\mongod.log"`

起来默认的端口:27107

* 还可以书写配置文件启动

进入`conf`文件夹中，创建`mongod.conf`文件，写入配置参数
```
dbpath = D:\MongoDB Server3.4\data
logpath = D:\MongoDB Server3.4\log\mongod.log
logappend = true
port = 27017
fork = true
```

>dbpath:可以使用相对路径或绝对路径
>logpath:需要制定一个日志文件
>fork:代表启动一个后台进程，window下无效
>logappend：代表，日志append

最后启动

`mongod --rest -f "D:\MongoDB Server3.4\conf\mongod.conf" `

>rest:参数可以打开 28107监控页面 http://127.0.0.1:28017

为了方便起见可以注册为服务:

以**管理员权限**执行以下命令:

`mongod --install --dbpath "D:\MongoDB Server3.4\data" --logpath "D:\MongoDB Server3.4\log\mongod.log"`
或
`mongod --install  -f "D:\MongoDB Server3.4\conf\mongod.conf"`

## 连接客户端

* 以mongo客户端进行连接

连接说明:
`mongo host:port/collection`

举例:
`mongo 127.0.0.1:27017/test`

>可以利用 mongo --help 查看帮助（各种配置项）

* 利用mongo可视化工具进行连接
* 利用mongo各种驱动进行连接

 **最后大功告成** :ok_hand:
