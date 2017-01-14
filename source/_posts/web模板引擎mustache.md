---
title: web模板引擎mustache
tags:
  - mustache
date: 2017-01-14 15:51:25
---


mustache 是一个轻逻辑(Logic-less) 模板解析引擎

<!--more-->

可以广泛应用在 [Ruby](https://github.com/mustache/mustache),[JavaScript](https://github.com/janl/mustache.js),[Node](https://github.com/raycmorgan/Mu),[Java ](https://github.com/spullara/mustache.java)等等

>具体支持的语言可查看[mustache](http://mustache.github.io/)

## 摘要

一个典型的 `mustache` 模板

```
Hello {{name}}
You have just won {{value}} dollars
{{#in_ca}}
Well, {{taxed_value}} dollars, after taxes
{{/in_ca}}
```
给出下面的值：
```js
{
  "name": "Chris",
  "value": 10000,
  "taxed_value": 10000 - (10000 * 0.4),
  "in_ca": true
}

```
得出以下的结果：
```
Hello Chris
You have just won 10000 dollars!
Well, 6000.0 dollars, after taxes.
```

## 描述

`mustache`可以用在`HTML`,配置文件，源代码...等等任何事情。
它通过扩展的标签在模板中使用一个对象或`hash`提供的值进行工作

我们称它为轻逻辑的，是因为没有`if`语句`else`从句，或者`for`循环，相反的这仅仅只有标签，一些标签被替换成一个值，一些替换为空的和其他的一系列值

## 标签类型

标签是通过`double mustaches` 表明，`{{person}}`是一个标签,
这个也是
```
{{#person}}
```
在上面我们把 **person**作为作为`key`或者`tag key`

### 变量

这个最基础的标签类型。一个 `{{name}}` 标签在一个基础的模板中试图去查找一个`name`的变量在当前上下文中 ，如果没有，父级上下文就会被递归的检查，
如果顶级上下文被达到，而且`name` 一直没有被发现，那么这个标签什么都不会渲染。

所有的变量在`HTML`中默认是转义的，如果想要没有转义的`HTML` 可以使用`triple mustache`
```
{{{name}}}
```

也可以使用`&`不转义一个变量如：
```
{{& name}}
```

默认的变量`miss`返回一个空字符串

```
Template:

 {{name}} 
 {{age}}
 {{company}}
 {{{company}}}

Hash:

{
  "name": "Chris",
  "company": "<b>GitHub</b>"
}
Output:
 
 Chris
 
 &lt;b&gt;GitHub&lt;/b&gt;
 <b>GitHub</b>

```

### 部分--块(Sections)

Sections 渲染一个文本块一次或者多次，依赖当前上下文`key`的值

一个`section` 以`#`开始，以`/`结束 
```
{{#person}} {{/person}}
```

>当数据对象的 key 值为 null, undefined, false；则不渲染输出任何内容

根据`key`的值分为以下情况：

#### False Values or Empty Lists

如果`person` 这个key 存在而且它的值为`False or Empty Lists`,则在`#`和`/`之间的`HTML`将不显示
```
Template:

	Shown.
	{{#person}}
	  Never shown!
	{{/person}}
Hash:

	{
	  "person": false
	}
Output:

	Shown.
```

#### Non-Empty Lists


如果`person` 这个key 存在而且它有一个`non-false`的值,则在`#`和`/`之间的`HTML`将被渲染和展示一次或多次

当值为`non-empty list`,`list`中的每一项都在块中的文本展示一次，这用方法我们可以循环一个集合

```
Template:

	{{#repo}}
	  <b>{{name}}</b>
	{{/repo}}
Hash:

	{
	  "repo": [
		{ "name": "resque" },
		{ "name": "hub" },
		{ "name": "rip" }
	  ]
	}
Output:

	<b>resque</b>
	<b>hub</b>
	<b>rip</b>
```

#### Lambdas

当值为可被调用的对象，比如`function`或者`lambda`,这个对象将被调用

#### Non-False Values

如果值是`non-false`而且不是`list`,就会被当做一个内容简单的渲染

```
Template:

	{{#person?}}
	  Hi {{name}}!
	{{/person?}}
Hash:

	{
	  "person?": { "name": "Jon" }
	}
Output:

	Hi Jon!
```

#### Inverted Sections--反转的块

`Inverted Sections`以`^`开始，以`/`结束
```
{{^person}} {{/person}}
```

`inverted sections` 可能渲染文本一次，基于反转`key`的值，也就是如果`key`不存在或者`false`或者`empty list`才会被渲染

```
Template:

	{{#repo}}
	  <b>{{name}}</b>
	{{/repo}}
	{{^repo}}
	  No repos :(
	{{/repo}}
Hash:

	{
	  "repo": []
	}
Output:
	
	No repos :(
```
>而且当数据对象的 key 值为 null, undefined, false 也可以渲染内容

#### Comments ---注释

开始于`!`而且是被忽略的

```
Template:

	<h1>Today{{! ignore me }}.</h1>
	
Output:

	<h1>Today.</h1>
```
>注释被包含在新的一行


参考文章：
>[mustache5](http://mustache.github.io/mustache.5.html)
