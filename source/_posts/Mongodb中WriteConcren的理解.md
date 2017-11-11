---
title: Mongodb中Write Concren的理解
tags:
  - mongo
date: 2017-11-11 14:36:50
---

----------------------------------------------------------------
Write Concren 描述的是向一个独立的**mongod**服务或者**复制集**后者**分片集群**
请求一个写操作后的验证结果的级别。**可以理解为服务什么条件下，进行写操作的响应**

----------------------------------------------------------------
<!--more-->

首先说明：
在版本**v2.6**之前，在指定一个`write concern`的写操作命令之后需要请求`getLastError`命令根据其返回值验证写操作是否成功了。在**2.6**版本之后：把`write concern`集成到写操作中了，取消调用`getLastError`命令了。

#### Write Concern 规范


Write concren 可以包括下面的字段
```js
{w: <value>,j: <booblean>,wtimeout: <number>}
```
* **w** 配置项：代表向**mongo**实例或者向带有指定标签的**mongo**实例或者**复制集**请求一个带有指定**w**值的的**写操作传播范围**的请求确定
* **j** 配置项 ：代表写操作是否要写入`journal`的请求确定
* **wtimeout**配置项：代表指定一个限制的时间防止写操作长时间阻塞 

##### w OPtinon

|value|description|
|:----:|:-----------:|
| <`number`> |w:1,独立的mongod服务或者复制集的primary节点，这是默认值;<br> w:0,不去确定一个写操作请求是否写入，但是可能返回`socket`和网络错误信息<br>如果指定了：w:0 ,但是又指定了j:true,那么j:true优先，会确定一个写操作写入<br>对于大于1的数字，仅仅使用在复制集中|
|"majority"|代表多数投票节点包括主节点|
|<`tag set`>|代表在复制集中带有指定标签的成员|

##### j Option

写操作是否要写入[`journal`](https://docs.mongodb.com/manual/core/journaling/)的请求确定
|说明对象|说明|
|:--------:|:-------:|
|j|如果j:true,代表`mongod`实例，结合指定的`w:<value>` 在硬盘上都要写入了journal<br>j:true不保证由于复制集中主节点故障，写操作不会回滚<br>在`3.2`版本中改变，当j:true时，仅仅在请求成员数量包括主节点都已经写入journal才返回结果；在之前的版本仅仅要求主节点写入journal，不管w:<`value`>指定的成员数量是否写入|

##### wtimeout

这个配置项指定一个时间限制(毫秒)为`write concern`,仅仅应用在**w**大于1的情况下

 **wtimeout**让写操作返回一个错误在指定的限制时间之后，即使指定的写操作最后将成功。当这些写操作返回时，MongoDn不撤销成功执行修改的数据在`write concern`超过`wtimeout`限制的时间之前

 如果你没有指定一个**wtimeout**配置项,那么`write concern`的级别是无法实现的，写操作将可能长久阻塞。指定**wtimeout**的值为0相当于没有**wtimeout**配置项

#### Acknowledgement Behavior

**w**和**j**配置决定**mongod**实例写操作请求返回的时间点

##### Standalone

一个独立的**mongod**确定一个写操作是在应用写到内存或者写到硬盘`journal`中之后。
下面展示了确定行为和`write concern`的关系

||**j is unspecified**|**j:true**|**j:false**|
|:------:|:---------:|:------------:|:-----------:|
|w:1|in memory|On-disk journa |In memory|
|w:"majority"|On-disk journal if running with journaling|On-disk journa |In memory|

##### Replica Sets 复制集

一个复制集确定一个写操作在指定成员数量应用写到内存或者写到硬盘`journal`中之后。
成员数量通过`w:<value>`设定。下面列出了确定行为：成员数量和`write concern`之间的关系
||**j is unspecified**|**j:true**|**j:false**|
|:------:|:---------:|:------------:|:-----------:|
|w:"majority"|Depends on the value of <br>**writeConcern-<br>-MajorityJournalDefault** <br>If true, On-disk journal.<br>If false, In memory.|On-disk journa |In memory|
|w:<`number`>|in memory|On-disk journa |In memory|

>[writeConcernMajorityJournalDefault](https://docs.mongodb.com/manual/reference/replica-configuration/#rsconf.writeConcernMajorityJournalDefault)这个值影响`{w:"majority"}`的表现


#### 特别说明

1. 上面的`Write Concren`文档行为表现是在版本`3.2`,引用[3.2 manual](https://docs.mongodb.com/v3.2/reference/write-concern/)
2. 在`3.0`版本改变的，在`3.0`之前`w:"majority"`代表着复制集中大多数节点成员而不是复制集中多数投票节点的成员
3. 在`2.6`版本改变的，在`Master/Slave`部署中，`Mongodb`对待`w:"majority"` 等于`w:1`,在之前的版本中`w: "majority"`在` master/slave`部署中会报错的