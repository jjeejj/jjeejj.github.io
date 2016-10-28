---
title: Hexo Next主题的使用说明
date: 2016-10-23 16:16:55
tags: [Hexo]
description: Hexo 安装主题的方式非常简单，只需要将主题文件拷贝至站点目录的 themes 目录下， 然后修改下配置文件即可.对于NexT 来说，安装步骤如下：
---
-------------------------
#### Hexo 安装主题的方式非常简单，只需要将主题文件拷贝至站点目录的 themes 目录下， 然后修改下配置文件即可


## 安装 NexT

对于NexT 来说，安装步骤如下：

### 下载主题

在终端窗口下，定位到 Hexo 站点目录下。使用 Git checkout 代码：

	$ cd your-hexo-site
	$ git clone https://github.com/iissnan/hexo-theme-next themes/next

>如果你对 Git 感兴趣，可以通过《Pro Git》这本书来学习。你可以访问[在线版本（第一版）](http://iissnan.com/progit/)


### 启用主题

与所有Hexo主题启用的模式一样。当克隆/下载完成后，打开 <button style="color:red;border-radius:5px;background-color: yellowgreen">站点配置文件</button>， 找到 theme 字段，并将其值更改为 ***hexo-theme-next***

	theme: hexo-theme-next

>到此，NexT 主题安装完成。下一步我们将验证主题是否正确启用。在切换主题之后、验证之前， 我们最好使用 hexo clean 来清除 Hexo 的缓存。

### 验证主题

首先启动 Hexo 本地站点，并开启调试模式（即加上 --debug），整个命令是:

	hexo s --debug
	
	INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
	DEBUG Database saved

此时即可使用浏览器访问 http://localhost:4000，检查站点是否正确运行

## 主题设定

### 选择Scheme

Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。同时，几乎所有的配置都可以 在 Scheme 之间共用。目前 NexT 支持三种 Scheme，他们是:
* Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
* Mist - Muse 的紧凑版本，整洁有序的单栏外观
* Pisces - 双栏 Scheme，小家碧玉似的清新

Scheme 的切换通过更改 <button style="color:write;border-radius:5px;background-color: rgb(153,84,187)">主题配置文件</button>，搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释 # 即可。

	#scheme: Muse
	#scheme: Mist
	scheme: Pisces
	
### 设置语言

编辑 <button style="color:red;border-radius:5px;background-color: yellowgreen">站点配置文件</button>，将 language 设置成你所需要的语言。建议明确设置你所需要的语言，例如选用简体中文，配置如下：

	language: zh-Hans
	
目前 NexT 支持的语言如以下表格所示：

|**语言**|**代码**|**设定示例**|
|:-------|:-------|:-----------|
|English|en|language: en|
|简体中文|zh-Hans|language: zh-Hans|
|Français|fr-FR|language: fr-FR|
|Português|pt|language: pt|
|繁體中文|zh-hk 或者 zh-tw|language: zh-hk|
|Русский язык|ru|language: ru|
|Deutsch|de|language: de|
|日本語|ja|language: ja|
|Indonesian|id|language: id|

### 设置菜单

菜单配置包括三个部分，第一是菜单项（名称和链接），第二是菜单项的显示文本，第三是菜单项对应的图标。NexT 使用的是 Font Awesome 提供的图标，[Font Awesome](http://fontawesome.io/) 提供了600+ 的图标，可以满足绝大的多数的场景，同时无须担心在 Retina 屏幕下 图标模糊的问题。

编辑 <button style="color:write;border-radius:5px;background-color: rgb(153,84,187)">主题配置文件</button>，修改以下内容：

①：设定菜单内容，对应的字段是 menu。 菜单内容的设置格式是：item name: link。其中 item name 是一个名称，这个名称并不直接显示在页面上，她将用于匹配图标以及翻译。

	menu:
	  home: /
	  archives: /archives
	  #about: /about
	  #categories: /categories
	  tags: /tags
	  #commonweal: /404.html

>若你的站点运行在子目录中，请将链接前缀的 / 去掉

NexT 默认的菜单项有（标注✿的项表示需要手动创建这个页面）：

|**键值**|**设定值**|**显示文本（简体中文**|
|:-------|:---------|:---------------------|
|home|home: /|主页|
|archives|archives: /archives|归档页|
|categories|categories: /categories|分类页✿|
|tags|tags: /tags|标签页✿|
|about|about: /about|关于页面✿|
|commonweal|commonweal: /commonweal|公益 404✿|

②：设置菜单项的显示文本。在第一步中设置的菜单的名称并不直接用于界面上的展示。Hexo 在生成的时候将使用 这个名称查找对应的语言翻译，并提取显示文本。这些翻译文本放置在 NexT 主题目录下的 languages/{language}.yml （{language} 为你所使用的语言）。

以简体中文为例，若你需要添加一个菜单项，比如 something。那么就需要修改简体中文对应的翻译文件 languages/zh-Hans.yml，在 menu 字段下添加一项：

	menu:
		home: 首页
		archives: 归档
		categories: 分类
		tags: 标签
		about: 关于
		search: 搜索
		commonweal: 公益404
		something: 有料
	
③：设定菜单项的图标，对应的字段是 menu_icons。 此设定格式是 item name: icon name，其中 item name 与上一步所配置的菜单名字对应，icon name 是 Font Awesome 图标的 名字。而 enable 可用于控制是否显示图标，你可以设置成 false 来去掉图标。

	menu_icons:
		enable: true
		# Icon Mapping.
		home: home
		about: user
		categories: th
		tags: tags
		archives: archive
		commonweal: heartbeat

>在菜单图标开启的情况下，如果菜单项与菜单未匹配（没有设置或者无效的 Font Awesome 图标名字）的情况下，NexT 将会使用 ? 作为图标
>请注意键值（如 home）的大小写要严格匹配
	  
### 设置侧栏

可以通过修改 <button style="color:write;border-radius:5px;background-color: rgb(153,84,187)">主题配置文件</button> 中的 sidebar 字段来控制侧栏的行为。侧栏的设置包括两个部分，其一是侧栏的位置，其二是侧栏显示的时机。

①：设置侧栏的位置，修改 sidebar.position 的值，支持的选项有：
* left - 靠左放置
* right - 靠右放置
>目前仅 Pisces Scheme 支持 position 配置。影响版本5.0.0及更低版本

	sidebar:
		position: left

②：设置侧栏显示的时机，修改 sidebar.display 的值，支持的选项有：
* post - 默认行为，在文章页面（拥有目录列表）时显示
* always - 在所有页面中都显示
* hide - 在所有页面中都隐藏（可以手动展开）
* remove - 完全移除

	sidebar:
	  display: post
	  
>已知侧栏在 use motion: false 的情况下不会展示。 影响版本5.0.0及更低版本。

### 设置头像

编辑 <button style="color:red;border-radius:5px;background-color: yellowgreen">站点配置文件</button> 新增字段 avatar，值设置成头像的链接地址。其中，头像的链接地址可以是

|**地址**|**值**|
|:-------|:-------|
|完整的互联网 URI|http://example.com/avtar.png|
|站点内的地址|将头像放置主题目录下的 source/uploads/（新建uploads目录若不存在）<br/> 配置为：avatar: /uploads/avatar.png <br/> 或者 放置在 source/images/ 目录下 <br/> 配置为：avatar: /images/avatar.png |

	avatar: /images/avatar.png

### 设置作者昵称

编辑 <button style="color:red;border-radius:5px;background-color: yellowgreen">站点配置文件</button>，设置 author 为你的昵称。

### 站点描述

编辑 <button style="color:red;border-radius:5px;background-color: yellowgreen">站点配置文件</button> 设置 description 字段为你的站点描述。

## 第三方服务

### 网易云音乐

去网易云音乐选择一首音乐，生成外链，嵌套到文章中

生成的代码如下：

	<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="http://music.163.com/outchain/player?type=2&id=114056&auto=1&height=66"></iframe>

还可以放到侧边栏主站中：

将外链插入到hexo/themes/hexo-theme-next/layout中的文件中
我选择把播放器放在sidebar里面，所以我就选择了_macro文件夹中的 sidebar.swig文件，将之前复制的代码到了该文件中：

```
{% if theme.links %}
	  <div class="links-of-blogroll motion-element {{ "links-of-blogroll-" + theme.links_layout | default('inline') }}">
		<div class="links-of-blogroll-title">
		  <i class="fa  fa-fw fa-{{ theme.links_icon | default('globe') | lower }}"></i>
		  {{ theme.links_title }}
		</div>
		<ul class="links-of-blogroll-list">
		  {% for name, link in theme.links %}
			<li class="links-of-blogroll-item">
			  <a href="{{ link }}" title="{{ name }}" target="_blank">{{ name }}</a>
			</li>
		  {% endfor %}
		</ul>
	  </div>
	  把生成的代码放到这个位置
{% endif %}
```

### 百度分享


## Q & A

### 1. Tags 标签页 404 Not Get/

		新建一篇文章
		hexo new page tags
		在 该文章的Front-matter 中添加
		type: "tags"
		主题菜单中的路径改为：
		tags: /tags

	
------------------------------------
