---
title: linux命令之htpasswd
tags:
  - linux
date: 2018-03-22 21:46:56
---

--------------------------------------------------

`htpasswd`是`Apache`的`Web`服务器内置的工具,用于创建和更新储存用户名和用户基本认证的密码文件

-------------------------------------------------
<!--more-->

#### 安装

1. 由于该工具是`Apache`的`Web`服务器内置的工具,所以直接安装`Apache`,在对应的`bin`目录下可以看到该命令的
2. 还可以直接安装`httpd-tools`这个工具包,比如：`yum install httpd-tools`,安装成功后就可以直接使用该命令

#### 语法

`htpasswd (选项) (参数)`

#### 选项

* `-c`: 创建一个新的密码文件
* `-b`: 在命令行中一并输入用户名和密码而不是根据提示输入密码
* `-D`: 删除指定的用户
* `-n`: 不更新密码文件,只将加密后的用户名密码输出到屏幕上
* `-p`: 不对密码进行加密,采用明文的方式
* `-m`: 采用MD5算法对密码进行加密(**默认的加密方式**)
* `-d`: 采用CRYPT算法对密码进行加密
* `-s`: 采用SHA算法对密码进行加密
* `-B`: 采用bcrypt算法对密码进行加密(**非常安全**)

#### 参数

* 用户名: 要创建或者更新的用户名
* 密码: 用户的新密码

#### 实例

1. 新建一个密码文件`.passwd`并添加一个用户,不提示直接输入用户名密码
    `htpasswd -bc .passwd jiang wen`

2. 在原有的密码文件`.passwd`下在添加一个用户
    `htpasswd -b .passwd wen jiang`

3. 更新用户的密码:有两种方式
 第一种,直接添加相同的用户名，就会自动区更新密码：`htpasswd -b .passwd wen wen`
 第二种,先删除需要更新密码的用户名,在添加用户：`htpassdw -D .passwd wen & htpasswd -b .passwd wen wen`

4. 不更新密码文件，只显示加密后的用户名和密码 
    `htpasswd -bn jiang jiang`

5. **nginx**模块 [http_auth_basic_module](http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html)中的使用,用于生成用户密码文件进行认证

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**