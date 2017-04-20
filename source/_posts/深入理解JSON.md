---
title: 深入理解JSON
tags:
  - js
date: 2017-04-20 15:28:07
---


--------------------------

在理解之前，看一下下面JS对象通过`JSON.stringify()`后的字符串是什么？

<!--more-->
```js
var friend={
    firstName: 'Good',
    'lastName': 'Man',
    'address': undefined,
    'phone': ["1234567",undefined],
    'fullName': function(){
        return this.firstName + ' ' + this.lastName;
    }
};
JSON.stringify(friend);
```

在以上的基础上，怎么把`friend`的姓和名变为大些应该怎么处理？

### JSON的定义

定义：`JSON(JavaScript Object Notation)` 是一种**轻量级**的**数据交换格式**,`JSON`采用完全独立于语言的**文本格式**

#### 一种数据格式

什么是格式？就是规范你的数据要怎么表示，比如：有个人叫“二百六”，身高“160cm”，体重“60kg”，现在你要将这个人的这些信息传给别人或者别的什么东西，你有很多种选择：

1. 姓名“二百六”，身高“160cm”，体重“60kg”
2. name="二百六"&height="160cm"&weight="60kg"
3. <person><name>二百六</name><height>160</height><weight>60</weight></person>
4. {"name":"二百六","height":160,"weight":60}

#### 基于文本的数据格式

JSON是基于文本的数据格式，相对于基于二进制的数据,我们常会称为“JSON字符串”

#### 轻量级的数据格式

在JSON之前，有一个数据格式叫xml,，但是JSON更加轻量，如xml需要用到很多标签.你可以明显看到xml格式的数据中标签本身占据了很多空间，而JSON比较轻量，即相同数据，以JSON的格式占据的带宽更小，这在有大量数据请求和传递的情况下是有明显优势的

#### 被广泛地用于数据交换

轻量已经是一个用于数据交换的优势了，但更重要的JSON是易于阅读、编写和机器解析的，即这个JSON对人和机器都是友好的，而且又轻，独立于语言（因为是基于文本的），所以JSON被广泛用于数据交换

### JSON和JS对象之间的关系

####  两个本质不同的东西为什么那么密切

JSON和JS对象本质上完全不是同一个东西，`JSON`的全名为`JavaScript Object Notation`,所以他的语法是基于`JS`的，但它就是一种格式，而`JS`对象是一个实例，存在于内存当中。

此外`JSON`是可以传输的，因为它是文本格式的，但`JS`对象是不可以传输的。在语法上`JSON`更加严格，但是JS对象就很松了

#### JSON格式别JS对象语法表现上严格在哪

|对比内容|JSON|JS对象|
|:----:|:----:|:----:|
|键名|必须加双引号|可允许不加、加单引号、加双引号|
|属性值|只能是数值（10进制)、字符串（双引号）、true、false、 null、对象（object）或者数组（array）,不能是函数、NaN, Infinity, -Infinity和undefined|任意|
|逗号问题|最后一个属性后面不能有逗号|最后一个属性后面有没有逗号都可以|
|数值|前导0不能用，小数点后必须有数字|没限制|

#### JSON不是JS的子集

看看一下代码：
```js
var code = '"\u2028\u2029"';
JSON.parse(code); // works fine
eval(code); // fails
```

JSON.parse可以正常解析，但是当做js解析时会报错。

### JS中的JSON函数

#### 将JS数据结构转换为JSON字符串，JSON.stringify

语法：`JSON.stringify(value[, replacer [, space]])`

##### 基本使用--一个参数

传入一个`JSON`格式的`JS`对象或者数组：
```js
JSON.stringify({"name":"Good Man","age":18})
//'{"name":"Good Man","age":18}'

//数组
JSON.stringify([1,2,3,"4"])
//'[1,2,3,"4"]'
```

##### 第二个参数是一个函数或者是一个数组或者null或者未提供

* 如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理
```js
var friend={
    "firstName": "Good",
    "lastName": "Man",
    "phone":"1234567",
    "age":18
};
var friendAfter = JSON.stringify(friend,function(key,value){
    if(key === 'phone'){
        return "(000)"+value;
    }else if(typeof value === 'number'){
        return 10+value;
    }else{
        return value; //如果你把这个else分句删除，那么结果会是undefined
    }
});
console.log(friendAfter);
//{"firstName":"Good","lastName":"Man","phone":"(000)1234567","age":28}
```
>如果上面的传入的不是键值对的对象形式，而是数组形式的，比如上面传入的是`var friend = ["Jack","Rose"]`,如果是数组形式，那么key是索引值，而value是这个数组项,记得要在函数内部返回value，不然会出错
* 如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中,而这个数组中存在但是源JS对象中不存在的属性会被忽略，不会报错
```js
var friend={
    "firstName": "Good",
    "lastName": "Man",
    "phone":"1234567",
    "age":18,
    "phone":{"home":"1234567","work":"7654321"}
};
var friendAfter=JSON.stringify(friend,["firstName","address","phone"]);
console.log(friendAfter);
//{"firstName":"Good","phone":"1234567","phone":{}} 
//由于 phone 对象的属性不在数组中，所以没有序列化出来
```
* 如果该参数为null或者未提供，则对象所有的属性都会被序列化

##### 第三个参数用于美化输出 —— 不建议用

指定缩进用的空白字符，可以取以下几个值：

* 是1-10的某个数字，代表用几个空白字符
* 是字符串的话，就用该字符串代替空格，最多取这个字符串的前10个字符
* 没有提供该参数 等于 设置成null 等于 设置一个小于1的数

示例代码：
```js
var friend={
    "firstName": "Good",
    "lastName": "Man",
    "phone":{"home":"1234567","work":"7654321"}
};
//直接转换
//{"firstName":"Good","lastName":"Man","phone":{"home":"1234567","work":"7654321"}}

var friendAfter=JSON.stringify(friend,null,4);
console.log(friendAfter);
/*
{
    "firstName": "Good",
    "lastName": "Man",
    "phone": {
        "home": "1234567",
        "work": "7654321"
    }
}
*/
var friendAfter=JSON.stringify(friend,null,"HAHAHAHA");
console.log(friendAfter);
/*
{
HAHAHAHA"firstName": "Good",
HAHAHAHA"lastName": "Man",
HAHAHAHA"phone": {
HAHAHAHAHAHAHAHA"home": "1234567",
HAHAHAHAHAHAHAHA"work": "7654321"
HAHAHAHA},
HAHAHAHA"age": 18
}
*/
var friendAfter=JSON.stringify(friend,null,"WhatAreYouDoingNow");
console.log(friendAfter);
/*
{
WhatAreYou"firstName": "Good",
WhatAreYou"lastName": "Man",
WhatAreYou"phone": {
WhatAreYouWhatAreYou"home": "1234567",
WhatAreYouWhatAreYou"work": "7654321"
WhatAreYou},
WhatAreYou"age": 18
}
*/
```

>键名不是双引号的（包括没有引号或者是单引号），会自动变成双引号；字符串是单引号的，会自动变成双引号
>最后一个属性后面有逗号的，会被自动去掉
>布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值
>NaN、Infinity和-Infinity，不论在数组还是非数组的对象的属性值中，都被转化为null
>所有以 symbol 为属性键的属性都会被完全忽略掉，即便 replacer 参数中强制指定包含了它们
>不可枚举的属性会被忽略
>非数组对象的属性不能保证以特定的顺序出现在序列化后的字符串中,即：非数组对象在最终字符串中不保证属性顺序和原来一致
>undefined、任意的函数（其实有个函数会发生神奇的事，后面会说）以及 symbol 值,1⃣️：出现在非数组对象的属性值中：在序列化过程中会被忽略，2⃣️：出现在数组中时：被转换成 null

#### 将JSON字符串解析为JS数据结构 —— JSON.parse

语法：`JSON.parse(text[, reviver]))`

如果第一个参数不是合法的`JSON`字符串，那么就会抛出异常，合法指这个JSON字符串完全符合JSON要求的严格格式。

可选的第二个参数，必须是函数，作用在属性已经被解析但是还没返回前，将属性处理后再返回。

```js
var friend={
    "firstName": "Good",
    "lastName": "Man",
    "phone":{"home":"1234567","work":["7654321","999000"]}
};
var friendAfter=JSON.stringify(friend);
//'{"firstName":"Good","lastName":"Man","phone":{"home":"1234567","work":["7654321","999000"]}}'

//不带第二个参数的解析结果
var friendObjOne = JSON.parse(friendAfter);
console.log(friendObjOne);

/**
{ firstName: 'Good',
  lastName: 'Man',
  phone: { home: '1234567', work: [ '7654321', '999000' ] } }

*/
//再将其解析出来，在第二个参数的函数中打印出key和value
var friendObj = JSON.parse(friendAfter,function(k,v){
    console.log(k);
    console.log(v);
    console.log("----");
});
//这个可以看出解析的顺序
/*
firstName
Good
----
lastName
Man
----
home
1234567
----
0
7654321
----
1
999000
----
work
[ ,  ]
----
phone
{}

{}
----
*/
```
这个遍历是由内而外的,这个由内而外指的是对于**复合属性**来说的，通俗地讲，遍历的时候，从头到尾进行遍历，如果是简单属性值（数值、字符串、布尔值和null），那么直接遍历完成.
如果是遇到属性值是对象或者数组形式的，那么暂停，先遍历这个子JSON,而遍历的原则也是一样的，等这个复合属性遍历完成，那么再完成对这个属性的遍历返回

>如果`reviver`返回`undefined`,那么则当前属性会从所属对象中删除，如果返回了其他值，则返回的值会成为当前属性新的属性值
>注意上面输出的，最后一个看上去没有`key`,其实是一个空字符串。到了解析最上层了，所有没有真正的属性了

####  影响 JSON.stringify 的神奇函数 —— object.toJSON

如果你在`js`对象上实现了一个`toSJON`方法，那么使用 `JSON.stringify`去序列化这个`js`对象.
`JSON.stringify`会把这个对象的toJSON方法返回的值作为参数去进行序列化

```js
var info={
    "msg":"I Love You",
    "toJSON":function(){
        var replaceMsg=new Object();
        replaceMsg["msg1"]="Go Die";
        return replaceMsg;
    }
};
console.log(JSON.stringify(info));
//{"msg1":"Go Die"}
```
>其实Date类型可以直接传给JSON.stringify做参数，其中的道理就是，Date类型内置了toJSON方法
```js
console.log(JSON.stringify(new Date()));
//2017-04-20T07:23:12.862Z"
```





