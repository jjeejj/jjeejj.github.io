---
title: vscode调试nodejs的配置介绍
tags:
  - node
  - other
date: 2017-07-08 16:13:10
---

------------------------------

`vscode` 调试 `nodejs`的项目 

[官方文档在这里](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

下面对该文档进行简单的翻译

-------------------------------
<!--more-->

#### vscode nodejs调试介绍

##### 被支持类似 Node 的运行环境
由于 `VScode`内置的`Nodejs`调试器是通过 `wire protocols ` 与`Nodejs`运行环境进行通信，所支持的 `wire protocols ` 是在 运行时确定的

现在有两种 `wire protocols ` 存在的
* legacy ： 原来的 `V8`的调试协议，现被所有的运行时所支持，但是有可能在 `Nodejs` v8.x 中丢弃
* inspector : 新的`V8`的检查协议，被使用在`Node.js versions >= 6.3`通过一个标志位`--inspect`

目前这两种协议，被以下版本所支持的情况
|Runtime|Legacy Protocol|Inspector Protocol|
|:-----:|:------:|:--------:|
|io.js|all|no|
|nodejs|<8.xl|> = 6.3（Windows：> = 6.9）|
|Electron|all|not yet|
|Chakra|all|not yet|

虽然`VScode`的`Node.js`调试器可能会始终自动选择最佳协议，但我们尽量使用明确的启动配置属性 `protocol`.

`protocol` 有以下的值：

* **auto**: 针对特定的运行环境自动去匹配使用的`protocol`，对于配置文件中的 `configurations`中的`request` 类型，或者没有配置该类型
* **inspector**：强制使用`inspector protocol`,这配置被`Node`版本大于等于 6.3 支持，但是还不被`Electron`支持
* **Legacy**：强制使用`Legacy protocol`,这配置被`Node`小于 8.0 和`Electron`支持

从 `VScode` 1.11开始，默认的值为 `auto`

如果你的运行环境，两种协议都支持，一般则是选择使用 `inspector`  而不是使用 `Legacy` 的原因

* 调试非常大的JavaScript对象时可以更稳定，当在客户端和服务器之间发送大的值时，`Legacy`协议可能会变得很慢
* 如果在项目中使用`ES6`代理，使用`inspector protocol`则可以防止在`Node v7+ `崩溃，这是一个`issue` [Microsoft/vscode#12749](https://github.com/Microsoft/vscode/issues/12749)
* 通过`inspector protocol`调试可以处理一些棘手的`source map`。如果在`source map`文件中设置断点时遇到问题，请尝试使用`inspector protocol`

我们尽量试图保持这两种协议实现同样的特征，但是这变得越来越困难，由于`Legacy protocol`被废弃和`inspector protocol`的快速发展。
由于这个原因，我们指定一个被支持的协议，如果一个特征不被`legacy`和`inspector`支持

##### 启动配置属性

下面的配置属性，被支持在启动配置类型为 `launch`和`attach`中
* `protocol`:使用的调试协议
* `port`:使用的调试端口
* `address`:使用的调试TCP / IP地址
* `restart`:终止时重新启动会话(restart session on termination)
* `timeout`:重新启动会话时，在这个毫秒数之后丢弃
* `stopOnEntry`:程序启动时立即中断
* `localRoot`:`VScode`的根目录
* `remoteRoot`:`Node`的根目录
* `sourceMaps`:开启`source maps`设置为`true`

下面的配置属性，只被支持在启动配置类型为 `launch`中
* `program`:程序的绝对路径
* `cwd`:启动程序在此目录中进行调试
* `runtimeExecutable`:要使用的运行时可执行文件的绝对路径。默认是node,此处还可以指定为其他的可执行文件如：`${workspaceRoot}/node_modules/.bin/electron` 或者 直接写为 `electron` `npm` `gulp`

#### 项目实例

以一个`express`项目为例讲解

用 `VScode`打开项目后，点击调试的按钮
![alt](/images/vscode调试nodejs的配置介绍/vscode_start.png)

这时候时没有配置文件的，点击那个齿轮⚙️ 会自动生成一个配置文件，或者点击**没有配置**那个下拉框新建一个配置文件

新建的配置文件为：
![alt](/images/vscode调试nodejs的配置介绍/vscode_launch_init.png)

然后会在项目目录下创建一个`.vscode`文件夹，下面有一个`launch.json`文件

可以把鼠标放在属性名称上，看对应属性的解释：
* `type`:配置类型
* `request`:请求配置类型。可以是“启动(launch)”或“附加(attach)”
* `name`:配置名称；在启动配置下拉菜单中显示
* `program`:程序的绝对路径。通过查看 `package.json` 和打开的文件猜测所生成的值。编辑此属性
* `address`:待调试进程的 TCP/IP 地址(仅限 Node.js >= 5.0)。默认值为 "localhost"
* `port`:调试要附加的端口。默认端口是 5858

在`launch.json`中会使用到一些预定变量, 这些变量的具体含义如下

* `${workspaceRoot}`:VSCode中打开文件夹的路径


***待完成***

