---
title: javascript的delete操作符详解
tags:
  - js
date: 2017-01-06 22:16:06
---


`javascript` 中 `delete` 操作符的运用及原理

<!--more-->

## 理论

为什么我们可以删除一个对象的属性：

```js
var x = {a:1,b:2}
delete x.a //true
x.a //undefined
```
但不能删除一个变量：

```js
var x = 1;
delete x; // false;
x; // 1
```

也不能删除一个函数：

```js
function x(){};
delete x; //false
typeof x; //"function"
```

>当 **delete**无法删除一个属性时才返回 **false**

为了理解这些问题，我们需要知道一些概念，变量实例化（variable instantiation）和 属性的内部属性（property attributes）

### 代码类型

`javascript`中有三类可执行的代码：

1. 全局代码 Globle code
2. 函数代码 Function code
3. Eval code

说明：

1. 当一个源文件被看做一个程序，它在全局作用域(scope)内执行，而这就被认为一段全局代码 **Globle code**
在浏览器的环境下，`SCRIPT`元素的内容通常被解析为一个程序，因而作为全局代码来执行
2. 任何一段在函数中直接执行的代码就被认为一段函数代码**Function code**，在浏览器环境下，事件属性的内容（`<a onclick="...">`）通常都作为函数代码来解析和执行
3. 放在内建函数**eval**中的代码就被作为**Eval code**来解析

### 代码执行的上下文

当 `javascript`代码执行的时，总是发生在一个特定的执行上下文（`context`）中。执行作用域是一个抽象的实体，有助于理解作用域和变量实例化的工作原理。
上面三类可执行的代码都有各自的执行上下文。当函数代码执行时就进入了函数代码的执行上下文，以此类推

执行上下文在逻辑上是一个栈(stack)。首先有一段全局代码，它拥有属于自己的执行上下文；这段代码中可能调用一个函数，这个函数有自己的执行上下文；
这个函数可能调用另一个函数，等等。**即使当函数递归调用自己时，每一步调用仍然进入不同的执行上下文**（需要有效的去验证？）

### 活化对象和变量对象

每一个执行上下文都有一个与之相关的变量对象（Variable Object）。变量对象是一个抽象实体，一种用来描述变量实例化的机制。
在一段源代码中声明的变量和函数被作为变量对象的属性添加到变量对象上。
当进入了全局代码的执行上下文时，一个全局对象被用作变量对象。 这恰恰是为什么全局声明的变量和函数变成一个全局对象的属性的原因：
```js
var GLOBAL_OBJECT = this;
var foo = 1;
GLOBAL_OBJECT.foo; //1
function bar(){};
typeof GLOBAL_OBJECT.bar; // "function"
GLOBAL_OBJECT.bar === bar; //true
```
全局变量和全局函数都成了全局对象的属性。
那些局部变量，在函数内部声明的变量也成了变量对象的属性。唯一的区别就是，在函数代码中，变量对象不是一个全局对象，而是我们称之为**活化对象**
每次进入函数代码的执行上下文时都会创建一个活化对象。

并非只有在函数代码中声明的变量和函数才成为活化对象的属性，函数的每一个实参（arguments，以各自相对应的形参的名字为属性名）， 
以及一个特殊的Arguments对象(以 arguments 为属性名)同样成为了活化对象的属性。
需要注意的是，活化对象作为一个内部的机制事实上不能被程序代码所访问。
```js
(function(foo) {
    var bar = 2;
    function baz() {};
		/*
			在抽象的过程中，
			特殊的'arguments'对象变成了所在函数的活化对象的属性：
			ACTIVATION_OBJECT.arguments = arguments;
			...参数'foo‘也是一样：
			ACTIVATION_OBJECT.foo; // 1
			...变量'bar'也是一样：
			ACTIVATION_OBJECT.bar; // 2
			...函数'baz'也是一样：
			typeof ACTIVATION_OBJECT.baz; // "function"
       */
}) (1);
```
 
最后**Eval code** 中声明的变量成为了**上下文的变量对象**的属性，简单地使用在执行它的的执行上下文的变量对象
```js
var GLOBAL_OBJECT = this;
eval('var foo = 1');
GLOBAL_OBJECT.foo; // 1;
(function() {
    eval('var bar = 2');
    /*
        在抽象过程中
        ACTIVATION_OBJECT.bar; // 2
    */
})();
```

### 内部属性

我们明确了变量发生了什么（它们成了属性），剩下的需要理解的概念就是属性的内部属性（property attributes）。
每一个属性第一拥有零至多个内部属性比如：`ReadOnly`,`DontEnum`,`DontDetele`等等

当声明变量和函数时，它们就成了变量对象的属性，这些属相伴随生成了内部属性**DontDetele**
然而，任何显式\隐式***赋值***的属性都不会生成**DontDetele**，而这就是本质上为什么我们能删除一些属性而不能删除其他的原因。
```js
var GLOBAL_OBJECT = this;
/**
	'foo'是全局对象的一个属性，
    它通过变量声明而生成，因此拥有内部属性DontDelete
    这就是为什么它不能被删除
*/
var foo = 1;
delete foo; // false
typeof foo; // "number"

/**
	'bar'是全局对象的一个属性，
	它通过变量声明而生成，因此拥有DontDelete子
	这就是为什么它同样不能被删除
*/
function bar() {};
delete bar; // false
typeof bar; // "function"
/**
	'baz'也是全局对象的一个属性，
    然而，它通过属性赋值而生成，因此没有DontDelete
    这就是为什么它可以被删除
*/
GLOBAL_OBJECT.baz = "baz";
delete GLOBAL_OBJECT.baz; // true
typeof GLOBAL_OBJECT.baz; // "undefined"
```

### 内建对象的DontDelete

属性的一个特殊的内部属性控制着该属性是否可以被删除。
内建对象的一些属性拥有内部属性 DontDelete，因此不能被删除
特殊的 `arguments` 变量（如我们所知的，活化对象的属性）拥有 `DontDelete`； 任何函数实例的 `length` (返回形参长度)属性也拥有 `DontDelete`
```js
(function() {
    //不能删除'arguments'，因为有DontDelete
	console.log(delete arguments); // false;
    typeof arguments; // "object"
    //也不能删除函数的length,因为有DontDelete
    function f() {};
    delete f.length; // false;
    typeof f.length; // "number"
})();
```

与`arguments` 想关的属性也拥有 `DontDelete` 不能被删除
```js
(function(foo,bar) {
    delete foo; // false
    foo; // 1
    delete bar; // false
    bar; // "bah"
}) (1,"bah");
```

### 未声明的变量赋值
未声明的变量赋值会成为全局对象的属性
而现在我们了解了**属性赋值**和**变量声明**的区别——后者生成 `DontDelete` 
而前者不生成——这也就是为什么未声明的变量赋值可以被删除的原因了

```js
var GLOBAL_OBJECT = this;
/* 通过变量声明生成全局对象的属性，拥有DontDelete */
var foo = 1;
/* 通过未声明的变量赋值生成全局对象的属性，没有DontDelete */
bar = 2;
delete foo; // false
delete bar; // true
```

>内部属性在属性生成的时候确定，之后的赋值过程不会改变已有的属性的内部属性

```js
/* 'foo'创建的同时生成DontDelete */
function foo() {};
/* 之后的赋值过程不改变已有属性的内部属性，DontDelete仍然存在 */
foo = 1;
delete foo; // false;
typeof foo; // "number"
/* 但赋值一个不存在的属性时，创建了一个没有内部属性的属性，因此没有DontDelete */
this.bar = 1;
delete bar; // true;
typeof bar; // "undefined"
```

>Eval code 中声明的变量事实上生成一个没有 DontDelete 的属性,隐式赋值

