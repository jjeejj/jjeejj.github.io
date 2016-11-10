---
title: node之process模块
tags: [node]
---

---------------------------------

`process` 对象作为一个全局的对象提供信息，代表当前`Node.js`的进程。
作为全局对象，总可以在`Node.js`应用中使用而不需要使用`require()`。

<!--more-->

## Process Events

`process`对象是`EventEmitter`的一个实例

### 事件:'exit'

当进程将要退出时触发

* `process.exit()` 方法被明确的执行
* `Node.js` 事件循环不在执行任何额外的工作

>在该方法执行的时候没有办法阻止退出事件循环，一旦所有的`exit` 事件监听的方法执行完成，这个`Node.js`进程将终止

当监听函数被调用的时候，带有一个特定的`exit code` 该值可以通过[`process.exitCode`][]属性，或者是来自`process.exit()`获取，作为唯一的参数
```js
process.on('exit',(code)=>{
	console.log(`About to exit with code: ${code}`)
})
```

>Node 执行程序退出正常情况下会返回 0


事件监听函数必须执行同步的操作，`Node.js`进程将立马退出在`exit`事件执行之后，事件监听函数在事件循环队列中添加的任务，讲被遗弃

下面的实例中，`setTimeout`将永远不会执行
```js
process.on('exit',(code)=>{
	setTimeout(()=>{
		console.log('This will not run');
	},0);
})
```

### 事件:'message'

## Process Method

### process.cwd()

该方法返回，当前`Node.js process` 的工作目录
```js
console.log(`Current directory : ${process.cwd()}`);
```

### process.abort()

`process.abort()`方法导致`Node.js process`立即退出并且创建一个核心的文件（<font color="red">此处的核心文件不理解？？</font>）。
不会触发`exit`事件

### process.exit([code])

* `code <Integer>` 退出的code ,默认为 0,会触发 `exit`事件

该方法带有指定的`exit code` 极可能快的退出当前`Node.js` 终端进程。
如果`code`没有指定，exit 使用默认的`success` code `0` ,或者指定的`process.exitCode` 的值

退出并带有
```js
process.exit(1);
```


## Process Property 

### process.env

该属性返回一个对象包含用户的环境信息 [详细参见 environ](http://man7.org/linux/man-pages/man7/environ.7.html)

返回的对象和下面格式一致
```js
{
	USERNAME: "jiang",
	COMPUTERNAME: "JIANG-PC",
}
```

是可以修改这个对象的，但是一些修改不能影响外部的`Node.js process`.换句话说下面的例子不会起作用：
```js
node -e 'process.env.foo = "boo"' && echo $foo
```
然而这样写是可以的：
```js
process.env.foo = 'bar'
console.log(process.env.foo)
```

在`process.env` 上定义一个属性，将会隐式转换为`string`
```js
process.env.test = null
console.log(process.env.test);
console.log(typeof process.env.test);
// => 'null'
// => 'string'
process.env.test = undefined;
console.log(process.env.test);
// => 'undefined'
```

可以使用`delete`删除 `process.env` 的属性
```js
process.env.TEST = 1;
delete process.env.TEST;
console.log(process.env.TEST);
// => undefined
```

>补充：用命令怎么启动监听呢？
>Window:用户：①：set PORT = 1234 ②： node app.js 
>linux系统环境（苹果的mac也是这种情况）：PORT=1234 node app.js

### process.execPath

该属性返回开启当前`Node.js process` 的可执行文件的决对路径
```js
console.log('process.execPath:',process.execPath);
//---> process.execPath: d:\nodejs\node.exe
```

### process.exitCode

进程退出时的code,为数字。当进程优雅的退出或通过`process.exit()`没有指定退出codes

>通过`process.exit(code)` 指定的code 会覆盖任何之前通过`process.exitCode`设置的值

