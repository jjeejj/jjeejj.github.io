---
title: hexo博客换电脑后重新配置步骤
tags: [hexo]
---

------------------------

由于经常重做系统或换了电脑，想要继续写hexo博客就得重新配置，但是每次都要去上网查询怎么去配置，比较耗时。所以就自己记录下来，方便以后产看。
（源代码都已经托管到 GitHub 上了，包括文件源文件和hexo 生成的静态文件）

<!--more-->

### 安装运行环境

* 安装 `node` 方法参考 [官网](https://nodejs.org)
* 安装 `hexo` 
方法：`npm install hexo-cli -g`
* 安装 `git`

### 克隆 github 上的源文件

* 克隆分支拉取`github`上的源文件
`git clone -b hexo <git repo> hexo-bolg`, `hexo` 代表分支，`hexo-bolg` 代表本地文件夹
* 安装依赖
`npm install`