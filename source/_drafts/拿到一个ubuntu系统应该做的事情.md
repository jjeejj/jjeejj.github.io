---
title: 拿到一个ubuntu系统应该做的事情
tags: [linux]
---
---------------------------------------------

通常拿个一个新的`ubuntu`系统，需要查看一些东西，配置一下自己喜好

--------------------------------------------
<!--more-->

登录系统之后，先查看一下系统的版本：
`lsb_release -a`
![alt](/images/拿到一个ubuntu系统应该做的事情/lsb_release.jpg)

这时有可能会看到，　`release` 为 `12.04`版本比较低，需要更新。则执行以下命令:

先把软件包，更新到最新： `sudo apt-get update`,

在更新`release`: `sudo do-release-upgrade`

然后一路 输入`y`或者`enter`既可以，默认的选项，等待更新成功即可.

这样更新完后的`release`为 `14.04`，还需要在执行一边上面的命令，更新到　`16.04`

接下来需要安装一些常用的软件：

* curl : `sudo apt-get install curl`
* git : `sudo apt-get install git`

* [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

    安装这个软件之前需要安装 [zsh](https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH)

* [nvm](https://github.com/creationix/nvm)

    `nvm`这个命令可能写在在`.bashrc`，有可能需要添加到`.zshrc`中，  `nvm`才能起作用的

* 利用 **nvm** 安装不同版本的`node`: `nvm install [node version]`

* docker : [docker ubuntu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#prerequisites)


