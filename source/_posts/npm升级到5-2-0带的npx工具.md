---
title: npm升级到5.2.0带的npx工具
tags:
  - node
  - other
date: 2017-07-13 22:01:55
---

-----------------------------------
今天更新无意中执行了该命令了，发现了一个好玩的东西
`npm install npm -g`

----------------------------------
<!--more-->

更新完成后
![alt](/images/npm升级到5.2.0带的npx工具/npmupdate.jpg)

发现一个附带的工具`npx`出现了。
更新后的`npm`的版本为 `5.2.0`，而`npx`的版本为`9.0.3`,则说明`npx` 之前就已经存在了，而是在这个`npm`版本中进行了官方化。

果然`github`一查就出来了，地址[npx](https://github.com/zkat/npx),作者为`npm`的工作人员，果断`follow`

**总的来说`npx`命令的作用执行`npm`包**

例如：

在项目下执行一下命令：
`npm i -D webpack`
则就可以直接使用下面的命令：
`npx webpack ...`

还可以这样使用：
![alt](/images/npm升级到5.2.0带的npx工具/npx_httpserver.jpg)

 具体的细致的文档，请参考官方文档 [https://github.com/zkat/npx](https://github.com/zkat/npx)
