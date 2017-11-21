---
title: linux命令之at
tags: [linux]
---
----------------------------------------------------

`linux`命令之 **at**

**at** 在指定的时间执行一次特定的任务，即：一次性定时任务计划执行

--------------------------------------------------
<!--more-->


该命令是`atd`进程控制，首先查看一下该进程是否启动，一般是随着系统自动启动的
`ps aux | grep atd`

若没有启动，则需要手动进行启动
```
/etc/init.d/atd start

支持的语法：Usage: /etc/init.d/atd {start|stop|restart|force-reload|status}
```

若没有安装，则需要安装：
```
apt-get install at
or
yum install -y at
```

#### 首先看看该命令的帮助

```sh
Usage: at [-V] [-q x] [-f file] [-mMlbv] timespec ...
       at [-V] [-q x] [-f file] [-mMlbv] -t time
       at -c job ...
       atq [-V] [-q x]
       at [ -rd ] job ...
       atrm [-V] job ...
       batch
```

#### 语法

`at (选项) (参数)`

#### 选项

* -f 指定包含具体指令的任务文件
* -q 指定新任务的队列名称
* -m 当指定的任务被完成之后，将给用户发送邮件，即使没有标准输出
* -c 打印任务的内容到标准输出
* -V 显示版本信息
* -d 删除指定的待执行任务，还可以使用**atrm**删除
* -l 显示待执行任务的列表，还可以使用**atq**显示


#### 参数

日期时间：指定任务执行的日期时间

#### 时间格式

* 当天的时间：hh:mm,加入时间已经过去，就放到明天执行
* 模糊的指定时间：**midnight**，**noon**,**teatime**
* 采用十二进制的时间：在时间的后面**am**或者**pm**
* 也可以指定执行命令的具体时间：**month day**(月 日),**mm/dd/yy**(月/日/年),**dd.mm.tt**(日/月/年),指定的日期必须跟在指定的时间后面
* 相对记时法：安排不就要执行的命令：**time+count time-units**，**time**就是指定的时间，**time-units**是单位有**minutes**,**hours**,**days**,**weeks**,**count**是时间数量：比如：**at 5pm+3 days**
* 还有一种直接用**today**(今天),**tomorrow**(明天)来指定时间的，比如：**at 5pm tomorrow**


#### 实例

1. 在当天17:35 输出时间到一个文件
    ```sh
    ➜  ~ at 17:36
    warning: commands will be executed using /bin/sh
    at> date > ./log.log
    at> <EOT>
    job 7 at Tue Nov 21 17:36:00 2017
    ```

2. 删除特定的任务
    ```sh
    [root@localhost ~]# atq 
    8 2013-01-06 17:20 a root 
    7 2013-01-08 17:00 a root 
    [root@localhost ~]# atrm 7 
    [root@localhost ~]# atq 
    8 2013-01-06 17:20 a root
    ```

>输入完成之后按 **ctrl+d**保存退出

#### 安全的问题

不是所有用户都可以运行`at`任务的。因为系统安全的原因。很多主机被攻击破解后，非常有可能运用一些计划任务来运行或搜集你的系统运行信息,并定时的发送给黑客。 所以，除非是你认可的帐号，否则先不要让他们使用 at 命令。

那怎么控制用户使用 at 命令的权限呢?

在这里面有两个文件进行控制的：

* `/etc/at.allow`: 这个文件优先被寻找，若有这个文件，则写在这个文件的使用者才能使用，没有在这个文件用户不能使用的
* `/etc/at.deny`: 若没有`/etc/at.allow`这个文件，就会寻找`/etc/at.deny`这个文件，写在`at.deny`的用户是不可以使用的，没有在的就可以使用
* 若两个文件都没有，那么就只有**root**用户你可以使用这个命令

>对于这个两个文件的书写时：**一个帐号写一行**

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**
