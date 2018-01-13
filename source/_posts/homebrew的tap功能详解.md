---
title: homebrew的tap功能详解
tags:
  - mac
  - homebrew
date: 2018-01-13 15:14:55
---

--------------------------------------------

`Homebrew`是使用`ruby`开发的`Mac`的软件包管理器.
这里就说明一下有关`Taps(third-party-repositories)`的知识

-------------------------------------------

<!--more-->

`brew tap`可以为`brew`的软件的 跟踪,更新,安装添加更多的的`tap formulae`

如果你在核心仓库没有找到你需要的软件,那么你就需要安装第三方的仓库去安装你需要的软件

`tap`命令的仓库源默认来至于`Github`，但是这个命令也不限制于这一个地方

#### brew tap 命令

* `brew tap` 没有参数会自动更新已经存在的`tap`并列出当前已经`tapped`的仓库
![tap_update](/images/homebrew的tap功能详解/tap_update.jpg)
![tap_list](/images/homebrew的tap功能详解/tap_list.jpg)

* `brew tap <user>/<repo>` 在本地对这个 `https://github.com/user/repo` 仓库上做了一个浅度的克隆，完成之后 `brew`就可以在这个仓库包含的`formulae`上工作,好比就在`Homebrew`规范的仓库,你可使用`brew install` 或者`brew uninstall `安装或者卸载这个仓库上的软件。当你执行`brew update`这个命令时，`tap` 和 `formulae` 就会自定更新
![tap_one](/images/homebrew的tap功能详解/tap_one.jpg)

* `brew tap <user>/<repo> URL` 在本地对这个 `URL` 仓库上做了一个浅度的克隆,和上面一个参数命令是不一样的,`URL`没有默认关联到`Github`,这个`URL`没有要求必须是`HTTP`协议，任何位置和任何协议而且**Git**也是能很好的处理的

* `brew untap <user>/<repo> [<user>/<repo> <user>/<repo> ...]` 移除已经安装的`tap`.这个仓库被删除,`brew`就不在可用在这个仓库的`formulae`.可以同时删除几个仓库

#### 仓库命名的规范

* 在 `Github`上,你的仓库名称必须是`homebrew-something`,为了使用一个参数的`brew tap`命令,`homebrew-`这个前缀不是可选的,是必须的。
    对于两个参数的`brew tap`命令没有这个限制,但是必须给出明确的全部的`URL`地址
* 当你在命令行使用`brew tap`时，你可以省略`homebrew-`这个前缀的

也就是说:`brew tap username/foobar`是作为长版本`brew tap username/homebrew-foobar`使用的一个简写.

**`brew`可以自己添加`homebrew-`前缀的在需要的时候**

#### 重复名称安装包的处理

如果你想安装的一个安装包在你`tap`的一个仓库上,但是同时还出现在了`homebrew/core`上,这就意味着你必须明确指出`tap`的名称去安装它,否则就会默认安装`homebrew/core`上的包.

如果你想要是你安装的`tap`的优先顺序高于`homebrew/core`这个默认的仓库,你可以使用`brew tap-pin username/repo`去**pin**这个仓库.你可以使用`brew-tap-unpin username/repo` 恢复这个`pin`

当你使用`brew install foo`这个命令时,`brew` 将按照下面的顺序去查找哪个`formula(tap)`将被使用:
1. pinned taps
2. core formulae
3. other taps

举个例子：

你想安装`vim`安装包,而且没有`pinned`某个仓库:
```
brew install vim  # installs from homebrew/core
brew install username/repo/vim  # installs from your custom repo
```

你想安装`vim`安装包,而且有`pinned`的仓库:
```
brew install vim  # installs from your custom repo
brew install homebrew/core/vim # installs from homebrew/core
```

#### 可以关注的Taps

* [`homebrew/php`](https://github.com/Homebrew/homebrew-php):和`php`关联的`formulae`
* [`denji/nginx`](https://github.com/denji/homebrew-nginx): `nginx modules` 的`tap`
* [`InstantClientTap/instantclient`](https://github.com/InstantClientTap/homebrew-instantclient): `Oracle`客户端实例的`tap`
* [`petere/postgresql`](https://github.com/petere/homebrew-postgresql): 允许同时安装多个`PostgreSQL`版本的`tap`
* [`dunn/emacs`](https://github.com/dunn/homebrew-emacs): `Emacs package`的`tap`
* [`sidaf/pentest`](https://github.com/sidaf/homebrew-pentest): 渗透测试工具的`tap`
* [`osrf/simulation`](https://github.com/osrf/homebrew-simulation): 机器仿真的`tap`


![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**
