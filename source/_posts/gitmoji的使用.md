---
title: gitmoji的使用
tags:
  - git
date: 2016-11-22 23:26:16
---


---------------------------------
git commit emoji 使用指南

执行`git commit` 使用`emoji`可以打个标签，使得此次 commit 的主要工作得以凸现，也能够使得其在整个提交历史中易于区分与查找

<!--more-->

效果图：
![alt](/images/gitmoji/gitmoji-show.png)

但是 emoji 表情在提交代码的时候也不能乱用，否则容易造成误解。因此开源项目 [gitmoji](https://github.com/carloscuesta/gitmoji/) 专门规定了在 github 提交代码时应当遵循的 emoji 规范

## commit 格式

git commit 时，提交信息遵循以下格式：

```
git commit -m ':emoji1: :emoji2: 主题'
```

示例：

文档说明

```
git commit -m ':memo: 文档说明'
```

## emoji 说明

|**emoji**|**emoji代码**|**说明**|
|:---:|:---:|:---:|
|:art:|`:art:`|改进代码结构/代码格式|
|:zap:|`:zap:`|提升性能|
|:fire:|`:fire:`|移除代码或文件|
|:bug:|`:bug:`|修复bug|
|:ambulance:|`:ambulance:`|关键补丁|
|:sparkles:|`:sparkles:`|引入新的功能|
|:memo:|`:memo:`|写文档|
|:rocket:|`:rocket:`|部署功能|
|:lipstick:|`:lipstick:`|更新 UI 和样式文件|
|:tada:|`:tada:`|初次提交|
|:white_check_mark:|`:white_check_mark:`|增加测试|
|:lock:|`:lock:`|修复安全问题|
|:apple:|`:apple:`|修复 macOS 下的问题|
|:penguin:|`:penguin:`|修复 Linux 下的问题|
|:checkered_flag:|`:checkered_flag:`|修复 Windows 下的问题|
|:bookmark:|`:bookmark:`|发行/版本标签|
|:rotating_light:|`:rotating_light:`|移除 linter 警告|
|:construction:|`:construction:`|工作进行中|
|:green_heart:|`:green_heart:`|移除 linter 警告|
|:rotating_light:|`:rotating_light:`|添加 CI 构建系统|
|:arrow_down:|`:arrow_down:`|降级依赖|
|:arrow_up:|`:arrow_up:`|升级依赖|
|:construction_worker:|`:construction_worker:`|添加 CI 构建系统|
|:chart_with_upwards_trend:|`:chart_with_upwards_trend:`|添加分析或跟踪代码|
|:hammer:|`:hammer:`|重构|
|:heavy_minus_sign:|`:heavy_minus_sign:`|少一个依赖|
|:whale:|`:whale:`|Docker 相关工作|
|:heavy_plus_sign:|`:heavy_plus_sign:`|增加一个依赖|
|:wrench:|`:wrench:`|修改配置文件|
|:globe_with_meridians:|`:globe_with_meridians:`|国际化与本地化|
|:pencil2:|`:pencil2:`|修复 typo|
|:hankey:|`:hankey:`|编写需要改进的错误代码|

## 参考资料

* [gitmoji](https://github.com/carloscuesta/gitmoji/) 
* [emoji-cheat-sheet](http://www.webpagefx.com/tools/emoji-cheat-sheet/)
