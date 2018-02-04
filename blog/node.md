# 温故而知新,Nodejs的陈年旧事

![node](https://yulongge.github.io/images/nodejs/nodejs.jpg)

## 简介

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。 

Node.js 的包管理器 npm，是全球最大的开源库生态系统。

私下了解：

- V8引擎(浏览器都有哪些引擎)
- 脚本语言
- 事件驱动
- I/O模型
- NPM

> Everything runs in parallel except your code!

JS是脚本语言，脚本语言都需要一个解析器才能运行。对于写在HTML页面里的JS，浏览器充当了解析器的角色。而对于需要独立运行的JS，NodeJS就是一个解析器。

每一种解析器都是一个运行环境，不但允许JS定义各种数据结构，进行各种计算，还允许JS使用运行环境提供的内置对象和方法做一些事情。例如运行在浏览器中的JS的用途是操作DOM，浏览器就提供了document之类的内置对象。而运行在NodeJS中的JS的用途是操作磁盘文件或搭建HTTP服务器，NodeJS就相应提供了fs、http等内置对象。

NodeJS的作者(Ryan Dahl)说，他创造NodeJS的目的是为了实现高性能Web服务器，他首先看重的是事件机制和异步IO模型的优越性，而不是JS。但是他需要选择一种编程语言实现他的想法，这种编程语言不能自带IO功能，并且需要能良好支持事件机制。JS没有自带IO功能，天生就用于处理浏览器中的DOM事件，并且拥有一大群程序员，因此就成为了天然的选择。

对于前端而言，虽然不是人人都要拿NodeJS写一个服务器程序，但简单可至使用命令交互模式调试JS代码片段，复杂可至编写工具提升工作效率。NodeJS生态圈正欣欣向荣

## 安装

![nodejs](https://yulongge.github.io/images/nodejs/download.png)

基于我们在用windows 和 mac ，所以介绍这两种操作系统的安装配置。

### window

下载 Windows Installer (.msi)

### mac

`homebrew`
```js
brew install node
```

`下载包`

macOS Installer (.pkg)	


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


## npm

NPM 是随同Nodejs 一起安装的包管理工具,能解决Nodejsd代码部署上的很多问题.

用法：

- 允许用户从npm服务器下载别人编写的第三方包到本地使用
- 允许用户从npm服务器下载并安装别人编写的命令行程序到本地使用
- 允许用户将自己编写的包或命令行程序上传到npm服务器供别人使用

> 新版的nodejs已经集成了npm,所以npm一并安装好了,可以用 `npm -v` 来检测是否安装成功。

### npm 常用命令

```js
npm install 安装模块
npm uninstall 卸载模块
npm update 更新模块
npm outdated 检查模块是否已经过时
npm ls 查看安装的模块
npm init 在项目中引导创建一个package.json文件
npm help 查看某条命令的详细帮助
npm root 查看包的安装路径
npm config 管理npm的配置路径
npm cache 管理模块的缓存
npm start 启动模块
npm stop 停止模块
npm restart 重新启动模块
npm test 测试模块
npm version 查看模块版本
npm view 查看模块的注册信息
npm adduser  用户登录
npm publish 发布模块
npm access 在发布的包上设置访问级别
npm package.json的语法
```
具体文档参考:

https://github.com/yulongge/Vampire/blob/master/blog/npm_command.md


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

每个项目的根目录下面，一般都有一个package.json文件，定义了这个项目所需要的各个模块，已经项目的配置信息，`npm install` 命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境

package.json 文件就是一个json文件，该对象的每一个成员就是当前项目的一项设置。

package.json 文件可以手工编写，也可以使用`npm init`命令自动生成。

```js
npm init
```
package.json中各个属性的意义：

https://github.com/yulongge/Vampire/blob/master/blog/package.json.md

### shrinkwrap

管理依赖是一个复杂的软件开发过程中必会遇到的问题。

在Node.js项目开发的时候，我们也经常需要安装和升级对应的依赖。虽然 npm 以及语意化的版本号 (semantic versioning, semver) 让开发过程中依赖的获取和升级变得非常容易， 但不严格的版本号限制，也带来了版本号的不确定性.

npm shrinkwrap 可以按照当前项目 node_modules 目录内的安装包情况生成稳定的版本号描述

```json
//A
{
   "name": "A",
   "version": "0.1.0",
   "dependencies": {
	 "B": "<0.1.0"
   }
}

//B

{
   "name": "B",
   "version": "0.0.1",
   "dependencies": {
	 "C": "<0.1.0"
   }
}

//C
{
   "name": "C",
   "version": "0.0.1"
}
```
你的项目只依赖于A，于是npm install会得到这样的目录结构。

```js
A@0.1.0
     `-- B@0.0.1
         `-- C@0.0.1
```

如果这时候B发布了新版本，你再执行npm install的时候。

```js
A@0.1.0
     `-- B@0.0.2
         `-- C@0.0.1
```

所以我们需要锁定版本，保证所在的环境下安装得到稳定的结果。

我们需要手动的执行`npm shrinkwrap`可以在项目中得到一个npm-shrinkwarp.json文件

```json
{
  "name": "A",
  "version": "0.1.0",
  "dependencies": {
    "B": {
      "version": "0.0.1",
      "dependencies": {
        "C": {
          "version": "0.0.1"
        }
      }
    }
  }
}
```

shrinkwarp命令会根据目前安装在node_modules的文件情况锁定依赖版本，在项目中执行`npm install`的时候，npm 会检查在根目录下有没有`npm-shrinkwrap.json`文件，如果有，则使用它来确定安装各个包的版本号信息。


## REPL

Node REPL(Read Eval Print Loop:读取-求值-输出-循环)：交互式解析器

### 使用说明
在终端输入node,就会进入REPL

```
node
>
```

![REPL](http://www.runoob.com/wp-content/uploads/2015/09/nodejs-gif2.gif)

- 简单表达式运算
- 使用变量
- 多行表达式
- 下划线(_)变量
- REPL命令


## 语法

官网的文档很全，不需要多说什么

http://nodejs.cn/api/

### 全局对象

JavaScript 中有一个特殊的对象，称为全局对象(Global Object),它及其所有的属性都可以在程序的任何地方访问，即全局变量。

所有的全局变量都是global对象的属性，global最根本的作用是作为全局变量的宿主。
		
在node.js中不会有全局变量，因为用户代码都是属于当前模块的。。

#### 全局变量

- __filename: 指向当前运行的脚本文件名。
- __dirname: 指向当前运行脚本所在的目录。
- process: 该对象表示node所在的当前进程，允许开发者与进程互动
  通常在写本地命令程序的时候，少不了要和它打交道。
- console: 指向node内置的console模块，提供命令行运行环境中的标准输入，输出功能,习惯行为跟浏览器的实施标准调试工具的console一致。


#### 全局函数

- setTimeout()
- clearTimeout()
- setInterval()
- clearInterval()
- require()
- Buffer()
- module
- module.exports

### 模块

Node.js采用模块化结构，按照CommonJs规范定义和使用模块。模块与文件是一一对应关系，即加载一个模块，实际上就是加载对应的一个模块文件。

requre命令用于指定加载模块，加载时可以省略脚本文件的后缀名。

```js
var server = require('./server.js');
//或
var server1 = require('./server'); 
```
require 方法参数：

- 参数中含有文件路径，这时路径是相对于当前脚本所在的目录
- 参数中不含路径，这时Node到模块的安装目录，去找已安装的模块

```js
var bar = require('bar');
```
有时候，一个模块本身就是一个目录，目录中包含多个文件，这时候，Node在package.json文件中，寻找main属性所指明的模块入口文件。

```js
{
	"name": "bar",
	"main": "./lib/bar.js"
}
```

```js
//等同于
var bar = rquire('bar/lib/bar.js');
```

如果模块目录中没有package.json文件，node.js会尝试在模块目录中找index.js,或index.node文件进行加载。

模块一旦被加载以后，就会被系统缓存。如果第二次加载该模块，则会返回缓存中的版本，这意味着模块实际上只会执行一次。如果希望模块执行多次，则可以让模块返回一个函数，然后多次调用。

### 核心模块

如果只是在服务器运行JavaScript代码，用处并不大，因为服务器脚本语言已经有很多种，Node.js的用处在于，它本身还提供了一系列功能模块，与操作系统互动。这些核心的功能模块，不用安装就可以使用。

- http: 提供HTTp服务功能
- url: 解析URL
- fs: 与文件系统交互
- querystring: 解析URL的查询字符串。
- child_process: 新建子进程
- util: 提供一系列使用工具
- path: 处理文件路径
- crypto: 提供加密和解密功能，基本上是对OpenSSL的包装。


核心模块都在Node的lib子目录中，为了提高运行速度，他们安装时都会被编译成二进制文件，核心模块总是最优先加载的，如果你自己写了一个HTTP模块，require('http')加载的还是核心模块。

### 网络操作

不了解网络编程的程序员不是好前端，而NodeJS恰好提供了一扇了解网络编程的窗口。通过NodeJS，除了可以编写一些服务端程序来协助前端开发和测试外，还能够学习一些HTTP协议与Socket协议的相关知识，这些知识在优化前端性能和排查前端故障时说不定能派上用场。
- http
- https

```js
var http = require('http');

http.createServer(function (request, response) {
  response.writeHead(200, { 'Content-Type': 'text-plain' });
  response.end('Hello World\n');
}).listen(8080);

```

```js
var express = require('express');
var app = express();

app.get('/', function (req, res) {
res.send('Hello World');
})

var server = app.listen(8081, function () {

  ...

})
```

### 文件操作(fs)

让前端觉得如获神器的不是NodeJS能做网络编程，而是NodeJS能够操作文件。小至文件查找，大至代码编译，几乎没有一个前端工具不操作文件。换个角度讲，几乎也只需要一些数据处理逻辑，再加上一些文件操作，就能够编写出大多数前端工具。

```js
fs.open(path, flags[, mode], callback) //打开文件
fs.stat(path, callback) //获取文件信息
fs.writeFile(file, data[, options], callback) //写入文件
fs.read(fd, buffer, offset, length, position, callback) //写入文件
fs.close(fd, callback) //关闭文件
```

```js
var fs = require("fs");

// 异步读取
fs.readFile('input.txt', function (err, data) {
if (err) {
  return console.error(err);
}
console.log("异步读取: " + data.toString());
});

// 同步读取
var data = fs.readFileSync('input.txt');
console.log("同步读取: " + data.toString());

console.log("程序执行完毕。");
```

### 工具模块

在 Node.js 模块库中有很多好用的模块。接下来我们为大家介绍几种常用模块的使用。

- Util 是一个Node.js 核心模块，提供常用函数的集合，用于弥补核心JavaScript 的功能 过于精简的不足
- OS 模块 提供基本的系统操作函数。
- Path 模块 提供了处理和转换文件路径的工具
- Net 模块 用于底层的网络通信。提供了服务端和客户端的的操作
- DNS 模块 用于解析域名

### 其他

- 多进程
- 数据库链接

## 衍生

- [express](http://expressjs.com/zh-cn/) 是一个简洁而灵活的 node.js Web应用框架
- [koa](https://koa.bootcss.com/) 基于Node.js平台的下一代web开发框架，koa 是由 Express 原班人马打造的。
- [yarn](https://yarn.bootcss.com/) Yarn 缓存了每个下载过的包，所以再次使用时无需重复下载。 同时利用并行下载以最大化资源利用率，因此安装速度更快

## 参考

- https://koa.bootcss.com/
- http://expressjs.com/zh-cn/
- http://www.runoob.com/nodejs/nodejs-tutorial.html
- http://nodejs.cn/api
- https://github.com/luohaha/Chinese-uvbook/blob/master/source/introduction.md






