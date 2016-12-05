---
title: css之overflow属性
tags:
  - css
description: 'overflow 为CSS中设置当对象的内容超过其指定高度及宽度时如何管理内容的属性,overflow 属性规定当内容溢出元素框时发生的事情'
date: 2016-11-08 16:33:22
---

------------------------------

## Overflow 基本属性

### 基本属性值

* **visible**(默认,但是IE6以下会把后面的容器撑开) ![visible](/images/css之overflow属性/visible属性值.png)
* **hidden**(超出的部分隐藏) ![visible](/images/css之overflow属性/hidden属性值.png)
* **scroll**(容器的两侧出现滚动条) ![visible](/images/css之overflow属性/scroll属性值.png)
* **auto**(智能模式,滚动条出现的时机) ![visible](/images/css之overflow属性/auto属性值.png)
* **inherit** (继承，不经常用，需IE8以上才支持)

>     对于IE7 设置为`auto`时，横向和竖向都会出现滚动条
>     overflow-x:hidden ========> overflow:hidden auto
>     overflow-y:hidden ========> overflow:auto hidden

>     不同浏览器宽度设定机制不一样，出现滚动条会占用父容器的宽高

### 起作用的前提

1. 父容器非 `display:inline` 水平
2. 对应方位的尺寸限制.`width/height/max-width/max-height/absolute拉伸`
3. 对于单元格`td`元素等，还需要设置`table`为`table-layout:fixed`状态

## Overflow 与滚动条

### 滚动条出现的条件

1. `overflow:aotu/overflow:scroll`  ------------`<html><textarea>` 天生就有滚动条
2. 内容超出范围 

### body/html 与滚动条

无论什么浏览器，**默认的滚动条均来自**`<html>`,而不是`<body>`标签

>IE7-浏览器，右侧一直出现滚动条，相当于`html{overflow:scroll;}`
>IE8+浏览器：相当于:`html{overflow:auto;}`

所以，如果我们想去掉页面默认的滚动条，只需要：
```css
html{overflow:hidden;}
```
### 页面js滚动高度

* chrome浏览器：`document.body.scrollTop`
* 其他浏览器： `document.documentElement.scrollTop`

>2016年11月5号测试的时候，浏览器这两个方法都获得支持

建议使用：
```js
var st = document.body.scrollTop || document.documentElement.scrollTop;
```

### overflow的滚动底部的padding缺失现象
例如：
```html
.container{width: 300px; height: 300px;padding: 0 100px;border:1px solid red; overflow-y: hidden;}
```
效果：
Chrome: ![chrome](/images/css之overflow属性/scroll-padding-chrome.png)
其他浏览器：![other](/images/css之overflow属性/scroll-padding-other.png)

>这个现象就会导致不同浏览器的 scrollWidth或scrollHeight不一致的现象

### 滚动条的宽度机制

***滚动条会占用容器可用的宽度和高度***

获取滚动条（滚动栏）的宽度
```css
.box{width: 400px;overflow: scroll;}
.in{zoom: 1;}
```
```html
<div class="box">
	<div id="in" class="in"></div>
</div>
```
```js
console.log(400-document.getElementById('in').clientWidth);
```

**结果**：不同浏览器均是 17px

### 自定义滚动条

-webkit内核--Chrome浏览器

**整体部分**
```
::-webkit-scrollbar
```
**两端按钮**
```
::-webkit-scrollbar-button
```
**外层轨道**
```
::-webkit-scrollbar-track
```
**内层轨道**
```
::-webkit-scrollbar-track-piece
```
**滚动滑块**
```
::-webkit-scrollbar-thumb
```
**边角**
```
::-webkit-scrollbar-corner
```

IE浏览器也可以自定义滚动条

>juqery自定义滚动插件：http://manos.malihu.gr/jquery-custom-content-scroller/

## overflow 与 BFC

### BFC 是什么？

**BFC(Block formatting context)** "块级格式化上下文"

页面结界内，内部的元素在怎么翻云覆雨都不会影响外部

### 影响及应用

overflow 的属性值，会不会触发BFC

* visible   不会
* auto      会
* scroll    会
* hidden    会

应用：
1. 清除浮动的影响
2. 避免margin穿透问题
3. 两栏自适应布局

## overflow 与 absolute 绝对定位

绝对定位元素不总是被父级元素`overflow`属性剪裁，尤其当`overflow`在绝对定位元素及其***包含块***之间的时候，则失效

>包含块指：“含`position:relative/absolute/fixed`声明的父级元素，如果没有则是`body`元素”

如何避免失效：

* `overflow`元素自身为包含块
* `overflow`元素的子元素为包含块
* 任意`transform`声明当做包含块
![alt](/images/css之overflow属性/transform.png)

## 依赖 overflow 的样式表现

css3 属性 `resize`,可以拉伸元素尺寸：

* **both**:水平垂直两边拉伸，可调整元素的宽度和高度
* **horizontal**：水平拉伸，可调整元素的宽度
* **vertical**:垂直拉伸，可调整元素的高度

>如果希望此属性生效，需要设置元素的 overflow 属性，值可以是 auto、hidden 或 scroll，不可以是 visible。

`ellipsis` 文字溢出'...'省略号表示

`text-overflow:ellipsis` :多出的文字...表示

>如果希望此属性生效，需要设置元素的 overflow 属性值为 hidden
```html
 #btn{
            width: 50px;
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
        }
<button id="btn">Lorem ipsum dolor sit amet, consectetur adipisicing elit.</button>
```	
效果：
![alt](/images/css之overflow属性/ellipsis.png)


## overflow 与 锚点技术

锚点定位的本质：

条件：

1. 容器可滚动
2. 锚点元素在容器内

本质：（改变容器的滚动高度）

1. 触发锚点定位
2. 锚点元素通过`scrollTop` 值改变向上偏移定位
3. 锚元素的上边缘与可滚动容器的上边缘对齐

锚点定位的触发：

1. url地址中的锚链与锚点元素
2. 可`focus`的锚点元素处于`focus`态

作用：

1. 快速定位
2. 锚点定位与`overflow`选项卡技术（体验不是很好）

## 补充

1. 对于IE7浏览器一下，文字越多，按钮两侧的`padding`留白就越大的问题


		为按钮添加css样式overflow:visible 就可以解决了

2. 水平居中跳动的问题（由于**滚动条会占用容器可用的宽度和高度**）

		.container{width:1150px;margin:0 auto}
		由于滚动条的出现，导致可用宽度变化，会向左偏移
		修复方法:
		1：默认显示滚动栏 html{overflow-y:scroll}
		2:.增加.container{padding-left:calc(100vm-100%)}
			100vm:浏览器的宽度，100%:容器的可以宽度（IE9+浏览器才支持）
			
3. IOS原生滚动回调效果

		-webkit-overflow-scrolling: touch;
