---
title: webpack的基本使用
tags:
  - webpack
date: 2016-11-10 22:09:28
---

----------------------------------------
`webpack` 是一个模块打包器

`webpack`处理模块和它的依赖并且生成静态资源替代这些模块

<!--more-->

## 	安装 webpack

你需要已经安装了 `Node.js`

	npm install webpack -g

## 基本使用

### 第一步

在一个空的文件夹中
创建这些文件

***add entry.js***
```js
document.write('It works');
```
***add index.html***
```html
<html>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <script type="text/javascript" src="bundle.js" charset="utf-8"></script>
    </body>
</html>
```
**然后运行下面的命令**

	webpack ./entry.js bundle.js

将编译`entry.js`文件，并生成`bundle.js`文件

如果成功将看到下面的内容：
![alt](/images/webpack的使用/webpack-bundle-start.png)

然后在浏览器中打开`index.html` 可以看到`It works`

### 第二步

我们添加一些代码在另一个文件中
***add content.js***
```js
module.exports = "It works from content.js";
```
***更新entry.js***
```js
- document.write("It works.");
+ document.write(require("./content.js"));
```

然后重新编译

	webpack ./entry.js bundle.js
	
更新浏览器，你将看到 `It works from content.js`


>wenpack 将分析入口文件的其他依赖的文件，这些文件（也叫做模块）也被包含到`bundle.js`文件中。
>webpack将给每一个模块生成一个唯一的id，并且通过该id在bundle.js中保存每一个模块的入口。
>当启动的时候仅仅是入口文件被执行。当需要依赖时，会在很短的时间执行`require`函数，加载需要的依赖

## 加载器--loader

我们想要添加一个`css`文件在我们的应用中

`webpack`仅仅只能处理原生的`javascript`,因此我们需要`css-loader` 去处理 CSS 文件，我们还需要`style-loader`去应用样式，在CSS文件中

运行 `npm install css-loader style-loader` 安装这些加载器

下面就可以使用了：

***add style.css***
```css
body{
	background: yellow
}
```
***更新entry.js***
```js
+ require("!style!css!./style.css");
  document.write(require("./content.js"));
```

重新编译并刷新浏览器，可以看到黄色的背景

>通过加载器的前缀，去引用该模块。该模块通过一个加载器管道。这些模块加载器会改变文件的内容以特定的方式。
>在这些处理改变应用完成后，得到的结果就是一个`javascript`模块

**命令行方式加载加载器**

我们只需要绑定文件扩展，因此只需要写`require('./style.css')`

***更新entry.js***
```js
- require("!style!css!./style.css");
+ require("./style.css");
  document.write(require("./content.js"));
```
然后运行

	webpack ./entry.js bundle.js --module-bind 'css=style!css'

>有些环境可能要求为双引号：--module-bind "css=style!css"

刷新浏览器可以看到同样的结果

## 一个配置文件

我们希望把配置项移到配置文件中

利用上面修改之后的`entry.js`文件，只引入样式文件不做处理

***add webpack.config.js***
```js
module.exports = {
	entry: './entry.js',
	output: {
		path: __dirname,
		filename: 'bundle.js'
	},
	module: {
		loaders: [
			{test:/\.css$/,laoder:"style!css"}
		]
	}
}
```

然后我们可以运行

	webpack

运行结果就可以在页面上看到结果


>webpack 命令试图在当前文件下加载 wenpack.config.js 文件

## 补充

### 漂亮的输出

我们想要进度条和颜色，可以这样实现

	webpack --progress --colors
	
### 监听模式

我们不想每次改变都重新编译，可以这样实现

	webpack --watch
	
`webpack` 可以缓存没有改变的模块，没有变化的模块会在编译后缓存到内存中，而不会每次都被重新编译，所以监听模式的整体速度是很快的。

>重新编译之后不会自动刷新浏览器


### development server

```js
npm install webpack-dev-server -g
webpack-dev-server --progress --colors
```

绑定一个下个小型的`express server` 在本地的 `8080`端口 （localhost:8080）
托管你的静态资源和`bundle`文件（自动编译），当`bundle`文件编译完成，会自动更新浏览器页面。
打开 http://localhost:8080/webpack-dev-server 在浏览器浏览

>这个 dev server 利用的是 webpack 的监听模式
