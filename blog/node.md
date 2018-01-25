# Nodejs

## 简介

Node.js就是运行在服务器端的JavaScript，是一个**事件驱动I/O服务端** JavaScript环境，基于Google的V8引擎。

## 安装

基于我们在用windows 和 mac ，所以介绍这两种操作系统的安装配置。

![nodejs](https://yulongge.github.io/images/nodejs/download.png)

### window

下载 Windows Installer (.msi)

### mac

`homebrew`
```js
brew install node
```

`下载包`

macOS Installer (.pkg)	

## npm

NPM 是随同Nodejs 一起安装的包管理工具,能解决Nodejsd代码部署上的很多问题.

用法：

- 允许用户从npm服务器下载别人编写的第三方包到本地使用
- 允许用户从npm服务器下载并安装别人编写的命令行程序到本地使用
- 允许用户将自己编写的包或命令行程序上传到npm服务器供别人使用

> 新版的nodejs已经集成了npm,所以npm一并安装好了,可以用 `npm -v` 来检测是否安装成功。

### npm 常用命令

```js
//安装模块
npm install [name]
```

### npm 镜像(淘宝)

这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步

你可以使用我们定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
这样就可以使用 cnpm 命令来安装模块了
```js
cnpm install [name]
```

### package.json

package.json 位于模块的目录下，用于定义包的属性

### shrinkwrap
锁定版本
## REPL

Node REPL(Read Eval Print Loop:交互式解释器)

可以执行一下任务:

- 读取
- 执行
- 打印
- 循环

## 管理node版本

大量开发者的贡献使Node版本的迭代速度很快，版本很多，所以升级Node版本就成为了一个问题。目前有n和nvm这两个工具可以对Node进行无痛升级，本文简单介绍一下二者的使用

### n

n是Node的一个模块，作者是TJ Holowaychuk（鼎鼎大名的Express框架作者），就像它的名字一样，它的理念就是简单

> `no subshells, no profile setup, no convoluted api, just simple`

```js
npm install -g n

//npm ERR! notsup Unsupported platform for n@2.1.8: wanted {“os”:”!win32”,”arch”:” any”} (current: {“os”:”win32”,”arch”:”x64”}) 

npm install -g n --force

```

#### 命令

```js
//查看已安装的版本
n

//查看最新版本
n --latest

// 安装最新的版本并使用
n latest (-d) // -d表示仅下载不使用

//查看最稳定的版本
n --stable

//安装最新稳定的版本并使用
n stable

//安装某个版本并使用
n <version> //n 6.2.2

//删除某些版本
n rm <version>

//查看可用版本
n ls

//查看帮助信息
n -h

//以制定的版本来执行脚本
n use <version> some.js
```

### nvm

> 既然有这么简单好用的n，那么nvm为什么还会大肆流行呢？

nvm全称Node Version Manager，不同于 n，nvm 不是一个 npm package，而是一个独立软件包. 它与n的实现方式不同，其是通过shell脚本实现的.

安装方式:

```js
//curl
curl https://raw.github.com/creationix/nvm/v0.4.0/install.sh | sh

//wget
wget -qO- https://raw.github.com/creationix/nvm/v0.4.0/install.sh | sh
```

以上脚本会把 `nvm` 库clone到 `~/.nvm`，然后会在 `~/.bash_profile`, `~/.zshrc`或`~/.profile` 末尾添加`source`，安装完成之后，你可以用以下命令来安装`node`

nvm 不支持windows

[nvm-windows](https://github.com/coreybutler/nvm-windows) 适用于windows

### 常用命令

```js
//安装指定的版本
nvm install 0.10

//使用指定的版本
nvm use 0.10

//查看当前已经安装的版本
nvm ls

//查看正在使用的版本
nvm current

//以指定版本执行脚本
nvm run 0.10.24 some.js

//卸载nvm
rm -rf ~/.nvm

```

### `n` vs `nvm`

- 安装简易度
nvm 安装起来显然是要麻烦不少；n 这种安装方式更符合 node 的惯性思维

- 依赖
我们在使用 n 管理 node 版本前，首先需要一个 node 环境。我们或者用 Homebrew 来安装一个 node，或者从官网下载 pkg 来安装，总之我们得先自己装一个 node —— n 本身是没法给你装的。
然后我们可以使用 n 来安装不同版本的 node。
在安装的时候，n 会先将指定版本的 node 存储下来，然后将其复制到我们熟知的路径 /usr/local/bin，非常简单明了。当然由于 n 会操作到非用户目录，所以需要加 sudo 来执行命令。

- 全局模块的管理
对全局模块的管理。n 对全局模块毫无作为，因此有可能在切换了 node 版本后发生全局模块执行出错的问题；nvm 的全局模块存在于各自版本的沙箱中，切换版本后需要重新安装，不同版本间也不存在任何冲突。
关于 node 路径。n 是万年不变的 /usr/local/bin；nvm 需要手动指定路径

## 衍生

- [express](http://expressjs.com/zh-cn/) 是一个简洁而灵活的 node.js Web应用框架

- [koa](https://koa.bootcss.com/) 基于Node.js平台的下一代web开发框架，koa 是由 Express 原班人马打造的。

- [yarn](https://yarn.bootcss.com/) Yarn 缓存了每个下载过的包，所以再次使用时无需重复下载。 同时利用并行下载以最大化资源利用率，因此安装速度更快

## 参考

- https://koa.bootcss.com/
- http://expressjs.com/zh-cn/
- http://www.runoob.com/nodejs/nodejs-tutorial.html

## Q & A



