---
title: linux命令之scp
date: 2017-12-02 14:31:31
tags: [linux]
---
-------------------------------------------------

由于经常在`linux`服务器和本地进行文件传输,这样就需要了解文件传输的命令

-------------------------------------------------
<!--more-->

在没有文件传输工具的情况下,就必须了解有关的文件传输命令，下面就来说说这个命令`scp(secure copy)`,是`linux`系统下基于`ssh`登陆进行安全的远程文件拷贝命令

#### 首先看看该命令的帮助

`scp`
```sh
usage: scp [-12346BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file]
           [-l limit] [-o ssh_option] [-P port] [-S program]
           [[user@]host1:]file1 ... [[user@]host2:]file2
```

#### 语法

`scp [可选参数] file_source file_target`

#### 参数
* -1: 强制scp命令使用ssh1协议
* -2: 强制scp命令使用ssh12协议
* -4: 强制scp命令只使用IPv4寻址
* -6: 强制scp命令只使用IPv6寻址
* -P: 指定数据传输用到的端口号
* -v: 详细方式显示输出
* -c cipher: 以cipher方式将数据进行传输加密,这个选项会直接传给ssh
* -l: 限定用户所能使用的带宽，以Kbit/s为单位
* -F: 指定一个替代的ssh配置文件，此参数直接传递给ssh
* -p: 保留原文件的修改时间，访问时间和访问权限
* -q: 不显示传输进度条
* -r: 递归复制整个目录
* -C: 允许压缩

#### 实例

##### 从本地复制到远程

**复制文件**
```sh
-------------------------------------------------------------
一: 
scp loacl_file remote_username@remote_ip:remote_folder
scp /home/space/music/1.mp4 root@www.test/com:/home/root/other/music
---------------------------------------------------------------------
二:
scp loacl_file remote_username@remote_ip:remote_file
scp /home/space/music/1.mp4 root@www.test/com:/home/root/other/music/1.mp3
------------------------------------------------------------------------
三:
scp loacl_file remote_ip:remote_folder
scp /home/space/music/1.mp4 www.test/com:/home/root/other/music
--------------------------------------------------------------
四:
scp loacl_file remote_ip:remote_file
scp /home/space/music/1.mp4 www.test/com:/home/root/other/music/1.mp3
```

上面的例子中:
* 前两个指定了用户名,执行命令后需要输入密码
* 后两个没有指定用户名,执行命令后需要输入用户名和密码
* 第一个和第三个只指定了目录，文件名称不变，对应本地的文件名称
* 第二个和第四个指定的文件名称

>注意:复制文件时该命令不会创建文件夹，若服务器没有对应的文件夹，则会把文件夹名称当做文件的名称。对于上面第一个例子，若服务器上没有`music`文件夹，则会`music`当做文件名去对应`1.mp4`
>对于表达式中的`remote_ip`可以是**ip**地址或者对应映射的**域名**

**复制目录**

```sh
scp -r /home/space/music/ www.test.com:/home/root/others/  
```

上面命令将本地 music 目录复制到远程 others 目录下

>复制目录是记得要加上`-r`参数.若服务器上没有指定的目录则会在服务器上自动创建一个目录

##### 从远程复制到本地

从远程复制到本地，只要将从本地复制到远程的命令的后2个参数调换顺序即可
```sh
scp -r root@www.test.com:/home/root/others/ /home/space/music/ 
scp root@www.test/com:/home/root/other/music/1.mp3 /home/space/music/1.mp4 
```

##### 特别说明

1. 使用scp命令要确保使用的用户具有可读取远程服务器相应文件的权限，否则scp命令是无法起作用的
2. 如果远程服务器防火墙有为scp命令设置了指定的端口，我们需要使用 -P 参数来设置命令的端口号
`scp -P 4500 /home/space/music/1.mp4 root@www.test/com:/home/root/other/music/1.mp3`

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**