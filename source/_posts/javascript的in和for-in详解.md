---
title: javascript的in和for..in详解
tags:
  - js
date: 2017-01-28 17:17:00
---

遍历一个对象的属性，把属性名和属性值都提出来
判断一个属性是否属于一个对象

<!--more-->

### for...in

**for...in** 语句用于遍历**数组**或者**对象的属性**

>对数组或者对象的属性进行循环操作

for ... in 循环中的代码每执行一次，就会对**数组的元素**或者**对象的属性**进行一次操作

语法:
```js
for (变量 in 对象)
{
    在此执行代码
}
```
>变量:用来指定变量，指定的变量可以是数组的属性，也可以是对象的属性
>如果是数组属性默认为下标

实例:
```js
//数组
var mycars = ["Saab","Volvo","BMW"];

for(var x in mycars){
	console.log(x);
	console.log(mycars[x]);
}
//0
//Saab
//1
//Volvo
//2
//BMW

//对象
var mycars = {
	s:"Saab",
	v:"Volvo",
	b:"BMW"
}
for(var x in mycars){
	console.log(x);
	console.log(mycars[x]);
}

//s
//Saab
//v
//Volvo
//b
//BMW
```
>对于遍历的key是string类型,所以 typeof x == "string"
>数组是特殊的对象
>使用for-in进行循环也被称为“枚举”
>如果数组/对象prototype的原型上添加一个函数如:Array.prototype.test=function(){}/Object.prototype.test=function(){},也会遍历出来,可以使用hasOwnProperty()获得本身的属性，排除原型链上的属性
>由于以上的原因，尽量不要使用该方法遍历数组
>在for-in中，属性列表的顺序（序列）是不能保证的,所以最好数组使用正常的for循环，对象使用for-in循环

### in

in操作符用来判断某个属性属于某个对象，可以是对象的直接属性，也可以是通过prototype继承的属性

对于一般的对象属性需要用字符串指定**属性的名称**:
```js
var mycar = {
	make: "Honda",
	model: "Accord", 
	year: 1998
}
console.log("make" in mycar); //true
console.log("makse" in mycar); //false

```
对于数组属性需要指定数字形式的**索引值**来表示数组的属性名称（固有属性除外，如length）
```js
var trees = new Array("redwood", "bay", "cedar", "oak", "maple");
0 in trees        // returns true
3 in trees        // returns true
6 in trees        // returns false
"bay" in trees    // returns false (you must specify the index number,
                  // not the value at that index)
"length" in trees // returns true (length is an Array property)
```
in的右边**必须是一个对象**,如：你可以指定一个用String构造器生成的，但是不能指定字符串直接量的形式：
```js
var color1 = new String("green");
"length" in color1 // returns true
var color2 = "coral";
"length" in color2 // Uncaught TypeError
```

如果你使用delete操作符删除了一个属性，再次用in检查时，会返回false
如果你把一个属性值设为undefined，但是没有使用delete操作符，使用in检查，会返回true