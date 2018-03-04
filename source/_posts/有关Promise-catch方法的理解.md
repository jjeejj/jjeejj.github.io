---
title: 有关Promise.catch方法的理解
tags:
  - js
date: 2018-03-04 13:50:13
---

----------------------------------------------

之前写了一篇有关`Promise`的`then`[方法的文章](https://mp.weixin.qq.com/s/PnoRPIgr1xYzOqLPNpqK5g),现在在讲解一下有关`Promise`原型上`catch`的方法

---------------------------------------------

<!--more-->
`catch`方法是为了处理拒绝的情况或者抛出异常的情况

#### 语法

```js
p.catch(onRejected);

p.catch(function(reason){
    // 拒绝
});
```
##### 参数

* `onRejected`:当`Promise` 被拒绝时,被调用的一个`Function`,该函数拥有一个参数
* `reson`: 拒绝的原因

##### 返回值

一个新的`Promise`

#### 示例代码

##### 返回的Promise行为状态

```js
var p1 = new Promise(function(resolve, reject) {
  resolve('Success');
});
p1.then(function(value) {
  console.log(value); // "Success!"
  throw 'oh, no!';
}).catch(function(e) {
  console.log(`e`,e); // "e oh, no!"
  //或者
  //Promise.resolve();
}).then(function(value){
    console.log('after a catch the chain is restored',value);
}, function (err) {
  console.log('Not fired due to the catch',err);
});
上面的打印结果为:
Success
e oh, no!
after a catch the chain is restored undefined
```

```js
var p1 = new Promise(function(resolve, reject) {
  resolve('Success');
});
p1.then(function(value) {
  console.log(value); // "Success!"
  throw 'oh, no!';
}).catch(function(e) {
  console.log(`e`,e); // "e oh, no!"
  //或者
  return Promise.resolve();
}).then(function(value){
    console.log('after a catch the chain is restored',value);
}, function (err) {
  console.log('Not fired due to the catch',err);
});
上面的打印结果为:
Success
e oh, no!
after a catch the chain is restored undefined
```

若把上面的代码改为以下这样:

```js
var p1 = new Promise(function(resolve, reject) {
  resolve('Success');
});
p1.then(function(value) {
  console.log(value); // "Success!"
  throw 'oh, no!';
}).catch(function(e) {
  console.log(`e`,e); // "e oh, no!"
  //或者
  return Promise.reject("ssssss");
}).then(function(value){
    console.log('after a catch the chain is restored',value);
}, function (err) {
  console.log('Not fired due to the catch',err);
});
上面的打印结果为:
Success
e oh, no!
Not fired due to the catch ssssss
```

```js
var p1 = new Promise(function(resolve, reject) {
  resolve('Success');
});
p1.then(function(value) {
  console.log(value); // "Success!"
  throw 'oh, no!';
}).catch(function(e) {
  console.log(`e`,e); // "e oh, no!"
  throw "ssssss"
}).then(function(value){
    console.log('after a catch the chain is restored',value);
}, function (err) {
  console.log('Not fired due to the catch',err);
});

上面的打印结果为:
Success
e oh, no!
Not fired due to the catch ssssss
```

在上面的代码中可以看出`catch`返回的`Promise`状态和`then`方法中的行为是一致的,具体请参考[then](https://mp.weixin.qq.com/s/PnoRPIgr1xYzOqLPNpqK5g)

**抛出一个错误或返回一个失败的 `Promise`,`Promise` 通过 catch() 返回失败onRejected状态的结果;否则,它将返回成功的`onFulfilld`状态的数据**

##### 异常捕获的行为

```js
// 抛出一个错误，大多数时候将调用catch方法
var p1 = new Promise(function(resolve, reject) {
  throw 'Uh-oh!';
});

p1.catch(function(e) {
  console.log(e); // "Uh-oh!"
});

// 在异步函数中抛出的错误不会被catch捕获到
var p2 = new Promise(function(resolve, reject) {
  setTimeout(function() {
    throw 'Uncaught Exception!';
  }, 1000);
});

p2.catch(function(e) {
  console.log(e); // 不会执行
});

// 在resolve()后面抛出的错误会被忽略
var p3 = new Promise(function(resolve, reject) {
  resolve();
  throw 'Silenced Exception!';
});

p3.catch(function(e) {
   console.log(e); // 不会执行
});
```

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**