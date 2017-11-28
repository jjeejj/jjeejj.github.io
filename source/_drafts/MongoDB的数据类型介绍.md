---
title: MongoDB的数据类型介绍
tags: [mongodb]
---
--------------------------------------------

`MongoDB`以`BSON`一种**序列化**的二进制格式存储数据。在每个序列化之前的文档又支持以下列表中的数据类型,每种数据类型都有一个对应的**数字**和**字符串**别名。可以用在[**$type**](http://www.mongoing.com/docs/reference/operator/query/type.html#op._S_type)操作符中用于查询文档

-------------------------------------------

<!--more-->

**数据类型**：

|**Type**|**Number**|**String**|**Notes**|
|:-----:|:------:|:-------:|:-----------:|
|Double|1|"double"||
|字符串|2|"string"||
|对象|3|"object"||
|数组|4|"array"||
|二进制数据|5|"binData"||
|未定义|6|"undefined"|已过期|
|ObjectId|7|"objectId"||
|Boolean|8|"bool"||
|日期|9|"date"||
|空|10|"null"||
|正则表达式|11|"regex"||
|DBPointer|12|"dbPointer"|已过期|
|JavaScript(代码)|13|"javascript"|此数据类型用于将JavaScript代码存储到文档中|
|符号|14|"symbol"|已过期|
|JavaScript(带范围)|15|"javascriptWithScope"||
|32位整数|16|"int"||
|时间戳|17|"timestamp"||
|64位整数|18|"long"||
|Decimal128|19|"decimal"|New in version 3.4|
|Min key|-1|"minKey"||
|Max key|127|||

你可以使用`typeof`或者`instanceof`[instanceof/typeof](http://www.mongoing.com/docs/core/shell-types.html#check-types-in-shell) 判断一个字段值的类型

以下的代码示例，都是在`Mongo shell`中实现的

下面介绍一下几种重要的数据类型

#### ObjectId

`ObjectId` 类似唯一主键，可以很快的去生存和排序，包含 **12 bytes**,含义是：

* 前4个字节表示创建**unix**时间戳,格林尼治时间 **UTC**时间，比北京时间晚了8个小时
* 接下来的3个字节是机器标识码
* 紧接的两个字节由进程id组成（PID
* 最后三个字节是随机数

`MongoDB`中存储的文档必须有一个`_id`键。这个键的值可以是任何类型的，默认是个`ObjectId`对象

由于`ObjectId`中保存了创建的时间戳，所以你不需要为你的文档保存时间戳字段，你可以通过 `getTimestamp` 函数来获取文档的创建时间:

```js
> var newObject = ObjectId()
> newObject.getTimestamp()
ISODate("2017-11-25T07:21:10Z")
```

`ObjectId` 转为字符串
```js
> newObject.str
5a1919e63df83ce79df8b38f
``` 

#### 字符串

**BSON字符串都是UTF-8编码**

#### 时间戳

`BSON`有一个特殊的时间戳类型用于 `MongoDB` 内部使用，与普通的 **日期** 类型不相关。 时间戳值是一个64位的值。其中：
* 前32位是一个 time_t 值（与Unix新纪元相差的秒数）
* 后32位是在某秒中操作的一个递增的``序数``

在**单个 mongod 实例**中，时间戳值通常是唯一的

在复制集中， **oplog** 有一个 **ts** 字段。这个字段中的值使用BSON时间戳表示了操作时间

>BSON时间戳类型主要用于 MongoDB**内部** 使用 。在大多数情况下的应用开发中，你可以使用BSON日期类型

#### 日期

表示当前距离`Unix`新纪元（1970年1月1日）的毫秒数.日期类型是有符号的,负数表示1970年之前的日期
```js
> var mydate1 = new Date() //格林尼治时间
> mydate1
ISODate("2017-11-25T07:38:00.833Z")
> typeof mydate1
object
```

```js
> var mydate2 = ISODate() //格林尼治时间
> mydate2
ISODate("2017-11-25T07:38:57.996Z")
> typeof mydate2
object
```
这样创建的时间是日期类型，可以使用`JS`中的`Date`类型的方法

返回一个时间类型的字符串
```js
> var mydate1str = mydate1.toString()
> mydate1str
Sat Nov 25 2017 15:38:00 GMT+0800 (CST)
> typeof mydate1str
string
```
或者
```js
> Date()
Sat Nov 25 2017 15:43:28 GMT+0800 (CST)
```

#### Boolean

此类型用于存储布尔值:`true / false`

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**