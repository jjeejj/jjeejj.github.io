---
title: linux系统配置免密码登录
tags:
  - linux
date: 2017-12-17 16:52:43
---

--------------------------------------------------------------

进行`linux`服务器管理的时候，经常需要登录进去，每个服务器都需要记住和输入对应的用户名和密码，是非常麻烦的。

所以这里配置一下服务器免密码进行登录，方便我们进行管理

--------------------------------------------------------------
<!--more-->

演示配置环境：客户端 `Mac`。服务器: `Ubuntu 16.04`

首先放一张图,进行介绍密匙免密登录的原理

![alt](/images/linux系统配置免密码登录/ssh-principle.jpg)

转化为图为：

![alt](/images/linux系统配置免密码登录/ssh-principle-image.png)

#### 本地配置公钥/私钥

生产配置之前先检查自己之前是否配置过，如果之前配置过再重新生成，就会覆盖之前的记录，导致你之前配置的记录失效

**执行命令生成**
`ssh-keygen -r rsa -C test@qq.com`

执行该命令后会在用户根目录生产一个`.ssh`的文件夹
![alt](/images/linux系统配置免密码登录/ssh-keygen.jpg)

创建的过程中会让你输入密码,可以不设置密码，一直回车就好

这里面`id_rsa`是私钥,`id_rsa.pub`是公钥，至于`konwn_hosts`文件是登录过服务器后自动生成的文件，用户保存远程服务器的指纹信息

>上面生产秘钥的命令中的 -r 参数是制定加密算法,-C 参数是制定 邮箱
>对于window系统就需要安装`git`客户端 [`msysgit`](http://gitforwindows.org/)


#### 配置服务器

需要把客户端生产的公钥`id_rsa.pub` 里面的内容拷贝到服务器上对应用户根目录下的`.ssh/authorized_keys`文件中。

**若没有该文件可以手动创建该文件**。也可以在服务上进行生成公钥/私钥,执行的命令和上面第一步一样的


也可以直接使用`ssh-copy-id user@host`命令直接拷贝到远程服务器上,为了防止出错，建议使用该命令进行复制客户端的公钥

有时候可能会修改然`authorized_keys`文件的权限
`chmod 600 authorized_keys`

接下来看一下`ssh`的配置文件`/etc/ssh/sshd_config`中的配置项是否打开

```sh
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile	%h/.ssh/authorized_keys
```
确保上面这几项是打开的，非注释掉的

最后重启`ssh`
`service sshd restart`

接下来就可以免密码登录的了,但是每次还是需要输入用户名和主机地址的，这可以在本地配置`alias`别名

如果你安装了`zsh`就在`.zshrc`文件中添加，如果没有安装就是默认的在`.bashrc` 文件中添加
`alias ssh_name user@host`

执行 `source .zshrc`  或 `source .bashrc` 生效配置文件


![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**



