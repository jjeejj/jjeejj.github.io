---
title: JavaScrip toString() 详解
tags:
  - js
date: 2017-01-15 17:48:18
---


`toString()`函数用于将**当前对象以字符串的形式返回**

该方法属于`Object`对象，由于所有的对象都"继承"了Object对象的实例，因此几乎所有的实例对象都可以使用该方法

<!--more-->

### 语法

```
object.toString()
```

### 返回值

`toString()`返回值为`String`类型，返回当前对象的字符串形式

`JavaScript`的许多内置对象都重写了该方法，以实现更适合自身的功能需要

|类型|行为描述|
|:--:|:----:|
|Array|将 Array 的每个元素转换为字符串，并将它们依次连接起来，两个元素之间用英文逗号作为分隔符进行拼接|
|Boolean|如果布尔值是true，则返回"true"。否则返回"false"|
|Date|返回日期的文本表示|
|Error|返回一个包含相关错误信息的字符串|
|Function|返回如下格式的字符串，其中 functionname 是一个函数的名称，此函数的 toString 方法被调用： "function functionname() { [native code] }"|
|Number|返回数值的字符串表示。还可返回以指定进制表示的字符串,numberObject.toString( [ radix ] ),radix:可选/Number类型指定的基数(进制数)，默认为10, 支持[2, 36]|
|String|返回 String 对象的值|
|Object(默认)|返回"[object ObjectName]"，其中 ObjectName 是对象类型的名称|

### 示例

```js
//数组
var array = ["jiang", true, 12, -5];
array.toString(); //"jiang,true,12,-5"
//布尔
var bool = true;
bool.toString(); //"true"
//Date
new Date().toString(); //Sun Jan 15 2017 17:37:58 GMT+0800 (中国标准时间)"
new Date('2123-12-22').toString(); //"Wed Dec 22 2123 08:00:00 GMT+0800 (中国标准时间)"
//Error
new Error('错了').toString();//"Error: 错了"(中间有一个空格)
//Function
var testshow = function test(){console.log('----------')};
testshow.toString(); //"function test(){console.log('----------')}"
//Number
var num =  15.26540;
num.toString();//"15.2654"
//Object
var obj = {name: "张三", age: 18};
obj.toString();//"[object Object]"
// HTML DOM 节点
var eles = document.getElementsByTagName("body");
eles.toString();// "[object HTMLCollection]"
eles[0].toString();//"[object HTMLBodyElement]"
```