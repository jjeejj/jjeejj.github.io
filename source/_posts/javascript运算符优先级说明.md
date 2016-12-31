---
title: javascript运算符优先级说明
tags:
  - js
date: 2016-12-31 19:37:46
---


运算符的优先级决定了表达式中的运算执行的顺序，优先级高的运算符最先被执行

<!--more-->

一个简单的例子：

```js
3 + 4 * 5 //计算结果为23
```

乘法运算符 `*` 比 加号运算符 `+` 有着更高的优先级，所以它会被先执行

## 结合性

结合性决定相同优先级的运算符的执行顺序

`a OP b OP c`

左结合(从左向右计算)相当于把左边的子表达式加上小括号`(a OP b) OP c`,
类似的，右关联(从右到左计算)相当于a OP (b OP c)

赋值运算符是右关联的,所以你可以这么写:

`a = b = 5;`

结果 a 和 b 的值都会成为5。这是因为赋值运算符的返回结果就是赋值运算符右边的那个值
具体过程是：b被赋值为5，然后a也被赋值为 b=5 的返回值，也就是5

## 汇总表

下面的表将所有运算符按照优先级的不同**从高到低**排列

<table border="1" align="center">
	<th>优先级</th><th>运算符</th><th>关联性</th><th>运算符</th>
	<tr>
		<td>19</td>
		<td>圆括号</td>
		<td>n/a</td>
		<td>( … )</td>
	</tr>
	<tr>
		<td rowspan="3">18</td>
		<td>成员访问</td>
		<td>从左到右</td>
		<td>… . …</td>
	</tr>
	<tr>
		<td>需计算的成员访问</td>
		<td>从左到右</td>
		<td>… [ … ]</td>
	</tr>
	 <tr>
		<td>new(带参数列表)</td>
		<td>n/a</td>
		<td>new … ( … )</td>
	</tr>
	<tr>
		<td rowspan="2">17</td>
		<td>函数调用</td>
		<td>从左到右</td>
		<td>… ( … )</td>
	</tr>
	<tr>
		<td>new(无参数列表)</td>
		<td>从右到左</td>
		<td>new …</td>
	</tr>
	<tr>
		<td rowspan="2">16</td>
		<td>后置递增(运算符在后)</td>
		<td>n/a</td>
		<td>… ++</td>
	</tr>
	<tr>
		<td>后置递减(运算符在后)</td>
		<td>n/a</td>
		<td>… --</td>
	</tr>
	<tr>
		<td rowspan="9">15</td>
		<td>逻辑非</td>
		<td>从右到左</td>
		<td>! …</td>
	</tr>
	<tr>
		<td>按位非</td>
		<td>从右到左</td>
		<td>~ …</td>
	</tr>
	<tr>
		<td>一元加法</td>
		<td>从右到左</td>
		<td>+ …</td>
	</tr>
	<tr>
		<td>一元减法</td>
		<td>从右到左</td>
		<td>- …</td>
	</tr>
	 <tr>
		<td>前置递增</td>
		<td>从右到左</td>
		<td>++ …</td>
	</tr>
	<tr>
		<td>前置递减</td>
		<td>从右到左</td>
		<td>-- …</td>
	</tr>
	 <tr>
		<td>typeof</td>
		<td>从右到左</td>
		<td>typeof …</td>
	</tr>
	 <tr>
		<td>void</td>
		<td>从右到左</td>
		<td>void …</td>
	</tr>
	<tr>
		<td>delete</td>
		<td>从右到左</td>
		<td>delete …</td>
	</tr>
	<tr>
		<td rowspan="3">14</td>
		<td>乘法</td>
		<td>从左到右</td>
		<td>* … *</td>
	</tr>
	<tr>
		<td>除法</td>
		<td>从左到右</td>
		<td>… / …</td>
	</tr>
	<tr>
		<td>取模</td>
		<td>从左到右</td>
		<td>… % …</td>
	</tr>
	 <tr>
		<td rowspan="2">13</td>
		<td>加法</td>
		<td>从左到右</td>
		<td>… + …</td>
	</tr>
	<tr>
		<td>减法</td>
		<td>从左到右</td>
		<td>… - …</td>
	</tr>
	<tr>
		<td rowspan="3">12</td>
		<td>按位左移</td>
		<td>从左到右</td>
		<td>… << …</td>
	</tr>
	<tr>
		<td>按位右移</td>
		<td>从左到右</td>
		<td>… >> …</td>
	</tr>
	<tr>
		<td>无符号右移</td>
		<td>从左到右</td>
		<td>… >>> …</td>
	</tr>
	 <tr>
		<td rowspan="6">11</td>
		<td>小于</td>
		<td>从左到右</td>
		<td>… < …</td>
	</tr>
	<tr>
		<td>小于等于</td>
		<td>从左到右</td>
		<td>… <= …</td>
	</tr>
	<tr>
		<td>大于</td>
		<td>从左到右</td>
		<td>… > …</td>
	</tr>
	<tr>
		<td>大于等于</td>
		<td>从左到右</td>
		<td>… >= …</td>
	</tr>
	<tr>
		<td>in</td>
		<td>从左到右</td>
		<td>… in …</td>
	</tr>
	 <tr>
		<td>instanceof</td>
		<td>从左到右</td>
		<td>… instanceof …</td>
	</tr>
	<tr>
		<td rowspan="4">10</td>
		<td>等号</td>
		<td>从左到右</td>
		<td>… == …</td>
	</tr>
	<tr>
		<td>非等号</td>
		<td>从左到右</td>
		<td>… != …</td>
	</tr>
	<tr>
		<td>全等号</td>
		<td>从左到右</td>
		<td>… === …</td>
	</tr>
	<tr>
		<td>非全等号</td>
		<td>从左到右</td>
		<td>… !== …</td>
	</tr>
	<tr>
		<td>9</td>
		<td>按位与</td>
		<td>从左到右</td>
		<td>… & …</td>
	</tr>
	<tr>
		<td>8</td>
		<td>按位异或</td>
		<td>从左到右</td>
		<td>… ^ …</td>
	</tr>
	<tr>
		<td>7</td>
		<td>按位或</td>
		<td>从左到右</td>
		<td>… | …</td>
	</tr>
	 <tr>
		<td>6</td>
		<td>逻辑与</td>
		<td>从左到右</td>
		<td>… && …</td>
	</tr>
	<tr>
		<td>5</td>
		<td>逻辑或</td>
		<td>从左到右</td>
		<td>… || …</td>
	</tr>
	 <tr>
		<td>4</td>
		<td>条件运算符</td>
		<td>从右到左</td>
		<td>… ? … : …</td>
	</tr>
	<tr>
		<td rowspan="12">3</td>
		<td rowspan="12">赋值</td>
		<td rowspan="12">从右到左</td>
		<td>… = …</td>
	</tr>
	<tr>
		<td>… += …</td>
	</tr>
	<tr>
		<td>… -= …</td>
	</tr>
	<tr>
		<td>… *= …</td>
	</tr>
	<tr>
		<td>… /= …</td>
	</tr>
	<tr>
		<td>… %= …</td>
	</tr>
	<tr>
		<td>… <<= …</td>
	</tr>
	<tr>
		<td>… >>= …</td>
	</tr>
	<tr>
		<td>… >>>= …</td>
	</tr>
	<tr>
		<td>… &= …</td>
	</tr>
	<tr>
		<td>… ^= …</td>
	</tr>
	<tr>
		<td>… |= …</td>
	</tr>
	 <tr>
		<td rowspan="2">2</td>
		<td>yield</td>
		<td>从右到左</td>
		<td>yield …</td>
	</tr>
	<tr>
		<td>yield*</td>
		<td>从右到左</td>
		<td>yield* …</td>
	</tr>
	<tr>
		<td>1</td>
		<td>展开运算符</td>
		<td>n/a</td>
		<td>... …</td>
	</tr>
	<tr>
		<td>0</td>
		<td>逗号</td>
		<td>从左到右</td>
		<td>… , …</td>
	</tr>
</table>




