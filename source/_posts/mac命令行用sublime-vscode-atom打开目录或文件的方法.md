---
title: mac命令行用sublime,vscode,atom打开目录或文件的方法
tags:
  - other
date: 2017-11-18 14:26:22
---

---------------------------------------------------

`MAC`  在命令行直接利用 **sublime**,**vscode**,**atom**，直接打开文件或者目录，能很高的提高开发效率

----------------------------------------------------
<!--more-->

总体的思路就是配置`alias`别名，但是`atom`有些意外的情况发生

1. 第一步打开终端，进到对应的用户目录下，找到你的`shell`配置文件

    ![shell_show](/images/mac命令行打开目录或文件的方法/shell_show.png)

2. 修改对应的`shell`配置文件，若你安装过`oh_my_zsh`,打开`.zshrc`文件，若你是默认没有修改过就开`.bash_profile`文件

    ```
    alias chrome="/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome"
    #alias atom="/Applications/Atom.app/Contents/MacOS/Atom"
    alias vscode="/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code"
    ```
    ![shell_add](/images/mac命令行打开目录或文件的方法/shell_add.png)

    添加图中对应的`alias`,图中应用程序路径是我自己的，你可以查看自己应用程序路径添加上

3. 执行以下命令让你修改的配置文件生效
    `source ./.zshrc` 或者 `source ./.bash_profile`

在上面可以看出有关`atom`的那个别名被注释掉了，这个因为用`atom`别名打开一个文件或者目录的时候，后台应用程序被挂起，命令行不退出输入模式。如果退出输入模式那么`atom`应用程序也会被关闭。所以用别名指向应用程序的路径对`atom`是不可以行的。

那么对于`atom`怎么处理呢？
答案就是：在`atom`应用程序中，有个**Install Shell Commonds**功能选项
![install_shell_commonds](/images/mac命令行打开目录或文件的方法/install_shell_commonds.png)
![install_shell_commonds_success](/images/mac命令行打开目录或文件的方法/install_shell_commonds_success.png)

点击之后在命令行就可以直接使用`atom`打开目录或者文件


![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**



