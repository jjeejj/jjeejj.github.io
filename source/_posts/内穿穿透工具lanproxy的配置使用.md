---
title: 内穿穿透工具lanproxy的配置使用
tags:
  - tools
date: 2018-01-21 12:56:19
---

--------------------------------------------

做微信开发或者把内网服务穿透出去,都会需要一个公网的地址。

经常使用的内网穿透工具有:**花生壳**,**ngrok**,**魔法隧道**等,但是这些域名都是第三方随机的，自定义域名的都是收费。

本来在**window**是使用的**n2n**内网穿透服务的,但是换了`Mac`之后没有找到`Mac`端的客户端。

所以本文讲解一下怎么用**lanproxy**搭建一个内网穿透的服务

--------------------------------------------

<!--more-->

#### 搭建环境

* 一台公网的服务器(本文使用的是`Ubuntu 16.04.3 LTS`),运行**lanproxy**服务端
* 服务器`JDK`的环境(本文使用的`openjdk 1.8.0_151`)
* 客户端机器,运行**lanproxy**客户端,也需要`java`的环境需要安装`jdk`和`maven`

#### 搭建过程

##### 获取 lanproxy 的源码 并编译

1. 去[`lanproxy`](https://github.com/ffay/lanproxy)的`github`地址去获取源码到本地
    `git clone git@github.com:ffay/lanproxy.git`
2. 进入项目目录下 执行`mvn package`进行大包编译，打包编译的文件在`distribution`目录下,包括`client`和`server`
![](/images/内穿穿透工具lanproxy的配置使用/mvn_package.jpg)

##### 配置并启动

###### server端

在`proxy-server-0.1`文件夹小的`conf`是`server`端的配置文件
```
server.bind=0.0.0.0 # 服务地址 
server.port=4900 # 服务端口

# ssl 配置可以默认
server.ssl.enable=true
server.ssl.bind=0.0.0.0
server.ssl.port=4993
server.ssl.jksPath=test.jks
server.ssl.keyStorePassword=123456
server.ssl.keyManagerPassword=123456
server.ssl.needsClientAuth=false

config.server.bind=0.0.0.0 # 服务页面管理访问地址
config.server.port=8090 # 服务页面管理访问端口
config.admin.username=admin # 服务页面管理访问用户名
config.admin.password=admin # 服务页面管理访问用密码
```

配置完成之后可以上该文件加到传服务器上,执行下面的命令
`scp -r proxy-server-0.1 user@premote_ip:remote_dolder`

在服务端执行`proxy-server-0.1/bin`文件夹下的`startuo.sh`，服务端启动

在浏览器上访问上面的你的 服务器`ip + prot`就可以看到管理页面了
![](/images/内穿穿透工具lanproxy的配置使用/server_admin_login.jpg)

输入上面你配置的用户名密码登录进去，进去配置
![](/images/内穿穿透工具lanproxy的配置使用/server_admin_login_success.jpg)

首先添加一个客户端:
![](/images/内穿穿透工具lanproxy的配置使用/server_admin_add.jpg)

添加成功后在客户端管理那可以看到刚刚添加的客户端:
![](/images/内穿穿透工具lanproxy的配置使用/server_admin_add_show.jpg)

然后在对刚刚添加成功的客户端进行配置:
![](/images/内穿穿透工具lanproxy的配置使用/server_admin_add_config.jpg)

代理名称随便输入,一般都用本地代理服务的名称方便产看

>一个客户端代理可以配置多个本地服务端口

###### client端

在`proxy-client-0.1`文件夹小的`conf`是`clent`端的配置文件
```
client.key=caaab8dc002c4e0f8b31ecb683d8900f # 这个key就是服务端中客户端管理的客户端秘钥
ssl.enable=false
ssl.jksPath=test.jks
ssl.keyStorePassword=123456

server.host= # 这个添加服务端地址,可以是配置的域名也可以是公网Ip

#default ssl port is 4993
server.port=4900 # 服务端端口
```
这个客户端 是`java`版本的所以需要`jdk`环境，配置完成后在`window`执行`bin目录下的 startup.bat`,在`linux（mac`）环境中运行bin目录下的 `startup.sh`

#### nginx 的配置

如果你微信开发只能`80`端口，但是你只有一个`80`端口，而且有很多其他的服务,这个就需要配置`nginx`进行端口转发

首先你要有一个域名，配置一条`A`记录指向的服务比如这个代理服务你可以配置:
`*.proxy.yourdomain` ---- > `你的公网ip地址`

在`nginx`上进行配置，主要是根据 `server_name`进行转发

假如你本地的微信服务是`8080`端口，上面客户端配置添加的公网端口是`5000`那你可以这样配置，把你本地微信的服务穿透出去为：
`wechat.proxy.yourdomain`(上面添加的域名`A`记录和这个有关系的，不然解析不到对应的服务器地址)那么`nginx`的配置为

```
{
    server {
        listen 80;
        server_name wechat.proxy.yourdomain;
        location / {
                proxy_pass  http://127.0.0.1:5000/;
        }
    }
}
```

配置完成后重启 `nginx -s reload` ,你就可以可以使用你配置**代理域名服务**访问你本地的服务了

**以后如果再有本地的服务需要穿透出去的,可以按照相同的方法进行配置**
如果在同一个机器上，就可以直接在服务配置页面下选择对应的客户端添加一个端口转发的配置就可以了
若不再一个机器上，就新建一个客户端在进行配置

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**
