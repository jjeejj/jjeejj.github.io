---
title: ES6之modules模块规范
tags:
  - js
description: modules是 ES6 引入的最重要的一个特性
date: 2016-11-18 18:05:38
---

-------------------------------------------

`modules`规范分两部分，一部分是如何导出，一部分是如何导入

## 命名导出

可以直接在任何变量或函数前面加上一个`export`关键字，就可以将它导出

比如：
```js
export const sqrt = Math.sqrt;
export function square(x){
	return x * x;
}
export function diag(x, y) {
    return sqrt(square(x) + square(y));
}
```
然后在另一个文件中引用
```js
import {square,diag} from './name-export.js';

console.log(square(11)); // 121
console.log(diag(4, 3));
```
`import {square,diag}` 是`import`特有的语法，所以其实`import`的时候并不是直接把整个模块以对象的形式引入的。

如果你想**导入整个模块作为一个对象** ,可以使用 `import * as lib from ./name-export.js` 

## 默认导出

上面的写法比较麻烦，因为必须指定一个名字，很多时间模块导出的是一个变量，根本没必要指定名字

有一种用法叫默认导出，就是指定一个变量名作为默认导出

```js
// myFunc.js
export default function (){............}

```
```js
import myFunc from './myFunc.js'  // 这样导入时默认的
console.log('myFunc',myFunc) //myFunc function (){............}
```

默认导出的时候不需要指定一个变量名，它默认就是文件名

导出的默认值就是`deault`模块本身

## 命名导出结合默认导出

默认导出同样可以结合命名导出来使用

```js
export default function (obj) {
    ...
};
export function each(obj, iterator, context) {
    ...
}
export { each as forEach };
```
上面的代码导出了一个默认的函数，然后由导出了两个命名函数，我们可以这样导入：

```js
import _, { each } from 'underscore';
```

注意这个逗号语法，分割了默认导出和命名导出

其实这个默认导出只是一个特殊的名字叫 `default`，你也可以就直接用他的名字，把它当做命名导出来用

## 仅支持静态导入导出

ES6规范只支持静态的导入和导出，也就是必须要在编译时就能确定，在运行时才能确定的是不行的，比如下面的代码就是不对的：

```js
//动态导入
var mylib;
if (Math.random()) {
    mylib = require('foo');
} else {
    mylib = require('bar');
}
//动态导出
if (Math.random()) {
    exports.baz = ...;
}
```
为什么要这么做，主要是两点：

1. 性能，在编译阶段即完成所有模块导入，如果在运行时进行会降低速度
2. 更好的检查错误，比如对变量类型进行检查


--------------------------------------------------

>由于浏览器对`ES6`的支持表示很好，可以通过 `babel` 转换之后去查看效果