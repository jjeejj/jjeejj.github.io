---
title: linux命令之hostname
tags:
  - linux
date: 2017-11-08 22:50:41
---

---------------------------------------------------

`hostname` 用于显示和设置系统的主机名称，环境变量HOSTNAME也保存了当前的主机名在使用hostname命令设置主机名后，系统并不会永久保存新的主机名，重新启动机器之后还是原来的主机名。如果需要永久修改主机名，需要同时修改`/etc/hosts`和**Fedora**`/etc/sysconfig/network`,或者**Ubuntu**`/etc/hostname`的相关内容

--------------------------------------------------
<!--more-->

首先先看看基本的帮助配置项：
```sh
Usage: hostname [-b] {hostname|-F file}         set host name (from file)
       hostname [-a|-A|-d|-f|-i|-I|-s|-y]       display formatted name
       hostname                                 display host name

       {yp,nis,}domainname {nisdomain|-F file}  set NIS domain name (from file)
       {yp,nis,}domainname                      display NIS domain name

       dnsdomainname                            display dns domain name 

       hostname -V|--version|-h|--help          print info and exit 

Program name:
       {yp,nis,}domainname=hostname -y
       dnsdomainname=hostname -d

Program options:
    -a, --alias            alias names
    -A, --all-fqdns        all long host names (FQDNs)
    -b, --boot             set default hostname if none available
    -d, --domain           DNS domain name
    -f, --fqdn, --long     long host name (FQDN)
    -F, --file             read host name or NIS domain name from given file
    -i, --ip-address       addresses for the host name
    -I, --all-ip-addresses all addresses for the host
    -s, --short            short host name
    -y, --yp, --nis        NIS/YP domain name

Description:
   This command can get or set the host name or the NIS domain name. You can
   also get the DNS domain or the FQDN (fully qualified domain name).
   Unless you are using bind or NIS for host lookups you can change the
   FQDN (Fully Qualified Domain Name) and the DNS domain name (which is
   part of the FQDN) in the /etc/hosts file.

```

#### 语法

`hostname (选项) (参数)`

#### 选项
```js
-a: 显示主机别名
-i: 显示主机名称对应的IP地址
-I: 显示主机对应的所有IP地址
-f: 从给定的文件中获取主机名
```

#### 参数

主机名：需要设置的主机名

#### 实例

1. hostname 查看主机名
    `[jwj@iZwz98uofb7lyzkjfkky3oZ ~]$ hostname  //iZwz98uofb7lyzkjfkky3oZ`
2. 临时修改主机名
    `[jwj@iZwz98uofb7lyzkjfkky3oZ ~]$ hostname helloworld`
    `[jwj@helloworld ~]$ hostname //helloworld`
>这种方式只是临时修改主机名，系统重启后就恢复到之前主机名
3. 永久修改主机名
    `对于Ubuntu来说，主机名存放在/etc/hostname`文件中，直接修改重启就生效了


#### 特别说明:
* `hostname`的实际配置文件为: `/etc/hostname` 对于`Ubuntu`
* `hostname`是`linux`系统下的一个内核参数，保存在`/proc/sts/kernel/hostname`下，但是这个值是`linux`启动时从`rc.sysinit`中读取的，`rc.sysinit`依赖`/etc/hostname`文件的内容
* 对于`/etc/hosts`,内容为主机名与IP的对应关系。文件的作用：相当于DNS域名解析，Linux系统在向DNS服务器发出域名解析请求之前会查询`/etc/hosts`文件，如果里面有相应的记录，就会使用hosts里面的记录.
    ```sh
    127.0.0.1 localhost
    ::1     localhost localhost.localdomain localhost6 localhost6.localdomain6
    172.18.187.237 iZwz98uofb7lyzkjfkky3oZ
    ```

    ```sh
    ::1     ip6-localhost ip6-loopback
    fe00::0 ip6-localnet
    ff00::0 ip6-mcastprefix
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters
    10.169.94.190 iZ949r9sg4gZ
    127.0.0.1 localhost
    172.18.177.3 iZwz93nasoxqpro57y8nmtZ
    ```

    对于内容的解释,分为三部分或两部分:
    1. 网络ip地址
    2. 主机名或域名或主机名.域名(该部分可以没有)
    3. 主机名（或别名）