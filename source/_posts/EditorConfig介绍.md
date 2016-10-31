---
title: 'EditorConfig介绍'
date: 2016-10-31 15:51:14
tags:
---

## 前言

EditorConfig是一套用于统一代码格式的解决方案，很多项目都有用到，比如`Bootstrap、jQuery、Underscore`和`Ruby`等等

<!--more-->

## EditorConfig是什么？

EditorConfig可以帮助开发者在不同的编辑器和IDE之间定义和维护一直的代码风格。EditorConfig包含一个用于定义代码风格的文件和一批编辑器插件，
这些插件可以让编辑器读取该配置文件并依此格式化代码。EditorConfig配置文件十分易读，能很好地在版本控制系统(version control system)下工作。

## EditorConfig文件是什么样的？

以下的.editorconfig文件是用于设置Python和Javascritp行尾和缩进风格的

```editorconfig
# Editorconfig is awesome:http://editorconfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# Matches multiple files with brace expansion notation
# Set default charset
[*.{js,py}]
charset = utf-8

# 4 space indentation for py files
[*.py]
indent_style = space
indent_size = 4

# tab indentation(no size specified)
[Makefile]
indent_style = tab

# indentation override for all js under lib directory
[lib/**.js]
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]	
indent_style = space
indent_size = 2

```

## EditorConfig存放的位置？
当打开一个文件时，EditorConfig插件会在打开文件的目录和其每一级父目录查找.editorconfig文件，直到有一个配置文件`root=true`。

EditorConfig配置文件是***从上往下**读取，并且距离打开文件***路径最近的文件最后被读取***。匹配的配置属性按照属性应用在代码上，所以***最接近代码文件的属性优先级最高***。

>window 用户，创建.editorconfig文件，可以先创建.editorconfig.文件，系统会自动重名为.editorconfig。

## 文件格式详情

EditorConfig文件使用[INI格式](https://zh.wikipedia.org/wiki/INI%E6%96%87%E4%BB%B6),但是允许在分段名（译注：原文是section names）中使用“and”
分段名是全局的文件路径，格式类似于gitignore。斜杠(/)作为路径分隔符，#或者;作为注释。注释应该单独占一行。EditorConfig文件使用UTF-8格式、CRLF或LF作为换行符。

**通配符**

|通配符|说明|
|:------:|:----:|
|*|匹配任意字符，除了路径分隔符'/'|
|**|匹配任意字符|
|?|匹配单个字符|
|[name]|匹配name的字符|
|[!name]|匹配非name的字符|
|[{s1,s3,s3}]|匹配任意给定的字符串（0.11.0起支持)|
|{num1..num2}|匹配数字num1和数字num2之间的任意数字，num1和num2的大小顺序没有规定|

>特殊字符可以用\转义，以使其不被认为是通配符

**支持的属性**
>不是每种插件都支持所有的属性，具体可见[WiKi](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties)

* `indent_style`:`tab`为hard-tabs,`space`为soft-tabs
* `indent_size`: 设置为整数表示规定每级缩进的列数和soft-tabs的宽度(空格数)，如果设定为tab，则会使用tab-width的值（如果有设定）
* `tab_width`: 设置整数用于指定替代tab的列数,默认值就是indent_size的值，一般无需指定
* `end_of_line`: 定义换行符，支持lf、cr和crlf。
* `charser`: 编码格式，支持latin1、utf-8、utf-8-bom、utf-16be和utf-16le，不建议使用uft-8-bom。
* `trim_trailing_whitespace`: 设为true表示会除去换行行首的任意空白字符，false反之。
* `insert_final_newline`: 设为true表明使文件以一个空白行结尾，false反之。
* `root`: 表明是最顶层的配置文件，发现设为true时，才会停止查找.editorconfig文件。

>目前所有的属性名和属性值都是大小写不敏感的。编译时都会将其转为小写。通常，如果没有明确指定某个属性，则会使用编辑器的配置，而EditorConfig不会处理。
>推荐不要指定某些EditorConfig属性。比如，tab_width不需要特别指定，除非它与indent_size不同。同样的，当indent_style设为tab时，不需要配置indent_size，这样才方便阅读者使用他们习惯的缩进格式。另外，如果某些属性并没有规范化（比如end_of_line），就最好不要设置它。


## 插件下载

查看官网，[下载插件](http://editorconfig.org/)