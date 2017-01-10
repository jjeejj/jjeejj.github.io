---
title: node之日志库log4js
tags:
  - node
date: 2017-01-10 16:22:59
---


在`node`上工作的日志框架

<!--more-->

支持以下的特征:

* 彩色的控制台日志输出或错误
* 代替`node`的`console.log`方法(还可以添加配置项)
* `file appender`可以在基于文件大小或日期进行配置
* `SMTP appender`
* `Loggly appender`
* `Logstash UDP appender`
* `logFaces appender`
* `multiprocess appender`(当你用到工作进程会有用)
* 一个`logger`对象为了`connect/express`服务
* 配置日志信息的布局和方式
* 不同的日志栏目有着不同的日志级别(让你的应用日志一部分为`DEBUG`,一部分为`ERROR`)

在**1.0**之后的重要改变

默认的`appender`改为`console`去输出。当用`console`去输出日志可以减少内存的问题。
当你在浏览器使用`log4js`,你现在需要明确的配置使用`console appender`

`log4js`不在支持小于**0.12.X** `node`版本

## 安装

**npm install log4js**

## 使用

```js
const log4js = require('log4js');
var logger = log4js.getLogger();
logger.debug('Some debug messages')
```

你可以看到

[2017-01-10 12:52:02.700] [DEBUG] [default] - Some debug messages

再看一个完整的例子，不过只是代码片段

```js
var log4js = require('log4js');
//console log is loaded by default, so you won't normally need to do this
//log4js.loadAppender('console');
log4js.loadAppender('file');
//log4js.addAppender(log4js.appenders.console);
log4js.addAppender(log4js.appenders.file('logs/access.log'),'access');
var logger = log4js.getLogger('access');
logger.setLevel('ERROR');
logger.trace('Entering access testing');
logger.debug('Got access.');
logger.info('access is Gouda.');
logger.warn('access is quite smelly.');
logger.error('Cheese is too ripe!');
logger.fatal('access was breeding ground for listeria.');

===>结果

[2010-01-17 11:43:37.987] [ERROR] access - access is too ripe!
[2010-01-17 11:43:37.990] [FATAL] access - access was breeding ground for listeria.

```

上面前六行代码可以这样写
```js
var log4js = require('log4js');
log4js.configure({
  appenders: [
    { type: 'console' },
    { type: 'file', filename: 'logs/access.log', category: 'access' }
  ]
});
```

>以上为两种加载 appender 的方式

## 配置

你可以手动配置 `appender`和 日志级别(像上面)，或者提供一个配置`json`文件，或者一个对象。
你可以配置`log4js`定期检查配置文件的改变，如果有改变就重新加载，这允许改变日志级别在不重启应用的情况下

开启这个功能，指定一个时间：
```js
log4js.configure('file.json',{'reloadSecs':300})
```

对于`file appender`你可以指定一个 log 存放的文件夹

```js
log4js.configure('my_log4js_configuration.json', { cwd: '/absolute/path/to/log/dir' });
```

如果你已经指定一个绝对路径在配置文件中的`FileAppenders`中，你需要为`FileAppenders`添加一个`"absolute": true`配置项去覆盖`cwd`

比如：
```js
{
  "appenders": [
    {
      "type": "file",
      "filename": "relative/path/to/log_file.log",
      "maxLogSize": 20480,
      "backups": 3,
      "category": "relative-logger"
    },
    {
      "type": "file",
      "absolute": true,
      "filename": "/absolute/path/to/log_file.log",
      "maxLogSize": 20480,
      "backups": 10,
      "category": "absolute-logger"          
    }
  ]
}
```

## 一个简单的配置文件

```json
{
    "appenders":[
        {
            "type":"console",
            "category":"console"
        },
        {
            "type":"dateFile",
            "filename":"logs/access.log",
            "pattern":"-yyyy-MM-dd",
            "alwaysIncludePattern":false,
            "category": "dateFile"
        }
    ],
    "levels":{
        "dateFile":"DEBUG"
    },
    "replaceConsole":true
}
```

>说明：type：appender 的类型，category：每一个日志对象的唯一标识，filename：日志文件的名称，pattern：日志文件的后缀，有一定的格式要求
> alwaysIncludePattern: 日志文件名是否总总是加上 pattern 的后缀，默认是当天的不加的
> levels:日志级别，key 为上面日志对象的 category ,value:为日志级别（一共八个级别）

>[官方文档](https://github.com/nomiddlename/log4js-node/wiki)