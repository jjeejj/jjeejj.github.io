---
title: 'linux命令:创建用户useradd和adduser'
tags:
  - linux
date: 2017-10-30 23:02:44
---

---------------------------------------------
`linux` 命令： 创建用户 `useradd`和`adduser`

---------------------------------------
<!--more-->

### useradd

#### 格式

`useradd [options] LOGIN`

#### 配置项--options

* `-b, --base-dir BASE_DIR       base directory for the home directory of thenew account--- 设置基本路径作为用户的登录目录`
* `-c, --comment COMMENT         GECOS field of the new account---加上备注文字，备注文字保存在passwd的备注栏中`
* `-d, --home-dir HOME_DIR       home directory of the new account---指定这个新建用户的家目录`
* `-D, --defaults                print or change default useradd configuration`
* `-e, --expiredate EXPIRE_DATE  expiration date of the new account---指定账号的失效日期，日期格式为MM/DD/YY，例如06/30/12。缺省表示永久有效`
* `-f, --inactive INACTIVE       password inactivity period of the new account---指定在密码过期后多少天即关闭该帐号`
* `-g, --gid GROUP               name or ID of the primary group of the new account---指定这个新建用户的用户组，可以是名称或是id`
* `-G, --groups GROUPS           list of supplementary groups of the new account---新建帐户补充用户组名单，company,employees`
* `-h, --help                    display this help message and exit---展示该命令的帮助`
* `-k, --skel SKEL_DIR           use this alternative skeleton directory`
* `-K, --key KEY=VALUE           override /etc/login.defs defaults`
* `-l, --no-log-init             do not add the user to the lastlog and faillog databases`
* `-m, --create-home             create the user's home directory---创建这个新建用户的家目录`
* `-M, --no-create-home          do not create the user's home directory---不创建这个新建用户的家目录`
* `-N, --no-user-group           do not create a group with the same name as the user---不创建一个和用户名一样的分组`
* `-o, --non-unique              allow to create users with duplicate (non-unique) UID---允许创建用户重复（非唯一）的UID`
* `-p, --password PASSWORD       encrypted password of the new account--为新账户加密密码`
* `-r, --system                  create a system account---建立一个系统账号`
* `-R, --root CHROOT_DIR         directory to chroot into`
* `-s, --shell SHELL             login shell of the new account---新建用户使用的shell`
* `-u, --uid UID                 user ID of the new account---指定这个新建账户的 UID`
* `-U, --user-group              create a group with the same name as the user---创建一个和用户名一样的分组`
* `-Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping`
* `--extrausers              Use the extra users database`

#### 应用实例

1. 建立一个新用户账户`testuser1`,并设置`UID`为544，主目录为`/usr/testuser1`,属于`users`组

    `useradd -u 544 -d /usr/testuser1 -g users -m testuser1`

2. 新创建一个`testuser2`用户，这初始属于`abc`组，且同时让他也属于`dba`组

    `useradd testuser2 -g abc -G dba`

3. 新建用户`testuser3`无法使用 `shell`

    `useradd testuser3 -s /usr/sbin/nologin -m`


>指定密码直接：`passwd 用户名`

>`-u`设定ID值时尽量要大于500，以免冲突。因为Linux安装后会建立一些特殊用户，一般0到499之间的值留给bin

>直接使用`useradd test` 创建出来的用户，是三无用户：**无 home directory,无密码，无系统shell**

>使用`useradd`创建的账号，保存在`/etc/passwd`文本文件中

### adduser

使用`adduser`时，创建用户的过程更像是一种人机对话，系统会提示你输入各种信息，然后会根据这些信息帮你创建新用户。

`adduser`指令是个`script`程序，利用交谈的方式取得输入的用户帐号资料，然后再交由真正建立帐号的`useradd`命令建立新用户，如此可方便管理员建立用户帐号