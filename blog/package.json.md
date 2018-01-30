package.json

# 概述

每个项目的根目录下面，一般都有一个package.json文件，定义了这个项目所需要的各个模块，已经项目的配置信息，`npm install` 命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境

package.json 文件就是一个json文件，该对象的每一个成员就是当前项目的一项设置。

package.json 文件可以手工编写，也可以使用`npm init`命令自动生成。

```js
npm init
```

完整的文件属性：

```json
"name": "Hello World",
"version": "0.0.1",
"author": "张三",
"description": "第一个node.js程序",
"keywords":["node.js","javascript"],
"repository": {
	"type": "git",
	"url": "https://path/to/url"
},
"license":"MIT",
"engines": {"node": "0.10.x"},
"bugs":{"url":"http://path/to/bug","email":"bug@example.com"},
"contributors":[{"name":"李四","email":"lisi@example.com"}],
"scripts": {
	"start": "node index.js"
},
"dependencies": {
	"express": "latest",
	"mongoose": "~3.8.3",
	"handlebars-runtime": "~1.0.12",
	"express3-handlebars": "~0.5.0",
	"MD5": "~1.2.0"
},
"devDependencies": {
	"bower": "~1.2.8",
	"grunt": "~0.4.1",
	"grunt-contrib-concat": "~0.3.0",
	"grunt-contrib-jshint": "~0.7.2",
	"grunt-contrib-uglify": "~0.2.7",
	"grunt-contrib-clean": "~0.5.0",
	"browserify": "2.36.1",
	"grunt-browserify": "~1.3.0",
}
```

# 属性介绍

## scripts

`scripts` 指定了运行脚本命令的npm 命令行缩写。

```json
"script": {
	"start": "cross-env NODE_ENV=development webpack-dev-server",
    "mock": "nodemon ./mock.server.js",
    "build": "npm run eslint && cross-env NODE_ENV=production PLATFORM=web webpack --color",
    "preview": "nodemon ./preview.server.js",
    "eslint": "eslint src/ --ext .jsx,.js",
    "test": "echo \"Error: no test specified\" && exit 1",
    "lint": "jshint .",
    "validate": "npm ls"
}
```

## dependencies, devDependencies 字段

`dependencies`指定了项目运行所依赖的模块，`devDependencies`指定了项目开发所需的模块。
他们都指向一个对象。该对象的各个成员，分别由模块和对应的版本要求组成，表示依赖的模块极其版本范围。

```json
"devDependencies": {
    "autoprefixer": "^6.7.7",
    "babel-core": "^6.24.0",
    "babel-eslint": "^7.2.3",
    "babel-loader": "^6.4.1",
	...
}
```
对应的版本可以加上各种限定

- 制定版本: 比如`1.2.2`，遵循"大版本，次版本，小版本"的格式规定，安装时只安装指定版本。
- 波浪号 + 指定版本: 比如`~1.2.2`,表示安装1.2.x的最新版本(不低于1.2.2),但是不安装1.3.x,也就是说安装时不改变大版本和次版本号。
- 插入号 + 指定版本: 比如`^1.2.2`，表示安装1.x.x的最新版本(不低于1.2.2), 但是不安装2.x.x,也就是说安装时不改变大版本号。如果大版本为0，则插入号的行为与波浪号相同。
- latest: 安装最新版本

```js
npm install express --save //写入dependencies
npm install express --save-dev //写入devDependencies
```

### bin 字段

bin项用来指定各个内部命令对应的可执行文件的位置。

```js
"bin": {
	"someTool": "./bin/someTool.js"
}
```

### mian字段

main字段指定了加载的入口文件，require('moduleName')就会加载这个文件。这个字段的默认值是模块根目录下面的index.js。

### config 字段

config字段用于添加命令行的环境变量。

```json
{
  "name" : "foo",
  "config" : { "port" : "8080" },
  "scripts" : { "start" : "node server.js" }
}
```

然后，在server.js脚本就可以引用config字段的值

```js
http.createServer(...).listen(process.env.npm_package_config_port);

npm run start

//npm config set foo:port 8080
```

### browser字段

browser指定该模板供浏览器使用的版本。Browserify这样的浏览器打包工具，通过它就知道该打包那个文件

```json
"browser": {
  "tipso": "./node_modules/tipso/src/tipso.js"
},
```

### engines 字段

### man 字段

### preferGlobal字段

### style字段

parcelify
