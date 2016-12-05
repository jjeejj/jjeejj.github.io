---
title: 有关 File API的文档说明
tags:
  - File
  - HTML5
date: 2016-11-04 22:52:08
---


---------------------------------------------
`File` 接口提供了文件的信息，以及文件内容的存取方法。对象可以用来获取某个文件的信息,还可以用来读取这个文件的内容.通常情况下,
`File`对象是来自用户在一个`input`元素上选择文件后返回的`FileList`对象,也可以是来自由拖放操作生成的 `DataTransfer`对象。

<!--more-->

File API 包含哪些对象？

|对象|数据类型|用途|
|:---:|:------:|:----:|
|Blob|Blob|Blob 对象是包含有只读原始数据的类文件对象|
|File|File|File 接口提供了文件的信息，以及文件内容的存取方法。通常来自于下面FileList，一个FileLis对象伪数组通常包含有多个File|
|FileList|Object|用户选择的文件集合。通常来自于一个input元素的files属性|
|FileReader|Function|可以创建读取文件实例的构造函数。实例上有读取不同类型文件的方法和事件机制|

## Blob 对象

`Blob` 对象是包含有只读原始数据的类文件对象,`Blob` 对象中的数据并不一定得是`JavaScript` 中的原生形式.
`File` 接口基于 `Blob`，继承了 `Blob` 的功能,并且扩展支持用户计算机上的本地文件。
创建 `Blob` 对象的方法有几种，可以调用 `Blob()` 构造函数，还可以使用一个已有 `Blob` 对象上的 `slice()` 方法切出另一个 `Blob` 对象。
想要从用户的文件里获取一个 `Blob` 对象，请参考 `File` 对象。

### 构造函数

Blob() 构造函数返回一个新的 Blob 对象。 blob的内容由给定数组参数值的连结构成。

```js
Blob(blobParts[,options])
var aBlob = new Blob( array, options );
```

### 属性

`Blob.isClosed`
&nbsp;&nbsp;布尔值，指示`Blob.close()`是否在该对象调用过。关闭的Blob对象不可读

`Blob.size`
&nbsp;&nbsp;Blob 对象中所包含数据的大小(字节)

`Blob.type`
&nbsp;&nbsp;一个字符串，表明该Blob对象所包含的数据的MINE类型，如果类型未指定，则该值为空字符串

### 方法

`Blob.close()`
&nbsp;&nbsp;关闭 Blob 对象，以便能释放底层资源

`Blob.slice([start[,end[,contentType]]])`
&nbsp;&nbsp;返回一个新的 Blob 对象，包含了源 Blob 对象中指定范围内的数据



## FileList 对象

一个`FileList` **对象** 来自于一个`HTML input`元素 `files`属性，你可以通过这个对象访问到用户所选择的文件，
该类型的对象还有可能来自用户的拖放操作，查看 [DataTransfer](https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer)对象了解详情

### 使用

所有`type`属性为`file`的`input`的元素都有一个`files`属性，用来储存用户选择的文件
```html
<input id="filesItem" type="file" mutiple>
```
>mutiple:属性可以让用户能同时选择多个文件

如何获取到一个FileList对象中的第一个文件([File](https://developer.mozilla.org/zh-CN/docs/Web/API/File#Method_overview) 对象)
```js
var file = document.getElementById('fileItem').files[0];
```

### 属性

|属性名|类型|描述|
|:-----:|:---:|:----:|
|length|integer|一个只读的整数值,用来返回该FileList对象中的文件数量|

### 方法

`item()`
根据给定的索引值.返回FileList对象中对应的 `File`对象
```js
File item(
	index
);
```

**参数**
***index***
&nbsp;&nbsp;`File`对象在`FileList`对象中的索引值，从0开始.

**返回值**
请求返回`File`对象

## File 对象

`File`  **对象** 接口提供了文件的信息，以及文件内容的存取方法


## FileReader 对象

使用FileReader对象,web应用程序可以***异步***的读取存储在用户计算机上的文件(或者原始数据缓冲)内容,
可以使用File对象或者Blob对象来指定所要处理的文件或数据.
其中File对象可以是来自用户在一个`input`元素上选择文件后返回的FileList对象,
也可以来自拖放操作生成的 DataTransfer对象,还可以是来自在一个HTMLCanvasElement上执行mozGetAsFile()方法后的返回结果.

创建一个`FileReader` 对象，如下：
```js
var reader = new FileReader();
```

### 读取文件内容状态

|状态常量|值|描述|
|:------:|:---:|:----:|
|EMPTY|0|还没有加载数据|
|LOADING|1|数据正在加载|
|DONE|2|已完成全部读取请求|

### 属性

|属性名|类型|描述|
|:------:|:---:|:----:|
|error|DOMError|在读取文件时发生的错误. **只读**|
|readyState|unsigned short|表明FileReader对象的当前状态. 值为上状态中的一个 **只读**|
|result|jsval|读取到的文件内容.这个属性只在读取操作完成之后才有效,并且数据的格式取决于读取操作是由哪个方法发起的 **只读**|

### 方法

**abort()**
中止读取操作，在返回时`readyState`的值为DONE
```js
void abort()
```
**参数**：无
**抛出的异常**
>     当该FileReader对象没有在进行读取操作时(也就是readyState属性的值不为LOADING时),调用abort()方法会抛出`DOM_FILE_ABORT_ERR`异常.

**readAsArrayBuffer()**

开始读取指定的`Blob`对象或`File`对象中的内容. 当读取操作完成时,`readyState`属性的值会成为`DONE`,
如果设置了`onloadend`事件处理程序,则调用之.
同时,`result`属性中将包含一个`ArrayBuffer`对象以表示所读取文件的内容.

```js
void readAsArrayBuffer(
	in Blob blob
)
```
**参数**
&nbsp;&nbsp;将要读取到一个`ArrayBuffer`中的`Blob`对象或者`File`对象.

**readAsBinaryString()**

开始读取指定的`Blob`对象或`File`对象中的内容. 当读取操作完成时,`readyState`属性的值会成为`DONE`,
如果设置了`onloadend`事件处理程序,则调用之.
同时,`result`属性中将包含所读取文件的原始二进制数据.

```js
void readAsBinaryString(
	in Blob blob
)
```
**参数**
&nbsp;&nbsp;将要读取到一个`ArrayBuffer`中的`Blob`对象或者`File`对象.

**readAsDataURL()**

开始读取指定的`Blob`对象或`File`对象中的内容. 当读取操作完成时,`readyState`属性的值会成为`DONE`,
如果设置了`onloadend`事件处理程序,则调用之.
result属性中将包含一个data: URL格式的字符串以表示所读取文件的内容.

```js
void readAsDataURL(
	in Blob blob
)
```
**参数**
&nbsp;&nbsp;将要读取到一个`ArrayBuffer`中的`Blob`对象或者`File`对象.

>这个方法很有用,比如,可以实现图片的本地预览

**readAsText()**

开始读取指定的`Blob`对象或`File`对象中的内容. 当读取操作完成时,`readyState`属性的值会成为`DONE`,
如果设置了`onloadend`事件处理程序,则调用之.
同时,result属性中将包含一个字符串以表示所读取的文件内容.

```js
void readAsBinaryString(
	in Blob blob,
	in DOMString encoding 可选
)
```
**参数**
*blob*
&nbsp;&nbsp;将要读取到一个`ArrayBuffer`中的`Blob`对象或者`File`对象.
*encoding* 可选
&nbsp;&nbsp;一个字符串,表示了返回数据所使用的编码.如果不指定,默认为UTF-8.

### 事件

**onabort**
&nbsp;&nbsp;当读取操作被中止时调用

**onerror**
&nbsp;&nbsp;当读取操作发生错误时调用

**onload**
&nbsp;&nbsp;当读取操作成功完成时调用

**onloadend**
&nbsp;&nbsp;当读取操作完成时调用,不管是成功还是失败.该处理程序在onload或者onerror之后调用

**onloadstart**
&nbsp;&nbsp;当读取操作将要开始之前调用

**onprogress**
&nbsp;&nbsp;在读取数据过程中周期性调用

## 四大对象的关系

![alt](/images/FileAPI的文档说明/File四大接口对象的关系.png)

--------------------------------------------------------------
## 补充

### 美化 文件输入框并显示文件名
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        .file{
            position: relative;
            background-color: #D0EEFF;
            border: 1px solid #99D3F5;
            border-radius: 4px;
            padding: 4px 12px;
            text-decoration: none;
            line-height: 20px;
            color: #1E88C7;
            overflow: hidden;
            margin-right: 2px;

        }
        .file input{
            position: absolute;
            top: 0;
            left: 0;
            opacity: 0;
           /* font-size: 100px;*/
        }
        a:hover{
            cursor: pointer;
            background-color:yellow;
        }
    </style>
</head>
<body>
    <a href="#" class="file">选择文件
        <input type="file" id="fileItem" multiple>
    </a>
     <span id="filename">文件名</span>

    <script src="//cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <!-- <script src="../js/vendor/jquery.min.js"></script> -->

     <script>
         $('#fileItem').on('change',function () {
             var fileList = $(this).prop('files');
             console.log('================',fileList);
             if(fileList){

               var fileName = Array.prototype.map.call(fileList,function (value,index,array) {
                    return value.name;
                })

                $('#filename').text(fileName.join(';'))
             }
         })
     </script>
</body>
</html>
```

效果：
![alt](/images/FileAPI的文档说明/file-input.png)

