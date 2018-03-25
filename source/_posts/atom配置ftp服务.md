---
title: atom配置ftp服务
tags:
  - atom
date: 2018-03-25 12:02:53
---

----------------------------------------------

通常我们本地写代码是需要上传到测试服务器上进行测试,但是每次都登录服务器,通过`scp`命令进行文件传输,比较麻烦

下面就在开发工具`atom`配置`ftp`服务使`项目文件与服务器文件直接对应上`,达到文件夹共享的功能

----------------------------------------------
<!--more-->

##### 安装ftp功能的插件

在`atom`上安装插件的地方搜索`remote-ftp`,并安装

![alt](/images/atom配置ftp服务/ftp_install.jpg)

##### 配置ftp服务

1. 打开一个需要共享的项目文件夹或者新建一个文件夹
2. 打开`ftp`配置的侧边栏可以通过`Packages -> Remote FTP -> Toggle`或者快捷键打开`control+option+o`
![alt](/images/atom配置ftp服务/ftp_sidebar.jpg)
3. 新建一个`sfpt`配置文件通过`Packages -> Remote FTP -> Create SFTP config file`通过方式创建的配置文件有一些默认的配置想,若直接点击侧边栏的`Edit Configuration`会创建一个空白的配置文件
配置文件的内容为:
```sh
{
    "protocol": "sftp", # 协议名称 有sftp 和  ftp
    "host": "example.com", # 服务器地址,可以是域名或者ip地址
    "port": 22, # 服务端口
    "user": "user", # 服务器用户名
    "pass": "pass", # 服务器密码
    "promptForPass": false, # 是否弹出输入密码提示
    "remote": "/", # 服务器上需要连接的文件夹,绝对地址
    "local": "./", # 本地需要共享的文件夹
    "agent": "",
    "privatekey": "",
    "passphrase": "",
    "hosthash": "",
    "ignorehost": "",
    "connTimeout": 10000, # 多长时间等待连接完成，连接超时
    "keepalive": 10000, # 多长时间发生 dummy 命令区保持连接
    "keyboardInteractive": false, # 是否开启验证码 键盘交互
    "keyboardInteractiveForPass": false,
    "remoteCommand": "",
    "remoteShell": "",
    "watch": [], # 监听哪些文件或者文件夹,当有改动时就会自动上传
    "watchTimeout": 500 # 文件最后一次修改到开始上传之间的延迟
}
```
>这里通过`SFTP`进行讲解,由于`FTP`服务需要在服务端启动`FTP`服务

4. 修改配置完成之后可以在侧边栏进行**连接服务**或者修改配置文件,连接成功之后的显示
![alt](/images/atom配置ftp服务/ftp_success.jpg)

5. 同步本地的项目文件
![alt](/images/atom配置ftp服务/ftp_sync.jpg)

**完成！！！** ***到此为止本地的文件夹就已经和远程的文件夹关联起来了,若监听对应的文件夹,那么修改后就会自动上传到服务器了***

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**