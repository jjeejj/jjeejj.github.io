---
title: yaml语法的学习
tags:
  - js
date: 2017-05-06 13:43:26
---

-------------------------------------------
`YAML`不是标记语言。

`YAML`是一种对所有编程语言友好的数据序列化的标准。

`YAML`的官网 [http://www.yaml.org/](http://www.yaml.org/),对每种编程语言都有特定的实现。本文以`JS-YAML`为例。

`YAML JavaScript parser` 在线转换地址[online](http://nodeca.github.io/js-yaml/)

<!--more-->
#### 语法

语法规则：
* 大小写敏感
* 使用缩进表示层级关系
* 缩进时不允许使用Tab键，只允许使用空格
* 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
* 不能有重复的`key`

>`#`表示注释，从这个字符一直到行尾，都会被解析器忽略。

支持的数据结构：
* 对象：键值对集合，又称映射(mapping)／哈希(hashes)／字典(dictionary)
* 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
* 纯量（scalars）：单个的、不可再分的值

#### 数据结构

##### 对象
对象的一组键值对，使用冒号结构表示
```yaml
amimal: pets
```
转为 JavaScript 如下
```js
{ amimal: 'pets' }
```
也支持另一种写法，将所有键值对写成一个***行内对象***
```yaml
hash: { name: Steve, foo: bar } 
```
转为 JavaScript 如下
```js
{ hash: { name: 'Steve', foo: 'bar' } }
```
>注意：`:`和值之间有一个空格，否则不能识别尾一个对象，会识别尾一个字符串

##### 数组

一组连词线开头的行，构成一个数组
```yaml
- 收拾
- 时尚
```
转为 JavaScript 如下
```js
[ '收拾', '时尚' ]
```
数据结构的子成员是一个数组，则在该项目下缩进一个空格
```yaml
-
 - 收拾
- 时尚
```
转为 JavaScript 如下
```js
[ [ '收拾' ], '时尚' ]
```
数组也可以采用行内表示
```yaml
arr: [ss,sss]
```
转为 JavaScript 如下
```js
{ arr: [ 'ss', 'sss' ] }
```
>注意：`-`和值之间有一个空格

##### 复合结构

对象和数组可以结合使用，形成复合结构
```yaml
languages:
 - Ruby
 - Perl
 - Python 
websites:
 YAML: yaml.org 
 Ruby: ruby-lang.org 
 Python: python.org 
 Perl: use.perl.org 
```
转为 JavaScript 如下
```js
{ languages: [ 'Ruby', 'Perl', 'Python' ],
  websites: 
   { YAML: 'yaml.org',
     Ruby: 'ruby-lang.org',
     Python: 'python.org',
     Perl: 'use.perl.org' } }
```
##### 纯量
纯量是最基本的、不可再分的值。以下数据类型都属于 JavaScript 的纯量：
* 字符串
* 布尔值
* 整数
* 浮点数
* Null
* 时间
* 日期

```yaml
str: '1111'
number: 12
isFalse: true
parent: Null
parent1: ~
date: 2017-02-09
time: 2017-02-09 12:23:34
snumber: !!str 11
sboo: !!str true
```
转为 JavaScript 如下
```js
{ str: '1111',
  number: 12,
  isFalse: true,
  parent: null,
  parent1: null,
  date: Thu Feb 09 2017 08:00:00 GMT+0800 (CST),
  time: Thu Feb 09 2017 20:23:34 GMT+0800 (CST),
  snumber: '11',
  sboo: 'true' }
```
>说明：
>允许使用两个感叹号，强制转换数据类型；
>null用~表示或Null表示；

##### 字符串
字符串是最常见，也是最复杂的一种数据类型

字符串默认不使用引号表示：
```yaml
string: 这是一行字符串
```
转为 JavaScript 如下
```js
{ str: '这是一行字符串' }
```
如果字符串之中包含空格或特殊字符，需要放在引号之中
```yaml
str: '内容： 字符串'
```
转为 JavaScript 如下
```js
{ str: '内容： 字符串' }
```
单引号和双引号都可以使用，双引号不会对特殊字符转义
```yaml
s1: '内容\n字符串'
s2: "内容\n字符串"
```
转为 JavaScript 如下
```js
{ s1: '内容\\n字符串', s2: '内容\n字符串' }
```
单引号之中如果还有单引号，必须连续使用两个单引号转义
```yaml
str: 'labor''s day' 
```
转为 JavaScript 如下
```js
{ str: 'labor\'s day' }
```
字符串可以写成多行，从第二行开始，必须有一个单空格缩进。换行符会被转为空格
```yaml
str: aaa
 sdsd
 dsdsdsd
```
转为 JavaScript 如下
```js
{ str: 'aaa sdsd dsdsdsd' }
```
多行字符串可以使用|保留换行符，也可以使用>折叠换行
```yaml
str: |
 sdsd
 dsdsdsd
str1: >
 sdsd
 dsdsdsd
```
转为 JavaScript 如下
```js
{ str: 'sdsd\ndsdsdsd\n', str1: 'sdsd dsdsdsd\n' }
```
`+`表示保留文字块末尾的换行，`-`表示删除字符串末尾的换行
```yaml
s1: |
  Foo

s2: |+
  Foo


s3: |-
  Foo
```
转为 JavaScript 如下
```js
{ s1: 'Foo\n', s2: 'Foo\n\n\n', s3: 'Foo' }
```
字符串之中可以插入 HTML 标记
```yaml
message: |

  <p style="color: red">
    段落
  </p>
```
转为 JavaScript 如下
```js
 message: '\n<p style="color: red">\n  段落\n</p>\n' }
```
#### 引用

锚点`&`和别名`*`，可以用来引用
```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
```
转为 JavaScript 如下
```js
{ defaults: { adapter: 'postgres', host: 'localhost' },
  development: 
   { database: 'myapp_development',
     adapter: 'postgres',
     host: 'localhost' },
  test: 
   { database: 'myapp_test',
     adapter: 'postgres',
     host: 'localhost' } 
}
```
`&`用来建立锚点（defaults），`<<`表示合并到当前数据，`*`用来引用锚点。

```yaml
- &showell Steve 
- Clark 
- Brian 
- Oren 
- *showell
```
转为 JavaScript 如下
```js
[ 'Steve', 'Clark', 'Brian', 'Oren', 'Steve' ]
```
#### 函数和正则表达式的转换

这是 `JS-YAML` 库特有的功能，可以把函数和正则表达式转为字符串
```yaml
# example.yml
fn: function () { return 1 }
reg: /test/
```
转为 JavaScript 如下
```js
{ fn: 'function () { return 1 }', reg: '/test/' }
```