title: background-repeat  属性详解
date: 2016-10-26 14:24:54
tags: [css]

---
---------------------------------------

**background-repeat** 是 **background** 属性之一。在CSS2.1中，众所周知，**background-repeat** 属性具有四个常见的值 **repeat、repeat-x、repeat-y和no-repeat**。
在 [CSS Backgrounds and Borders Module Level 3](https://www.w3.org/TR/css3-background/#background-repeat)中，给 **background-repeat** 新增加了两个属性值 **round和space**

<!--more-->

## 回顾background-repeat的使用

在CSS2.1中 **background-repeat** 具有四个属性值**repeat、repeat-x、repat-y和no-repeat**。先简单的回顾一下

### repeat--默认属性

**background-repeat** 取值为 **repeat** 时，表示背景图片沿容器 **X** 和 **Y** 轴平铺。将平铺满整个容器。

	body{
		background:url(https://avatars2.githubusercontent.com/u/15176971?v=3&s=40) repeat
	}
	
或者
	
	body{
		background-image:url(https://avatars2.githubusercontent.com/u/15176971?v=3&s=40);
		background-repeat:repeat
	}

效果如下：

![alt](/images/background-repeat属性详解/bg-repeat.png)
>当使用 **background-repeat** 取值为 **repeat** 时，如果背景图片尺寸和容器尺寸不能完全匹配时，会造成背景图片不全，如上图红框区域

### repeat-x

**repeat-x** 可以说是 **repeat** 的分值之一，表示背景图片沿容器的 **X** 轴平铺。

	body{
		background:url(https://avatars2.githubusercontent.com/u/15176971?v=3&s=40) repeat-x
	}
	
效果如下：

![alt](/images/bg-repeat-x.png)
>使用 **repeat-x** 只是会让背景图片沿容器 **x** 轴平铺，和 **repeat** 有点类似，有可能在容器最右侧不具备背景图片展示的空间

### repeat-y

**repeat-y** 和 **repeat-x** 类似，不同的是，**repeat-y** 让背景图片沿容器 **Y** 轴方向平铺背景图片。

	body{
		background:url(https://avatars2.githubusercontent.com/u/15176971?v=3&s=40) repeat-y
	}
	
效果如下：

![alt](/images/background-repeat属性详解/bg-repeat-y.png)
>同样的道理，如果使用repeat-y有可能造成容器底部没有足够的空间展示整个背景图片

### no-repeat

**no-repeat** 刚好和 **repeat** 相反，表示背景图片不做任何平铺，也就是说背景图片有多大，在容器显示出来的效果就有多大

	body{
		background:url(https://avatars2.githubusercontent.com/u/15176971?v=3&s=40) no-repeat
	}
	
效果如下：

![alt](/images/background-repeat属性详解/bg-no-repeat.png)
>使用no-repeat时也会有背景图片显示不全的情况，那就是当容器的尺寸小于背景图片尺寸的时候

## css3 新增属性----round 和 space

在CSS3中给 **background-repeat** 属性新增了两个属性，那就是 **round** 和 **space**

### round

背景图像自动缩放直到适应且填充满整个容器
![alt](/images/background-repeat属性详解/bg-round.png)

### space

背景图像以相同的间距平铺且填充满整个容器或某个方向--图片不进行缩放---两边的图像靠边
![alt](/images/background-repeat属性详解/bg-space.png)

## 不同的x和y轴的repeat值
从规范中，我们可以获得<repeat-style>可以有x和y的值，允许提供2个参数，如果提供全部2个参数，第1个用于横向，第二个用于纵向,如果只提供1个参数，则用于横向和纵向

比如:

* repeat 相当于repeat repeat
* repeat-x 相当于repeat no-repeat
* repeat-y 相当于no-repeat repeat
* space 相当于space space
* round相当于round round
* no-repeat相当于no-repeat no-repeat

>也就是说，在background-repeat取值是，可以将x和y的值任意组合，比如round space、space round、round repeat之类的


