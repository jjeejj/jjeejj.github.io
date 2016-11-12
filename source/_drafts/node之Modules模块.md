---
title: node之Modules模块
tags: [node]
---
----------------------------------
`Node.js`有一个简单的模块加载系统，在`Node.js`中文件和模块是一一对应的。

<!--more-->

下面例子中`foo.js`加载同一目录下的`circle.js`模块

`foo.js`里面的内容：
```js
const circle = require('./circle.js');
console.log(`The area of a circle of radius 4 is ${circle.area(4)}`);
```
`circle.js` 里面的内容：
```js
const PI = Math.PI;
exports.area = (r)=>{retrun PI * r *r };
exports.circumference = (r)=>{return 2 * PI * r};
```
`circle.js` 暴露了`area()`和`circumference()`函数，要添加`functions` 和 `object` 在你的模块的根上，你可以添加他们到`exports`这个指定的对称上

模块本地的变量是私有的，因为这个模块被`Node.js`的一个方法***(module wrapper)***包裹着。
在这个例子中，`PI`这个变量是`circle.js`私有的

如果你想要你的模块根上暴露出一个函数（比如构造函数），或者你想要暴露一个完整的对象，你可以把他分配给`module.exporst` 代替`exports`

>注意，exports是module.exports的一个引用，只是为了用起来方便

下面，`bar.js`使用了`square`模块，`square`模块暴露了一个构造函数
```js
const square = require('./square.js');
var mySquare = square(2);
console.log(`The area of my square is ${mySquare.area()}`);
```
`square`模块被定义在`square.js`中
```js
module.exports = (width)=>{
	return {
		area:()=>{retrun width* width}
	}
}
```

模块系统的实现在require("module")中

## 访问主模块

当`Node.js`直接运行一个文件时，`require.mian` 就被设置为`module`。也就是说你可以判断一个文件是不是直接被运行	

	require.main === module
	
对于一个`foo.js`文件，如果直接运行`node foo.js`是`true`,但是通过`require('./foo,js')`得到的事`false`

因为`module`提供一个`filename`属性（通常等于`__filename`）.所以当前应用的入口点，可以通过`require.main.filename`获取

## module 对象

在每一个模块中，变量`module`代表当前模块对象的引用，特别的 `module.exports`可以通过全局模块对象 `exports` 获取到。 
`module` 不是事实上的全局对象，而更像是每个模块内部的

### module.children

* `<Array>`

这个模块引入的所有模块对象--------不包括`Node.js`核心基础模块