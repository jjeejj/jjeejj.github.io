---
title: css之border属性
tags:
  - css
description: 'border 简写属性在一个声明设置所有的边框属性.可以按顺序设置如下属性：border-width,border-style,border-color'
date: 2016-11-20 17:51:22
---


---------------------------------

## border-width

`border-width`不支持百分比属性

原因：语义和使用场景决定的，不会因为设备就按比例变大

`border-width` 还支持一些关键字：
`thin---1px,medium(默认值)---3px,thick---5px`

为何`border-width`的默认值为`medium(3px)`，而`thin(1px)` 更常用？

原因：`border-style:double`至少`3px`才有效果

## border-style

常用的类型：

* solid 实线
* dashed 虚线

虚线的趣数据
**Chrome/FireFox**--------宽高比：3:1
![alt](/images/css之border属性/border-style-dashed.png)
**IE**---------------宽高比： 2:1
![alt](/images/css之border属性/border-style-dashed-ie.png)

>所以，ie看起来虚线比较密

* dotted 点线

**Chrome** ---------方形
![alt](/images/css之border属性/border-style-dotted.png)
**IE/FireFox**---------------圆形
![alt](/images/css之border属性/border-style-dotted-ie.png)

* double 双线

计算规则：

1px = 0+1+0;
2px = 1+0+1
3px = 1+1+1
4px = 1+2+1
5px = 2+1+2
6px = 2+2+2
7px = 2+3+2

>双线宽度永远相等，中间间隔+-1

实际例子：
```html
.double{
		width: 120px;
		height: 20px;
		border-top: 60px double;
		border-bottom: 20px solid;
	}
<div class="double">

</div>
```
效果：
![alt](/images/css之border属性/border-style-double.png)

* inset 内凹
* outset 外凹

等等其他的属性值..............

## border-color

为设置4个边框的颜色，可以设置1到4种颜色

`border-color:red green blue pink;`

* 上边框是红色
* 右边框是绿色
* 下边框是蓝色
* 左边框是粉色

`border-color:red green blue;`

* 上边框是红色
* 右边框和左边框是绿色
* 下边框是蓝色

`border-color:red green;`

* 上边框和下边框是红色
* 右边框和左边框是绿色

`border-color:red;`

* 所有 4 个边框都是红色

`border-color`的默认颜色就是`color`的颜色

案例：`hover` 图形边框变色


##  border与 background 定位

`background-position`定位的局限性：

* 只能相对左上角进行定位，不能相对于右下角进行定位

可以使用`border`实现定位

```html
 .pos{
		background-image: url(../img/ad002.png);
		background-repeat: no-repeat;
		border: 1px solid red;
		border-right: 50px solid transparent;
		width: 800px;
		height: 400px;
		background-position: 100% 40px;
	}
<div class="pos">
```

>因为 100% 左侧定位不计算 `border`

## border 与三角等图形的构建

基础图形

```html
.triangle{
            width: 100px;
            height: 100px;
            border: 100px solid;
            border-color: red green blue orange
        }
<div class="triangle">

</div>		
```

效果图：
![alt](/images/css之border属性/border-triangle.png)

>默认 `div`的宽高是不包括边框的


##  border与透明边框

始于 IE7 足够兼容


## border 在布局中的应用

不支持百分比宽度

1. border 与等高布局

```html
.box{
	border-left: 300px solid #222;
	height: 100px;
	text-align: center;
	background-color: red;
	}
.left{
	width: 300px;
	margin-left: -300px;
	float: left;
	color: #fff
}
<div class="box">
	<nav class="left">
		<h5>导航1</h5>
	</nav>
	<section class="right">
		<div class="module">模块1</div>
		<div class="module">模块1</div>
	</section>
</div>
```
效果
![alt](/images/css之border属性/border-equal-height.png)



