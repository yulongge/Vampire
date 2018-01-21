# Nodejs

## 简介

Node.js就是运行在服务器端的JavaScript，是一个**事件驱动I/O服务端** JavaScript环境，基于Google的V8引擎。

## 安装

基于我们在用windows 和 mac ，所以介绍这两种操作系统的安装配置。

![nodejs](https://yulongge.github.io/images/nodejs/download.png)

### window

### mac

## npm

NPM 是随同Nodejs 一起安装的包管理工具,能解决Nodejsd代码部署上的很多问题.

用法：

- 允许用户从npm服务器下载别人编写的第三方包到本地使用
- 允许用户从npm服务器下载并安装别人编写的命令行程序到本地使用
- 允许用户将自己编写的包或命令行程序上传到npm服务器供别人使用

> 新版的nodejs已经集成了npm,所以npm一并安装好了,可以用 `npm -v` 来检测是否安装成功。

### npm 常用命令

### npm 镜像(淘宝)

### package.json

### 版本控制

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

### nvm

nvm全称Node Version Manager，它与n的实现方式不同，其是通过shell脚本实现的

nvm 不支持windows

nvm-windows 适用于windows