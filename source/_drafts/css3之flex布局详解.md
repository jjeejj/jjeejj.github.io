---
title: css3之flex布局详解
tags: [css3]
---
---------------------------------------
`Flexible Box` 弹性盒子（也叫做伸缩盒模型）

<!--more-->

![alt](/images/css3之flex布局详解/flex.png)

Flexible Box弹性盒子有两根轴,`main-axis` 和 `cross-axis`。两轴的开始/结束位置为`main-start/end`和`cross-start/end`。
盒子内的各元素在两轴空间为`main-size`和`cross-size`

语法属性：

1. 设置作用在容器上的

* **display**
* **flex-direction**
* **flex-wrap**
* **flex-flow**
* **justify-content**
* **align-items**
* **align-content**

2. 设置在内部元素上的

* **flex-grow**
* **flex-shrink**
* **flex-basis**
* **flex**
* **order**
* **align-self**

## 容器属性

### display

`display`属性新增了`flex`和`inline-flex`可以将盒子改为弹性盒子模型

```html
.divflex{
	border: 1px solid green;
	display: flex; /*flex布局*/  
	/*display: inline-flex; /*inline-flex布局*/*/  
}
.divflex div{
	margin:2px;
	width: 50px;
	height: 50px;
	border: 1px solid red;
}


<div class="divflex">
	<div>1</div>
	<div>2</div>
	<div>3</div>
	<div>4</div>
</div>

```

效果

`display: flex`
![alt]()
`display: inline-flex`
![alt]()

>