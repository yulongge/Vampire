## Parcel

去年的12月份，发布了Parcel

横空出世的`Parcel`，这几个月来成为了前端圈的又一大热点，github短短几个月就获得了上万的star.

[作者发布声明](https://medium.com/@devongovett/announcing-parcel-a-blazing-fast-zero-configuration-web-application-bundler-feac43aac0f1)

为了解决我在 `Browserify` 和 `Webpack` 等现有模块打包工具中遇到的两个主要问题：`性能和配置经验`，作者开始研究 `Parcel`

`作者`: `devongovett`
`github`: [Parcel](https://github.com/parcel-bundler/parcel)
`medium`: [medium](https://medium.com/@devongovett)
`api`: [api](https://parceljs.org/)

## 简介

Parcel 是一个Web应用程序 打包器(bundler) ，与以往的开发人员使用的打包器有所不同。它利用多核处理提供极快的性能，并且你不需要进行任何配置。

快速，零配置的Web应用程序打包器。

### 特性

- 🚀 非常快的打包时间 - 多核编译，以及文件系统缓存，这样即使在重新启动后也能快速重建。
- 📦 对于 JS, CSS, HTML, 图片以及文件资源以及其它支持开箱即用，不需要安装插件。
- 🐠 在需要时使用 Babel，PostCSS 和 PostHTML 自动转换模块 - 甚至是node_modules。
- ✂️ 零配置代码分割使用动态import() 语句。
- 🔥 内置支持热加载
- 🚨 友好的错误日志体验 - 语法高亮显示的代码帧有助于查明问题。

## 安装

可以使用`Yarn` 或 `npm`安装 `Parcel`

```shell
//Yarn:

yarn global add parcel-bundler
//npm:

npm install -g parcel-bundler

npm init -y //生成一个配置文件package.json,加入参数y表示生成默认配置。
```

## 初窥

Parcel 可以将任何类型的文件作为 入口点(entry point) ，但是 HTML 或 JavaScript 文件是一个很好的开始。如果你使用相对路径将你的主 JavaScript 文件链接到 HTML 中，Parcel 也会为你处理，并将该引用替换为输出文件的 URL 。


接下来，创建一个 index.html 和 index.js 文件

```html
<html>
<body>
  <script src="./index.js"></script>
</body>
</html>
```
```js
console.log("hello world!");
```

`Parcel` 内置了一个开发服务器，这会在你更改文件时自动重建你的应用程序，并支持`模块热替换`，以便你快速开发，你只需要制定入口文件即可：

```shell
parcel index.html
````

使用`-p <port number>`选项覆盖默认端口。

```shell
parcel watch index.html
```

## 资源

Parcel 基于资源的。资源可以代表任何文件，但Parcel 对JavaScript, css, HTML文件等特定类型的资源有特殊的支持。Parcel自动分析这些文件中引用的依赖关系，并将其包含到输出包中(output bundle).相似类型的资源被组合在一起成为相同的输出包。如果您导入不同类型的资源(例如，如果在js中导入一个css文件)， 它新建一个子包，并在父级中保留一个引用。

### JavaScript

Web 打包器(bundler)最传统的文件类型是JavaScript。Parcel支持CommonJS和ES6模块语法来导入文件。它还支持动态`import()`函数语法来异步加载模块。

```js
//使用CommonJS语法导入模块
const dep = require('./path/to/dep');

// 使用ES6 import语法导入模块

import dep from './path/to/dep';
```

你也可以在JavaScript文件导入非`JavaScript`资源，例如css,甚至图像文件。当您导入其中一个文件，它不像其他一些打包器(bundler)一样内敛的。相反，它及其所有依赖项都被放置在一个单独的包(bundle)，例如一个css文件中。当使用`css modules`时，导出的类被放置在JavaScript包中。其他资源类型将导出一个URL到JavaScript包的输出文件中，所以你可以在你的代码中引用他们。

```js
import './test.css';
import classNames from './test.css';
import imageURL from '.test.png';
```

### CSS

CSS 资源 可以在 JavaScript 或 HTML 文件导入，并且可以通过 @import 语法包含引用的依赖关系，以及通过 url() 函数引用图像，字体等。其他的 @import 的 CSS 文件被内联到同一个 CSS 包中，并且 url() 引用被重写为它们的输出文件名。所有的文件名应该是相对于当前的 CSS 文件。

```css
/* 导入另一个 CSS 文件 */
@import './other.css';

.test {
  /* 引用一个 image 文件 */
  background: url('./images/background.png');
}
```

除了纯 CSS ，还支持其他编译成 CSS 语言，如 LESS ，SASS 和 Stylus ，并以相同的方式工作。

### SCSS

SCSS编译需要 node-sass 模块。可以用 npm 来安装它：

npm install node-sass
一旦 node-sass 安装完成，你就可以在 JavaScript 文件中导入 SCSS 文件。

import './custom.scss'
SCSS 文件中的依赖可以使用 @import 语句。

### HTML

HTML 资源通常是你提供给 Parcel 的入口文件，但也可以被 JavaScript 文件引用，例如，提供其他网页的链接。脚本，样式，媒体和其他 HTML 文件的 URL 被提取和编译，如上所述。引用被重写到 HTML 中，以便它们链接到正确的输出文件。所有的文件名应该是相对于当前的 HTML 文件

```html
<html>
<body>
  <!-- 引用一个 image 文件 -->
  < img src="./images/header.png">

  <a href=" ">Link to another page</a >

  <!-- 导入一个 JavaScript 包 -->
  <script src="./index.js"></script>
</body>
</html>
```

## 转换
尽管许多 打包器(bundler) 都要求你安装和配置插件来转换资源，Parcel 内置许多长江的转换和转译器，让你开箱即用。你可以使用 Babel 转换 JavaScript，CSS 使用 PostCSS ，HTML 使用 PostHTML 。当在模块中找到配置文件（例如 .babelrc ，.postcssrc ）时， Parcel 会自动运行这些转换。

这甚至可以在第三方 node_modules 中工作：如果配置文件是作为包的一部分发布的，转换会自动打开，且仅适用于该模块。由于只处理需要转换的模块，因此可以快速打包。这也意味着您不需要手动配置转换来包含和排除某些文件，或者知道第三方代码是如何构建的，以便在你的应用程序中使用它。

### Babel

Babel 是一个流行的 JavaScript 转译器，拥有大量的插件生态系统。在 Parcel 中使用 Babel 的方式与其单独使用或与其他打包器配合使用的方式相同。

在你的应用程序中安装预设和插件：

```shell
yarn add babel-preset-env
```

然后，创建一个 .babelrc 文件：

```json
{
  "presets": ["env"]
}
```

### PostCSS

PostCSS 是一个用插件转换 CSS 的工具，比如 autoprefixer, cssnext, 和 CSS Modules。 您可以使用以下名称之一创建配置文件，从而使 Parcel 使用 PostCSS 配置 ： .postcssrc (JSON), .postcssrc.js, 或者 postcss.config.js.

在你的应用程序中安装插件:

```shell
yarn add postcss-modules autoprefixer
```

然后，创建一个 .postcssrc 文件

```json
{
  "modules": true,
  "plugins": {
    "autoprefixer": {
      "grid": true
    }
  }
}
```

插件指定在 plugins 对象的 key 中，并选项定义使用对象值。 如果插件没有选项，只需将其设置为 true 即可。

Autoprefixer ， cssnext 和其他工具的目标浏览器可以在 .browserslistrc 文件中指定：

```shell
> 1%
last 2 versions
```

CSS Modules 的启用方式稍有不同，在顶级 modules key 上使用。这是因为 Parcel 需要对 CSS Modules 有特殊的支持，因为它们也会导出一个对象，包含到 JavaScript 包中。请注意，你仍然需要在你的项目中安装 postcss-modules

### PostHTML

PostHTML 是一个用插件转换 HTML 的工具。您可以使用以下名称之一创建配置文件，从而使 Parcel 使用 PostHTML 配置 ：.posthtmlrc (JSON), posthtmlrc.js, 或者 posthtml.config.js.

在你的应用程序中安装插件：

```shell
yarn add posthtml-img-autosize
```

然后，创建一个 .posthtmlrc 文件：

```json
{
  "plugins": {
    "posthtml-img-autosize": {
      "root": "./images"
    }
  }
}
```

插件指定在 plugins 对象的 key 中，并选项定义使用对象值。 如果插件没有选项，只需将其设置为 true 即可。

### TypeScript

TypeScript 是 JavaScript 类型的超集，可以编译成普通的JavaScript，它也支持现代的 ES2015+ 特性。 无需任何额外的配置即可转换 TypeScript 

```html
<!-- index.html -->
<html>
<body>
  <script src="./index.ts"></script>
</body>
</html>
```

```js
// index.ts
import message from "./message";
console.log(message);
```

```js
// message.ts
export default "Hello, world";
```

## 代码拆分(Code Splitting)
Parcel 支持零配置代码拆分，开箱即用。这使您可以将你的应用程序代码拆分为可以按需加载的独立包，这意味着更小的初始包大小和更快的加载时间。 当用户在应用程序中浏览模块并需要加载时，Parcel 会自动负责按需加载子包。

代码拆分是通过使用动态的import() 函数的 语法提案 来控制的，它的工作方式与普通的 import 语句或 require 函数类似，但返回一个 Promise 。 这意味着模块是异步加载的。

以下示例显示如何使用动态导入来按需加载应用程序的子页面。

## 模块热替换(Hot Module Replacement)

模块热替换（HMR）通过在运行时自动更新浏览器中的模块，而不需要刷新整个页面来改进开发体验。 这意味着应用程序状态可以在小的更改时保留。 Parcel 的 HMR 实现支持开箱即用的JavaScript 和 CSS 资源。 在生产模式下打包时，HMR 自动被禁用。

在保存文件时，Parcel 会重建所更改的内容，并将更新发送到包含新代码的任何正在运行的客户端。 新的代码会替换旧版本，并与所有的父级资源一起重新计算。 你可以使用 module.hot API 挂接到这个过程中，这个API可以在一个模块即将被丢弃时或者当一个新版本进入时通知你的代码。像 react-hot-loader 这样的项目可以帮助你完成该过程，并通过 Parcel 开箱即用。

有两种已知的方法：module.hot.accept 和 module.hot.dispose 。你可以在module.hot.accept中使用回调函数，该函数在模块或其任何依赖项被更新时执行。 当该模块即将被替换时，module.hot.dispose 回调函数会被调用。

```js
if (module.hot) {
  module.hot.dispose(function () {
    // 模块即将被替换时
  });

  module.hot.accept(function () {
    // 模块或其依赖项之一刚刚更新时
  });
}
```

## 生产环境

当需要打包应用程序用于生产环境时，可以使用 Parcel 的生产模式

```js
parcel build entry.js
```

这将禁用 监听(watch) 模式和模块热更换，所以它只会构建一次。它还会开启 minifier 用于压缩输出包文件的大小。Parcel使用的 minifiers 包括用于 JavaScript 的 uglify-es，用于 CSS 的 cssnano，和用于 HTML 的 htmlnano

启用生产模式还需要设置 NODE_ENV = production 环境变量。 像 React 这样的大型库有开发调试功能，通过设置这个环境变量来禁用调试功能，从而使生产的构建更小更快。

### config

- 设置输出目录

  default: 'dist'

  ```shell
  parcel build entry.js --out-dir build/output
  或者
  parcel build entry.js -d build/output

  root
  - build
  - - output
  - - - entry.js
  ```
- 设置要提供服务的公共 URL

  默认: --out-dir option

  ```shell
  parcel build entry.js --public-url ./
  ```
  ```html
  <link rel="stylesheet" type="text/css" href="1a2b3c4d.css">
  <script src="e5f6g7h8.js"></script>
  ```

- 禁用压缩

  默认: minification enabled

  ```shell
  parcel build entry.js --no-minify
  ```

- 禁用文件系统缓存

  默认: cache enabled

  ```shell
  parcel build entry.js --no-cache
  ```
  

## 配置

```json
// .babelrc
{
  "presets": ["env", "react"]
}

{
  "presets": ["env", "preact"]
}
```

## VS Webpack

Parcel 能做到无配置完成以上项目构建要求；
Parcel 内置了常见场景的构建方案及其依赖，无需再安装各种依赖；
Parcel 能以 HTML 为入口，自动检测和打包依赖资源；
Parcel 默认支持模块热替换，真正的开箱即用；

而反观 Webpack，比 Parcel 要麻烦很多：

需要写一堆配置；
需要再安装一堆依赖；
不能简单的自动生成 HTML；

Parcel 还需要时间去打磨

不支持 SourceMap ：在开发模式下，Parcel 也不会输出 SourceMap，目前只能去调试可读性极低的代码；
不支持剔除无效代码 ( TreeShaking )：很多时候我们只用到了库中的一个函数，结果 Parcel 把整个库都打包了进来；
一些依赖会 让Parcel 出错：当你的项目依赖了一些 Npm 上的模块时，有些 Npm 模块会让 Parcel 运行错误；

Parcel 需要为零配置付出代价

不守规矩的 node_module 
不灵活的配置

Parcel 使用场景受限

目前 Parcel 只能用来构建用于运行在浏览器中的网页，这也是他的出发点和专注点。

在软件行业不可能存在即使用简单又可以适应各种场景的方案，就算所谓的人工智能也许能解决这个问题，但人工智能不能保证 100% 的正确性。

反观 Webpack 除了用于构建网页，还可以做：

打包发布到 Npm 上的库
构建 Node.js 应用 ( 同构应用 )
构建 Electron 应用
构建离线应用 ( ServiceWorkers )

构建速度和输出文件大小对比

数据项	Parcel	Webpack
生成环境构建时间	8.310s	9.58s
开发环境启动时间	5.42s	8.06s
监听变化构建时间	3.17s	2.87s
生成环境输出 JS 文件大小	544K	274K
生成环境输出 CSS 文件大小	23K	23K

从以上数据可以看出： Parcel 构建速度快，但 Parcel 输出文件大

导致 Parcel 构建速度快的原因和 iOS 比 Android 用起来更流畅的原因类似：

Parcel 因为一体化内置，所以集成和优化的更好，而 Webpack 通过插件和 Loader 机制去让第三方扩展这会拉低性能；
Parcel 内置多进程并行构建，而 Webpack 默认是单进程构建 ( Webpack 也支持多进程 )；

导致 Parcel 输出 JS 文件大的原因在于：

不支持 TreeShaking
构建出的 JS 中出现了所有文件的名称，如图：

## 参考

http://www.css88.com/doc/parcel/getting_started.html

https://segmentfault.com/a/1190000012612891