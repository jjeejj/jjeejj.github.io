---
title: 纯css制作各种图形
date: 2016-10-26 22:21:51
tags: [css]
description: 本文介绍 css 画各种不同图形的方法
---
----------------------------------------------

### Square (正方形)
![alt](/images/纯css制作各种图形/square.png)

	#square{
		width: 100px;
		height: 100px;
		background-color: red;
	}
-------------------------------
### Rectangle (矩形)
![alt](/images/纯css制作各种图形/rectangle.png)

	#rectangle{
		width: 200px;
		height: 100px;
		background-color: red;
    }
--------------------------------
### Circle (圆形)
![alt](/images/纯css制作各种图形/circle.png)

	#circle{
		width: 100px;
		height: 100px;
		background-color: red;
		-moz-border-radius:50%;
		-webkit-border-radius:50%;
		border-radius: 50%;
    }
------------------------------
### Oval (椭圆)
![alt](/images/纯css制作各种图形/oval.png)

	#oval{
		width: 200px;
		height: 100px;
		background-color: red;
		-moz-border-radius:50%;
		-webkit-border-radius:50%;
		border-radius:50%;
	}
>圆角大于等于50%就可以，其实这是W3C对于**[重合曲线](https://www.w3.org/TR/css3-background/#corner-overlap)**有这样的规范：如果两个相邻角的半径和超过了对应的盒子的边的长度，那么浏览器要重新计算保证它们不会重合

--------------------------------
### Triangle Up (向上的三角形)
![alt](/images/纯css制作各种图形/triangle-up.png)

	#triangle-up{
		width: 0;
		height: 0;
		border-bottom: 100px solid red;
		border-left: 50px solid transparent;
		border-right: 50px solid transparent;
	}
>边框的四个角会被该角相邻的连个边框线评分
>![alt](/images/纯css制作各种图形/border-triangle-split.png)

--------------------------------
### Triangle Down (向下的三角形)
![alt](/images/纯css制作各种图形/triangle-down.png)

	#triangle-down{
		width: 0;
		height: 0;
		border-left: 50px solid transparent;
		border-right: 50px solid transparent;
		border-top: 100px solid red;
	}
----------------------------------
### Triangle Left (向左的三角形)
![alt](/images/纯css制作各种图形/triangle-left.png)

	#triangle-down{
		width: 0;
		height: 0;
		border-right: 50px solid transparent;
		border-right: 50px solid transparent;
		border-top: 100px solid red;
	}
-------------------------------------------
### Triangle Right (向右的三角形)
![alt](/images/纯css制作各种图形/triangle-right.png)

	#triangle-down{
		width: 0;
		height: 0;
		border-left: 50px solid transparent;
		border-bottom: 50px solid transparent;
		border-top: 100px solid red;
	}
--------------------------------------------