---
title: JavaScript数组的高级用法-reduce和reduceRight详解
date: 2016-10-23 12:57:14
tags: [js,Array]
description: ES6 新增了两个归并数组的方法：reduce() 和 reduceRight().这两个方法都会迭代数组的所有项，然后构建一个最终值返回。
---

-------------------------
### reduce 方法（升序,从数组的第一项开始）

**语法:**

	array.reduce(callback[,initialValue])
	
|**参数**|**定义**|
|:-----|:-----|
|***array***|必须，一个数组对象|
|***callback***|必须，一个接受做多4个参数的回调函数。对数组中的每一个元素，***reduce***方法会调用*callback*函数一次|
|***initialValue***|可选，如果指定*initialValue*，则它将用作初始值来累计。第一次调用*callback*函数会将此值作为参数，而非数组提供|

**返回值：**

　通过最后一次调用回调函数获得的累积结果。

**异常**

　当满足下列任一条件时，将引发**TypeError**异常：
  * *callback*参数不是函数对象
  * 数组不包含元素，且未提供*initiaValue*。
	
**回调函数语法**

	function callback(previousValue,currentValue,currentIndex,array)

|**回调参数**|**定义**|
|:-----|:-----|
|***previousValue***|通过上一次调用回调函数获得的值，如果向 ***reduce*** 方法提供 ***initialValue***，则在首次调用函数时，***previousValue*** 为 ***initialValue***。|
|***currentValue***|当前数组元素的值。|
|***currentIndex***|当前数组元素的数字索引。|
|***array***|包含该元素的数组对象|

### 第一次调用回调函数

**在第一次调用回调函数时，作为参数提供的值取决于 reduce 方法是否具有 initialValue 参数**

如果向 reduce 方法提供 initialValue：

* previousValue 参数为 initialValue
* currentValue 参数是数组中的第一个元素的值

如果未提供 initialValue：

* previousValue 参数是数组中的第一个元素的值
* currentValue 参数是数组中的第二个元素的值

#### 示例

1. 下面的示例将数组值连接成字符串，各个值用'::'分隔开。由于未向 *reduce* 方法提供初始值，第一次调用回调函数时会将’abc‘作为 *previousValue* 参数并将’def‘作为 *currentValue* 参数。

		function appendCurrnet(previousValue,currentValue){
			return previousValue + '::' + currentValue;
		}
		var array = ['abc','def',true,234];
		var result = array.reduce(appendCurrnet);
		console.log(result) //"abc::def::true::234"

2. 向数组添加舍入后的值。使用初始值 0 调用 *reduce* 方法,第一次调用时：*previousValue*为0

		function addRounded (previousValue, currentValue) {
			return previousValue + Math.round(currentValue);
		}
		var numbers = [10.9, 15.4, 0.5];
		var result = numbers.reduce(addRounded, 0);
		console.log(result) //27

3. 处理数组中的每一项,currentIndex 和 array 参数用于回调函数

		function addDigitValue(previousValue, currentValue, currentIndex, array) {
			var exponent = (array.length - 1) - currentIndex;
			var currentValue = currentValue * Math.pow(10, exponent);
			return previousValue + currentValue;
		}
		var digits = [4, 1, 2, 5];
		var result = digits.reduce(addDigitValue, 0);
		console.log(result) //(4000 + 100 + 25 + 5) =4125

**说明:** **1示例**只是为了说明该方法，可以简单的直接用[Array.join()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)方法得到结果
		
### reduceRight 方法（降序，从数组的最后一项开始）

reduceRight的语法以及回调函数的规则和reduce方法是一样的，区别就是在与reduce是升序，即角标从0开始，
而reduceRight是降序，即角标从arr.length-1开始。如果有初始值，则从最后一个数开始计算，
如果没有初始值，则previousValue参数是数组中最后一个元素的值，currentValue是数组中倒数第二个元素的值。

#### 示例
1. reduceRight 方法可应用于字符串。下面的示例演示如何使用此方法反转字符串中的字符

		function AppendToArray(previousValue, currentValue) {
			return previousValue + currentValue;
		}
		var word = "retupmoc";
		var result = [].reduceRight.call(word, AppendToArray);
		console.log(result); //computer
		
**说明:**也可用数组的reverse()方法,也可用于反转字符串，但不能用call方法直接调用。需先把字符串转化为数组

-----------------------------------
