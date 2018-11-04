---
title: gitment评论模块接入hexo
tags:
  - hexo
date: 2017-07-02 12:43:46
---

-------------------------------------------
[**gitment**](https://github.com/imsun/gitment)评论系统接入`hexo`

由于用的`next`主题,就以该主题进行讲解.主题版本为 `5.0.0`

新的版本会直接集成 `gitment` 系统

--------------------------------------
<!--more-->

`Gitment` 是一款基于`GitHub Issues` 的评论系统,只能使用 GitHub 账号进行评论

#### 注册 OAuth Application

[来这里注册](https://github.com/settings/applications/new),这里的`Homepage URL`和`Authorization callback URL`一般是博客首也地址
需要注意的是，这里需要填写完成，需带上协议名如：`http://`

![alt](/images/gitment评论模块接入hexo/gitment_auth.jpg)

会得到一个 `client ID` 和一个 `client secret`，这个将被用于之后的用户登录
![alt](/images/gitment评论模块接入hexo/gitment_client.jpg)

#### 修改主题配置

**在主题下的`_config.yml`文件中最后添加**
```yml
# Gitment
# Introduction: https://imsun.net/posts/gitment-introduction/
gitment:
    enable: true
    githubID: <githubID>
    repo: <repo>
    ClientID: <ClientID>
    ClientSecret: <ClientSecret>
    lazy: false
```

`githubID`:为`github`的用户名
`repo`:为博客的仓库
`ClientID`:为上一步获取的`client ID`
`ClientSecret`:为博客的仓库`client secret`
`lazy`:是否默认把评论框收起来

>此处应该注意缩进

**修改语言的配置，在主题下的`language`文件夹下**

`zh-Hans.yml`中添加
```yml
gitmentbutton: 显示 Gitment 评论
```

`zh-en.yml`中添加
```yml
gitmentbutton: Show comments from Gitment
```

`zh-hk.yml`中添加
```yml
gitmentbutton: 顯示 Gitment 評論
```

`zh-tw.yml`中添加
```yml
gitmentbutton: 顯示 Gitment 評論
```

**修改layout文件下的_partials中的comments.swig文件**

```js
{% elseif theme.gitment.enable %}
    {% if theme.gitment.lazy %}
        <div onclick="ShowGitment()" id="gitment-display-button">{{ __('gitmentbutton') }}</div>
        <div id="gitment-container" style="display:none"></div>
    {% else %}
        <div id="gitment-container"></div>
    {% endif %}
{% endif %}
```

**在layout/_scripts/third-party/comments/下添加gitment.swig文件**
文件内容为：
```js
{% if theme.gitment.enable %}
     {% set owner = theme.gitment.githubID %}
     {% set repo = theme.gitment.repo %}
     {% set cid = theme.gitment.ClientID %}
     {% set cs = theme.gitment.ClientSecret %}
     <link rel="stylesheet" href="https://www.wenjunjiang.win/css/gitment.css">
     <script src="https://www.wenjunjiang.win/js/gitment.js"></script>
     {% if not theme.gitment.lazy %}
         <script type="text/javascript">
             var gitment = new Gitment({
                 id: document.location.href, 
                 owner: '{{owner}}',
                 repo: '{{repo}}',
                 oauth: {
                     client_id: '{{cid}}',
                     client_secret: '{{cs}}'
                 }});
             gitment.render('gitment-container');
         </script>
     {% else %}
         <script type="text/javascript">
             function ShowGitment(){
                 document.getElementById("gitment-display-button").style.display = "none";
                 document.getElementById("gitment-container").style.display = "block";
                 var gitment = new Gitment({
                     id: document.location.href, 
                     owner: '{{owner}}',
                     repo: '{{repo}}',
                     oauth: {
                         client_id: '{{cid}}',
                         client_secret: '{{cs}}',
                     }});
                 gitment.render('gitment-container');
             }
         </script>
     {% endif %}
 {% endif %}
```

**在layout/_scripts/third-party下的文件comments.swig文件添加件**
```
{% include './comments/gitment.swig' %}
```
**在该source/css/_common/components/third-party/增加评论的样式gitment.styl**
文件内容
```css
#gitment-display-button{
     display: inline-block;
     padding: 0 15px;
     color: #0a9caf;
     cursor: pointer;
     font-size: 14px;
     border: 1px solid #0a9caf;
     border-radius: 4px;
 }
 #gitment-display-button:hover{
     color: #fff;
     background: #0a9caf;
 }
```

**在该source/css/_common/components/third-party/third-party.styl导入gitment的配置**
```css
@import "gitment";
```

　>最后打开发布的页面，用自己`github`帐号进行登录初始化

#### 参考文章

[Feature: Add Gitment Support ](https://github.com/iissnan/hexo-theme-next/pull/1634/files)








