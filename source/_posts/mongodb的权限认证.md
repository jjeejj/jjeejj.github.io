---
title: mongodb的权限认证
tags: [mongodb,数据库]
date: 2016-12-07 23:33:09
---


默认情况下，`mongodb`没有开启权限认证

开启有两种方法

1. 在启动时指定 `auth=true` (该方式比较实用，本文介绍该方法)
2. 使用`keyfile`

<!--more-->

## 使用 auth = true 进行启动

使用该方法启动后会在日志文件中看到：
![authenable](/images/mongodb的权限认证/authenable.png)

此时还可以进行登录，但是无法进行操作：

![alt](/images/mongodb的权限认证/noauth.png)

这是因为还没有创建用户的原因

## 创建用户

此时需要把上一步的认证关闭，在创建完用户后再开启否则无法进行创建

创建语法：
```js
use admin
db.createUser(
  {
    user: "test",
    pwd: "test",
	customData:{}
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```

>user:用户名
>pwd:密码
>roles中是个数组:role：角色类型，db:针对哪个数据库的

角色：

内置的：

```
Built-In Roles（内置角色）：
1. 数据库用户角色：read、readWrite;
2. 数据库管理角色：dbAdmin、dbOwner、userAdmin；
3. 集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
4. 备份恢复角色：backup、restore；
5. 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
6. 超级用户角色：root  
// 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
7. 内部角色：__system
```

```
read：允许用户读取指定数据库
readWrite：允许用户读写指定数据库
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
root：只在admin数据库中可用。超级账号，超级权限
```

>还可以自建角色,自行百度

示例:
```
db.createUser(
   {
     user: "jjeejj",
     pwd: "jjeejj",
     roles: [ {role:"userAdmin", db:"admin"},{role:"readWrite",db:"test"} ]

   }
)
```
执行结果
```
Successfully added user: {
	"user" : "jjeejj",
	"roles" : [
		{
			"role" : "userAdmin",
			"db" : "admin"
		},
		{
			"role" : "readWrite",
			"db" : "test"
		}
	]
}
```

最后进行登录，两种方式：

`mongo  -u "jjeejj" -p "jjeejj" --authenticationDatabase "admin"`
或
```
mongo
use admin
db.auth("jjeejj","jjeejj")
```





