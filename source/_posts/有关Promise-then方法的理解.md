---
title: 有关Promise.then方法的理解
tags:
  - js
date: 2017-11-05 13:23:40
---

--------------------------------------------

`then`方法是`Promise`原型上一个方法,返回一个新的`Promise`,它最多需要两个参数:分别对应成功和失败的请求。

-------------------------------------------
<!--more-->

#### 语法

```js
p.then(onFulfilld,onRejected);

p.then(function(value){
    //fulfillment
},function(reason){
    //rejection
})
```

#### 参数

* `onFulfilld`: 当`Promise`变成接受状态（fulfillment）时，该参数作为回调函数被调用。该函数有一个参数，即接受的值（the fulfillment  value）
* `onRejected`: 当`Promise`变成拒绝状态（rejection ）时，该参数作为回调函数被调用。该函数有一个参数,，即拒绝的原因（the rejection reason）

>当参数`onFulfilld`或者`onRejected`的类型不是`Function`时，那么就会丢弃对应状态函数的回调信息，但是不会报错，会创建一个没有经过回调函数创建的新的`Promise`,这个新的`Promise`只是简单接受调用`then`方法的原`Promise`的终态作为它的终态。对应下面的例子：
```js
var p = new Promise(function(resolve,reject){
    resolve('hello');
});

p.then('world').then(function(data){
    console.log(data) //hello
})

//或者

Promise.resolve('First Value').then(Promise.resolve('Second Value')).then(null).then((value) => {
    console.log(value)    // First Value
})
```

#### 返回值

该方法返回一个新的`Promise`,该返回值与`then`方法中的回调函数的返回值有关

1. 如果返回的是一个值，那么返回的`Promise`将是接受状态(**resolve**),而且将返回的值作为接受状态的回调函数的参数值(**resolve(returnData)**)
```js
p.then(function(data){
    return data;
})
```
2. 如果`then`中的回调函数抛出一个错误，那么返回的`Promise`将会成为拒绝状态(**reject**)，并且将抛出的错误作为拒绝状态的回调函数的参数值(**reject(err)**)
```js
p.then(function(data){
    throw 42;
})
```
3. 显示返回一个对应接受或拒绝状态的`Promise`
```js
p.then(function(data){
    return Promise.resolve('42')
})
```
4. 返回一个未定状态（pending）的Promise，那么then返回Promise的状态也是未定的，并且它的终态与那个Promise的终态相同
```js
p.then(function(data){
    return new Promise(function(resolve,reject){
        if(????){
            resolve(true);
        }else{
            reject(false);
        }
    })
})
```