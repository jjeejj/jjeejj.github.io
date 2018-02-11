---
title: 利用CI自动部署hexo博客
tags:
  - hexo
date: 2018-02-11 22:41:58
---

------------------------------------------------------------

每次写完一篇文章,都会手动执行`hexo g`和`hexo d`去生成静态网页后在进行部署到`Github page上去` 

而且为了保存文章的源码`md`文件还需要`push`到对应的仓库分支上,比较麻烦

都说`懒惰`推动着新的东西出现,那么能不能只保存原`md`文件`push`上去,其他的操作都让它自动去完成呢 ？

答案肯定是可以的,下面就来讲解一下CI具体的实现方法(**这里只讲解如何把travis接入**):

------------------------------------------------------------
<!--more-->

首先来介绍利用的工具: 

* `Github Page`:是`github`网站提供的静态网站服务,集体详细的介绍可以查看[官方文档](https://pages.github.com/)
* `travis`: 是在线托管的`CI`服务,用`Travis`来进行持续集成,不需要自己搭服务器[官方网站](https://travis-ci.org/)

接入`Travis`过程:

1. 登录`Travis`网站用`github`授权登录
2. 登录后在个人主页选择你需要`CI`的仓库
3. 点击你选择的`hexo`博客的仓库进行配置
点击左上角红色框的**More options**按钮,选择`Settings`打开配置页面进行配置
![alt](/images/利用CI自动部署hexo博客/ci_setting.jpg)

第一个配置项:`Build only if .travis.yml is present`代表的意思是:只有在`.travis.yml`文件中配置的**分支**改变了才构建
第二个配置项:`Build pushes`代表当推送完这个分支后开始构建

到了这一步,我们已经开启了要构建的仓库,但是还有个问题就是,构建完后,我们怎么将生成的文件推送到github上呢? 
我们只要想`github一push`,他就自动构建并`push`静态文件到`githubpages`,那么下面要解决的就是`Travis CI`怎么访问`github`了

4. 在`Travis CI`配置`Github`的`Access Token` 用来访问`Github`

首先我们进入`github`的设置页面,然后点击`Developer settings`选项进入开发者设置,然后字点击`Personal access tokens`
![alt](/images/利用CI自动部署hexo博客/github_access_token.jpg)
点击右上角的`Generate new token`会让你输入密码确定,然后进入一个生成`token`的页面
![alt](/images/利用CI自动部署hexo博客/generate_access_token.jpg)

输入`token`的描述,选择这个`token`权限,然后然后点击生成就可以了,然后复制保存下来,下次在进来就看不到了
![alt](/images/利用CI自动部署hexo博客/generate_access_token_after.jpg)

左后还在 `Travis` 仓库配置界面`setting`里面 环境变量`Environment Variables`进行配置`token`方便在构建文件中引用:如下图
![alt](/images/利用CI自动部署hexo博客/config_token.jpg)
4. 在博客的源码文件分支下添加`.travis,yml`配置文件,决定怎么执行构建任务,下面是`.travis,yml`的内容:
```yml
language: node_js
node_js: stable

install:
  - npm install

script:
  - hexo g

after_script:
  - cd ./public
  - git init 
  - git config user.name "jjeejj"
  - git config user.email "wenjunjiang1993@163.com"
  - git add .
  - git commit -m "Update docs"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master

branches:
  only:
    - hexo
env:
 global:
   - GH_REF: github.com/jjeejj/jjeejj.github.io.git
```

这其中的`config`改成对应你自己的

到此为止,就已经配置完成了,下面就让我们推送一篇文章测试一下的
现在我本地源码目录有新加的一篇文章:
![alt](/images/利用CI自动部署hexo博客/new_article.jpg)

然后进行`push`提交(此刻应该等待编译成功了)

然后稍等一会就可以在博客网站上看到刚才的那篇文章了
![alt](/images/利用CI自动部署hexo博客/new_article_show.jpg)

你也可以在`Travis`的博客仓库的控制台看到编译的日志
![alt](/images/利用CI自动部署hexo博客/new_article_build_log.jpg)

>这里备注一下,由于平常我们写文章都是使用`heox new draft`新建的草稿文件,是放在`_drafts`文件夹中的,写完之后需要手动`hexo publish`一下,移到`_psots`文件夹中的,使用构建时候,还是需要这样做的