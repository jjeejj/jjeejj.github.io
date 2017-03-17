---
title: Node原生Util模块详解
tags: [node]
---

`util`模块为了`Node`自己内部的而设计的`Api`,然而这个工具方法对于应用和模块开发者也是有用的

使用方法

`const util = require('util');`

<!--more-->

### util.debuglog(section)

### util.inspect(object[, options])

返回一个对象的字符串表现形式，在代码调试的时候非常有用

* `object`:Any JavaScript primitive or Object
* `options`:配置项
		`showHidden`<boolean>如果为 true，将会显示对象的不可枚举属性。默认为 false
		`depth`<number> 告诉 inspect 格式化对象时递归多少次。这在格式化大且复杂对象时非常有用。默认为 2。如果想无穷递归的话，传 null 
		`colors`如果为 true, 输出内容将会格式化为有颜色的代码。默认为 false
		`customInspect`如果为 false, 那么定义在被检查对象上的inspect(depth, opts) 方法将不会被调用。 默认为true