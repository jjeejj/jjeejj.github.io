---
title: ECMAScript6系列之-简介(-)
tags: [ES6,js]
description: ECMAScript 6.0（以下简称ES6）是JavaScript语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言
---

## ECMAScript和JavaScript的关系 

一个常见的问题是，ECMAScript和JavaScript到底是什么关系？

要讲清楚这个问题，需要回顾历史。
1996年11月，JavaScript的创造者Netscape公司，决定将JavaScript提交给国际标准化组织ECMA，希望这种语言能够成为国际标准。
次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript.
这个版本就是1.0版。

该标准从一开始就是针对JavaScript语言制定的，但是之所以不叫JavaScript，有两个原因。
一是商标，Java是Sun公司的商标，根据授权协议，只有Netscape公司可以合法地使用JavaScript这个名字，且JavaScript本身也已经被Netscape公司注册为商标。
二是想体现这门语言的制定者是ECMA，不是Netscape，这样有利于保证这门语言的开放性和中立性

因此，ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现（另外的ECMAScript方言还有Jscript和ActionScript）。
日常场合，这两个词是可以互换的。

## ES6与ECMAScript 2015的关系

经常可以看到”ECMAScript 2015“这个词，它与ES6是什么关系呢？

2011年，ECMAScript 5.1版发布后，就开始制定6.0版了。因此，”ES6”这个词的原意，就是指JavaScript语言的下一个版本

但是，因为这个版本引入的语法功能太多，而且制定过程当中，还有很多组织和个人不断提交新功能。
事情很快就变得清楚了，不可能在一个版本里面包括所有将要引入的功能。
常规的做法是先发布6.0版，过一段时间再发6.1版，然后是6.2版、6.3版等等。

但是，标准的制定者不想这样做。他们想让标准的升级成为常规流程：
任何人在任何时候，都可以向标准委员会提交新语法的提案，然后标准委员会每个月开一次会，评估这些提案是否可以接受，需要哪些改进。
如果经过多次会议以后，一个提案足够成熟了，就可以正式进入标准了。
这就是说，标准的版本升级成为了一个不断滚动的流程，每个月都会有变动。

标准委员会最终决定，标准在每年的6月份正式发布一次，作为当年的正式版本。
接下来的时间，就在这个版本的基础上做改动，直到下一年的6月份，草案就自然变成了新一年的版本。
这样一来，就不需要以前的版本号了，只要用年份标记就可以了

ES6的第一个版本，就这样在2015年6月发布了，正式名称就是《ECMAScript 2015标准》（简称ES2015）。
2016年6月，小幅修订的《ECMAScript 2016标准》（简称ES2016）如期发布，这个版本可以看作是ES6.1版，
因为两者的差异非常小（只新增了数组实例的includes方法和指数运算符），基本上是同一个标准。
根据计划，2017年6月将发布ES2017标准。

因此，ES6既是一个历史名词，也是一个泛指，含义是5.1版以后的JavaScript的下一代标准，涵盖了ES2015、ES2016、ES2017等等，
而ES2015则是正式名称，特指该年发布的正式版本的语言标准。

## 语法提案的批准流程 

任何人都可以向`TC39`标准委员会提案。一种新的语法从提案到变成正式标准，需要经历五个阶段。每个阶段的变动都需要由`TC39`委员会批准

1. Stage 0 - Strawman（展示阶段）
2. Stage 1 - Proposal（征求意见阶段）
3. Stage 2 - Draft（草案阶段）
4. Stage 3 - Candidate（候选人阶段）
5. Stage 4 - Finished（定案阶段）

一个提案只要能进入Stage 2，就差不多等于肯定会包括在以后的正式标准里面。

ECMAScript当前的所有提案，可以在TC39的[官方网站](https://github.com/tc39/ecma262) 查看

## ECMAScript的历史

ES6从开始制定到最后发布，整整用了15年。

前面提到，ECMAScript 1.0是1997年发布的，接下来的两年，连续发布了ECMAScript 2.0（1998年6月）和ECMAScript 3.0（1999年12月）。
3.0版是一个巨大的成功，在业界得到广泛支持，成为通行标准，奠定了JavaScript语言的基本语法，以后的版本完全继承。
直到今天，初学者一开始学习JavaScript，其实就是在学3.0版的语法。

2000年，ECMAScript 4.0开始酝酿。这个版本最后没有通过，但是它的大部分内容被ES6继承了。因此，ES6制定的起点其实是2000年。

为什么ES4没有通过呢？因为这个版本太激进了，对ES3做了彻底升级，导致标准委员会的一些成员不愿意接受。
2007年10月，ECMAScript 4.0版草案发布，本来预计次年8月发布正式版本。但是，各方对于是否通过这个标准，发生了严重分歧。
以Yahoo、Microsoft、Google为首的大公司，反对JavaScript的大幅升级，主张小幅改动，
以JavaScript创造者Brendan Eich为首的Mozilla公司，则坚持当前的草案。

2008年7月，由于对于下一个版本应该包括哪些功能，各方分歧太大，争论过于激烈，ECMA开会决定，中止ECMAScript 4.0的开发
将其中涉及现有功能改善的一小部分，发布为ECMAScript 3.1，而将其他激进的设想扩大范围，放入以后的版本，由于会议的气氛，该以后版本的项目代号起名为Harmony（和谐）
会后不久，ECMAScript 3.1就改名为ECMAScript 5。

2009年12月，ECMAScript 5.0版正式发布。
Harmony项目则一分为二，一些较为可行的设想定名为JavaScript.next继续开发，后来演变成ECMAScript 6。
一些不是很成熟的设想，则被视为JavaScript.next.next，在更远的将来再考虑推出。
TC39委员会的总体考虑是，ES5与ES3基本保持兼容，较大的语法修正和新功能加入，将由JavaScript.next完成。
当时，JavaScript.next指的是ES6，第六版发布以后，就指ES7。

2011年6月，ECMAscript 5.1版发布，并且成为ISO国际标准（ISO/IEC 16262:2011）
2013年3月，ECMAScript 6草案冻结，不再添加新功能。新的功能设想将被放到ECMAScript 7。
2013年12月，ECMAScript 6草案发布。然后是12个月的讨论期，听取各方反馈。
2015年6月，ECMAScript 6正式通过，成为国际标准。从2000年算起，这时已经过去了15年

## Babel转码器

[Babel](https://babeljs.io/) 是一个ES6的转码器，可以将ES6转化为ES5代码，从而在现在的环境中运行。
这就意味着，你可以使用ES6进行编码，不用担心环境的支持问题。[中文网](http://babeljs.cn/)
```js
input.map((item)=>{return item +1})
```
转化为
```js
input.map(function (item) {
  return item + 1;
});
```

### 配置文件 .babelrc

Babel 的配置文件是 `.babelrc`,存放早项目的根目录下。使用Babel的第一步就是配置这个文件
该文件用来设置**转码规则**和**插件**，基本格式如下：
```json
{
	"presets":[],
	"plugins":[]
}
```
`presets`字段设置转码规则，官网提供以下的规则集，可以根据需要安装：
```js
# ES2015转码规则
npm isntall babel-preset-es2015 --save-dev

# react 转码规则
npm isntall babel-preset-react --save-dev

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
npm install babel-preset-stage-0 --save-dev
npm install babel-preset-stage-1 --save-dev
npm install babel-preset-stage-2 --save-dev
npm install babel-preset-stage-3 --save-dev
```

然后将这些规则加入 `.babelrc`
```json
{
	"presets":[
		"es2015",
		"react",
		"stage-2"
	],
	"plugins":[]
}
```
>     注意，以下所有Babel工具和模块的使用，都必须先写好.babelrc。

### 命令行转码babel-cli

首先进行安装
`npm install babel-cli -g`

用法：
```js
// 转码结果输出到标准输出
babel exanple.js

// 将转码结果输出到文件中
babel example.js -o compiled.js

//监听变化文件
babel example.js -w -o compiled.js

// 整个目录转码
// --out-dir 或 -d 参数指定输出目录
babel src -d lib
```
>    具有的命令可以查看 `babel --help`查看


还可以安装在项目中，不依赖全局环境
`npm install babel-cli --save-dev`

修改`package.json`文件
```json
{
  
  "scripts": {
    "build": "babel babel -d lib"
  },
  "devDependencies": {
    "babel-cli": "^6.18.0",
    "babel-preset-es2015": "^6.18.0",
    "babel-preset-react": "^6.16.0",
    "babel-preset-stage-2": "^6.18.0"
  }
}
```
直接运行命令
`nom run build`

就可以得到结果

>     说明文档(http://babeljs.cn/docs/usage/cli/)

### babel-node

>不应该在生产环境中使用

`babel-cli`自带一个`babel-node`,提供一个支持ES6的REPL环境。它支持Node的REPL环境的所有功能，而且可以直接运行ES6代码。
它不用单独安装，而是随`babel-cli`一起安装。然后，执行`babel-node`就进入REPL环境。

使用：

`babel-node [options] [ -e script | script.js ] [arguments]`

或者进入 `REPL`环境

```
>(x=> x *2)(2)
4
```

`babel-node`命令可以直接运行ES6脚本。将上面的代码放入脚本文件es6.js，然后直接运行。
`babel-node`也可以安装在项目中。

`npm install babel-cli --save-dev`

然后修改`package.json`文件
```
{
  "scripts": {
    "babel-node": "babel-node script.js"
  }
}
```
上面代码中，使用babel-node替代node，这样script.js本身就不用做任何转码处理。

### require hook
