---
title: linux命令之ls
tags: [linux]
---
------------------------------------------------------------

`ls`命令用于显示工作目录下的内容，默认显示文件及子目录；带上参数后，我们可以用`ls`做更多的事

------------------------------------------------------------
<!--more-->

#### 首先看看该命令的帮助

```sh
usage: ls [-ABCFGHLOPRSTUWabcdefghiklmnopqrstuwx1] [file ...]
```

列出指定工作目录的的文件,默认是当前文件夹; 默认按照字母排序

#### 语法

`ls [options] [files]`

#### 选项 -- options

* -a,--all 不忽略以 **.** 开头的文件,即隐藏文件，展示所有的文件
* -A  同 -a ，但不列出 **.** (目前目录) 及 **..** (父目录)
* -l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
* -h,--human-readable 以人更易读的方式列出文件大小(e.g., 1K 234M 2G)
* -r,--reverse 将文件以相反次序显示(默认依英文字母次序)
* -R,--recursive  列出递归的目录即若目录下有文件，则以下之文件亦皆依序列出
* -S 以文件的大小进行排序，从大到小排序
* --block-size=SIZE 改变显示文件的基础单位大小
* -d,--directory  只显示文件及名称，不显示内容
* -t 通过修改时间进行排序，最新修改的在最上面
* -p 在文件夹后面添加 **/** 标记

#### 参数--files

要展示的工作目录(默认是当前文件夹)

#### 实例

1. 不带参数的`ls`命令，只列出文件及目录，没有其他附件信息

![alt](/images/linux命令之ls/ls.jpg)

>有时候你可能发现`ls`命令列出了其他信息,那是因为你的`ls`命令是带了参数`ls`命令的别名

2. 以文件的大小进行排序

`ls -lhS`
![alt](/images/linux命令之ls/ls-S.jpg)

3. 指定显示文件大小的基本单位

单位包括：
```
K = Kilobyte
M = Megabyte
G = Gigabyte
T = Terabyte
P = Petabyte
E = Exabyte
Z = Zettabyte
Y = Yottabyte
```

`ls -l --block-size=M`
![alt](/images/linux命令之ls/ls--blocksize.jpg)

>注意：使用强制基本单位后，不会显示小数，会四舍五入
>如果在 `--block-size` 之前使用`-h`,`--block-size` 起作用；若在之后使用则`-h`起作用。谁在后谁起作用

4. 只显示目录文件

`ls -ld */`
![alt](/images/linux命令之ls/ls-d.jpg)

5. 以时间进行排序，通过修改时间列出，最新修改的在最上面

`ls -lt`
![alt](/images/linux命令之ls/ls-t.jpg)

6.  增加 / (斜线) 标记目录

`ls -lp`
![alt](/images/linux命令之ls/ls-p.jpg)


**详细的命令使用方式,可以使用`man ls` 或`ls --help` 查看**


![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**