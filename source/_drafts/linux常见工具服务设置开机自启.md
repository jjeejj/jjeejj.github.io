---
title: linux常见工具服务设置开机自启
tags: [linux]
---

我们使用`Linux`系统通常是部署服务,而且有些基础服务是必须启动,需要开始的就要自动启动,比如`Nginx`,`Redis`服务等等...

<!--more-->

对于`Linux`系统常用的有`Centos`,`Ubuntu`,下面就分别介绍各个服务在这两个系统下怎么配置开机自启动的

* 对于`Centos 7` 以上的系统是用`Systemd`进行初始化控制的,`Systemd`的服务文件以`.service`结尾
* 

### Nginx服务

#### Centos

* 对于`yum`安装的,`yum`会自动创建`nginx.service`文件,所以可以直接使用`systemctl enable nginx.service`设置
* 对于手动安装源码安装的`nginx`,是没有`nginx.service`文件的,需要手动创建`nginx.service`文件
   1. 在系统服务目录创建`nginx.service`文件 `vim /lib/systemd/system/nginx.service`
   2. 文件内容为：
    ```cnf
    [Unit]
    Description=nginx
    After=network.target

    [Service]
    Type=forking
    ExecStart=/usr/local/nginx/sbin/nginx
    ExecReload=/usr/local/nginx/sbin/nginx -s reload
    ExecStop=usr/local/nginx/sbin/nginx -s quit

    [install]
    WantedBy=multi-user.target
    ```
        每一项的解释:
        [Unit] : 服务说明
        Description： 描述
        After：服务类别
        [Service]： 服务运行时参数设置
        Type：后台运行形式
        ExecStart：服务的具体运行命令
        ExecReload：重启命令
        ExecStop：停止命令

    > [Service]的启动、重启、停止命令全部要求使用绝对路径

    [Install]: 运行级别下服务安装的相关设置，可设置为多用户

    3. 设置开机启动 `systemctl enable nginx.service`
    4. 若执行命令失败请查看`nginx.service`文件权限并修改为`chmod 755 nginx.service`

### Redis服务

#### Centos

```cnf
[Unit]
Description=Redis persistent key-value database
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/bin/redis-server /etc/redis.conf --supervised systemd
ExecStop=/usr/libexec/redis-shutdown
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
```