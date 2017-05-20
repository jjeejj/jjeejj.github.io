---
title: node原始Buffer模块详解
tags: node
---
----------------------------------
在`ES6`引入`TypedArray`之前，`JavaScript`没有读取和操作二进制数据流的数据。`Buffer`类被引入作为 Node.js API 的一部分，使其可以在 TCP 流和文件系统操作等场景中处理二进制数据流

现在 `TypedArray` 已经被添加进 `ES6` 中，`Buffer` 类以一种更优与更适合 `Node.js` 用例的方式实现了 `Uint8Array` API

`Buffer` 类的实例类似于整数数组，除了其是***大小固定***的、且在 V8 ***堆外分配物理内存***。 `Buffer` 的大小在其创建时就已确定，且不能调整大小

<!--more-->

`Buffer` 类在 `Node.js` 中是一个**全局变量**，因此无需 `require('buffer').Buffer`
```js
//创建一个长度为10，且用 0 填充的 Buffer
const buf1 = Buffer.alloc(10);
// 创建一个长度为 10、且用 0x1 填充的 Buffer。 
const buf2 = Buffer.alloc(10, 1);
// 创建一个长度为 10、且未初始化的 Buffer。
// 这个方法比调用 Buffer.alloc() 更快，
// 但返回的 Buffer 实例可能包含旧数据，
// 因此需要使用 fill() 或 write() 重写。
const buf3 = Buffer.allocUnsaft(10);
// 创建一个包含 [0x1, 0x2, 0x3] 的 Buffer。
const buf4 = Buffer.from([1, 2, 3]);
// 创建一个包含 UTF-8 字节数组 [0x74, 0xc3, 0xa9, 0x73, 0x74] 的 Buffer。
const buf5 = Buffer.from('tést');
// 创建一个包含 Latin-1 字节数组 [0x74, 0xe9, 0x73, 0x74] 的 Buffer。
const buf6 = Buffer.from('tést', 'latin-1');
```

#### Buffer.from(),Buffer.alloc(),Buffer.allocUnsafe()
在 `Node.js v6` 之前的版本中，`Buffer` 实例是通过 `Buffer` 构造函数创建的，它根据提供的参数返回不同的 `Buffer`：
* 传一个数值作为第一个参数给 `Buffer()`,则分配一个指定大小的新建的 Buffer 对象。 分配给这种 Buffer 实例的`内存是没有初始化的，且可能包含敏感数据`