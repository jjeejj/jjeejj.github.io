---
title: hexo支持moji表情的方法
tags:
  - hexo
date: 2016-11-23 13:39:04
---

-------------------------------
将`markdown`转化为`html`的转化器叫做`markdown渲染器`

在`hexo`中默认的`markdown渲染器`是`hexo-renderer-marked`,这个渲染器是不支持插件扩展的

而[hexo-renderer-markdown-it](https://github.com/celsomiranda/hexo-renderer-markdown-it) 是支持扩展的

可以使用[markdown-it-emoji](https://github.com/markdown-it/markdown-it-emoji)插件来支持`emoji`

需将原来的`marked`渲染器换成`markdown-it`渲染器

<!--more-->

## 安装

首先进去`hexo`博客的根目录下，卸载`marked`渲染器 并安装`markdown-it`渲染器
```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-markdown-it --save
```

之后再安装`markdown-it-emoji`插件

由于`hexo-renderer-markdown-it`原装只有5个插件，分别是：

* markdown-it-abbr
* markdown-it-footnote
* markdown-it-ins
* markdown-it-sub
* markdown-it-sup

没有emoji的表情插件，因此要安装 markdown-it-emoji 该插件
```
npm install markdown-it-emoji --save
```

## 配置

打开站点根目录下的配置文件`_conifg.yml`,在末尾添加以下内容
```
# Markdown-it config
## Docs: https://github.com/celsomiranda/hexo-renderer-markdown-it/wiki
markdown:
  render:
    html: true
    xhtmlOut: false
    breaks: true
    linkify: true
    typographer: true
    quotes: '“”‘’'
  plugins:
    - markdown-it-abbr
    - markdown-it-footnote
    - markdown-it-ins
    - markdown-it-sub
    - markdown-it-sup
    - markdown-it-emoji
  anchors:
    level: 2
    collisionSuffix: 'v'
    permalink: false
    permalinkClass: header-anchor
    permalinkSymbol: ¶
```

关于上面详细的每一项配置项
请参考[Advanced Configuration](https://github.com/celsomiranda/hexo-renderer-markdown-it/wiki/Advanced-Configuration)

## 给新的渲染器添加twemoji

由于安装了`markdown-it-emoji` `:smile:`渲染为😄 为Unicode字符表情，不好看

所以安装twemoji
```
npm install twemoji --save
```
安装完成后编辑`node_modules/markdown-it-emoji/index.js` 最终的文件内容为：
```js
'use strict';


var emojies_defs      = require('./lib/data/full.json');
var emojies_shortcuts = require('./lib/data/shortcuts');
var emoji_html        = require('./lib/render');
var emoji_replace     = require('./lib/replace');
var normalize_opts    = require('./lib/normalize_opts');
var twemoji           = require('twemoji');//添加twemoji

module.exports = function emoji_plugin(md, options) {
  var defaults = {
    defs: emojies_defs,
    shortcuts: emojies_shortcuts,
    enabled: []
  };

  var opts = normalize_opts(md.utils.assign({}, defaults, options || {}));

  md.renderer.rules.emoji = emoji_html;
  //使用 twemoji 渲染
  md.renderer.rules.emoji = function(token, idx) {
    return twemoji.parse(token[idx].content);
  };

  md.core.ruler.push('emoji', emoji_replace(md, opts.defs, opts.shortcuts, opts.scanRE, opts.replaceRE));
};
```
因为我安装twemoji的2.2.0之后，默认的是72X72,显示有点大，可以通过css控制，在你的主题css中添加如:
```css
img.emoji {
   height: 3em;
   width: 3em;
   margin: 0 .05em 0 .1em;
   vertical-align: -0.1em;
}
```
>我用的主题是`next`，修为样式的路径为`hexo-theme-next\source\css\_custom` 下的`custom.styl`


现`emoji`就可以使用了

## 参考文章

* [Hexo中添加emoji表情](http://www.cnblogs.com/fsong/p/5929773.html)