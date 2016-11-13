---
title: css之line-height属性
tags:
  - css
description: 'line-height 属性设置行间的距离（行高）,不允许使用负值'
date: 2016-11-13 21:07:46
---

---------------------------------
## line-height的定义

`line-height`，行高，两行文字基线之间的距离
* 什么是基线：字母 `x`下边缘的位置
![alt](/images/css之line-height属性/基线.png)
* 为什么是基线？---基线乃任意线（*）定义之根本
* 需要两行吗？---两行的定义已经决定了一行的表现

### baseline与line-height的关系
![alt](/images/css之line-height属性/baseline与line-height.png)

### baseline与字体的关系---不同的字体基线的位置是不一样的
上图的字体为微软雅黑的
下图的为：Arial)上)与Simsun(下)
![alt](/images/css之line-height属性/baseline与字体.png)

## line-height与行内框盒子模型

所有内联元素的样式表现都与行内框盒子模型有关

行内框盒子模型

`<p>这是一行普通的文字，这里有一个<em>em</em>标签</p>`

上面包含<font color="red">4</font>中盒子

1. 内容区域（content area）是一种围绕文字看不见的盒子。内容区域（content area）的大小与`font-size`大小相关
2. 内联盒子（inline-boxes），不会让内容成块显示，而是排成一行。如果外部有`inline`水平得标签，则属于内联盒子，如果没有则是匿名内联盒子
3. 行框盒子(line-boxes),每一行就是一个盒子。每个行框盒子，又是有一个个内联盒子组成（inline-boxes）
4. `<p`标签所在“包含盒子”（container box）,此盒子由一行一行的“行框盒子（line-boxes）”组成---`block`水平得盒子

## line-height的高度机理

### 文本占据的高度
`<p>这是一行普通的文字，这里有一个<em>em</em>标签</p>`

```js
console.log(document.querySelector('p').clientHeight);
//====>18px;
```

元素的高度从何而来呢？

行框盒子的高度由其中内联元素高度行高(`line-height`)决定的


### line-height明明是两基线之间的距离，单行文字哪来的行高？怎么控制高度的？

需要了解了：

1. 行高由于其继承性，影响无处不在，即使单行文本也不例外
2. 行高只是幕后黑手，**高度的表现不是行高**，而是内容区域和行间距

>内容区域 + 行间距 = 行高
>内容区域高度只与**字号（font-size）***和**字体(font-family)**,与`line-height`没有任何关系
>在`simsun`字体下，内容区域高度等于文本值大小

**总结：**
行高决定了内联盒子的高度，行间距可大可小（也可以为负值）。行间距保证**内容区域 + 行间距 =行高**

如果行框盒子模型中，与多个内联元素，行高怎么计算？

答案：一般有最高的行高得来
```html
<p class="test3">这是一行普通的文字，这里有一个<em style="line-height: 40px">em</em>标签</p>
console.log(document.querySelector('.test3').clientHeight);

//=======> 40px
```

但是特殊情况下不是，比如：
```html
<p class="test3">这是一行普通的文字，这里有一个<em style="line-height: 40px;vertical-align: 20px">em</em>标签</p>
console.log(document.querySelector('.test3').clientHeight);

//=======> 49px
```

含多个行框盒子的包含容器的高度：

**多行文本的高度就是单行文本高度累加**

## line-height的各类属性值

* **normal**
* **number**
* **length**
* **percent**
* **inherit**

### normal

默认属性值，跟着用户的浏览器走的，且与元素字体有关

### number

使用数字作为行高值：

	line-height: 1.5

根据当前元素的`font-size`大小计算的

### length

使用具体长度单位

```html
line-height: 1.5em;
```
```html
line-height: 1.5rem;
```
```html
line-height: 20px;
```
```html
line-height: 20pt;
```

### percent

使用百分比值作为行高值

	line-height: 150%

相对于设置了该`line-height`属性的元素的`font-size`大小计算的

### inherit

行高继承 IE8+

	input {line-height: inherit}

`input`框等元素的默认行高为`normal`,使用`inherit`可以让文本框样式可控性更强

### 补充

1. **line-height: 1.5** , **line-height: 150%**,**line-height: 1.5em**有什么区别？
①：计算没有区别
②：应用元素有差别：
* `line-height: 1.5` 所有可继承的元素根据`font-size`重计算行高
* `line-height: 150% | 1.5em ` 当前元素根据 `font-size`计算行高具体值，继承给下面元素

2. **body全局数值行高使用经验**

		body{font-size: 14px; line-height: 1.4286}

>匹配 20 像素的使用经验
> line-height = 20px /14 px =1.4286

可以缩写
```html
body{font: 14px/1.4286;}
```

## line-height与图片的表现

### 行高会不会改变图片的高度

行高**不会**影响图片实际占据的高度

默认为这样显示
![alt](/images/css之line-height属性/img-baseline.png)

添加行高 `line-height: 100px`
![alt](/images/css之line-height属性/img-baseline100px.png)

由于默认的是基线对其，所以容器高度增增加了和图片底部和文字底部有一定距离

### 如何消除图片底部间隙，由于基线对齐的原因

* 图片块状化---无基线对齐
	
	img {display: block}
	
* 图片底线对齐

	img {vertical-align: bottom}
	
* 行高足够小---基线位置下移

### 小图片和大文字

基本上高度受行高影响


## line-height 的实际应用

### 图片的水平垂直居中

```html
.img {
	background-color: #ccc;
	text-align: center;
	line-height: 300px;
	max-width: 100%;
}
img {
	vertical-align: middle;
}
<p class="img"><img src="../img/ad003.png" alt=""></p>
```

效果：
![alt](/images/css之line-height属性/图水平垂直居中.png)

>该方法只适合 IE8+以上的浏览器

### 多行文本水平垂直居中

```html
 .box{
	max-width: 100%;
	line-height: 250px;
	text-align: center;
	background-color: green;
}

.box > .text {
	display: inline-block;
	vertical-align: middle;
	line-height: normal;
	text-align: left;
}
<div class="box"><span class="text">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Iste, ullam.</span></div>
```
效果：
![alt](/images/css之line-height属性/多行文字水平垂直居中.png)

>**说明**：
>多行文字水平垂直居中的实现原理和图片的实现原理一样的，区别是要把多行文字所在容器的`display`水平转化为和图片一样的`inline-block`,
>以及重置外部继承的`text-align`和`line-height`属性值


## Q&A

### 为什么`line-height`可以让当行文本垂直居中？

不是真正的垂直居中，只是平常字体大小比较下，看上去垂直居中了