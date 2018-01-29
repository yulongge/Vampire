### npm command

NPM的全称是Node Package Manager，是随同NodeJS一起安装的包管理和分发工具，它很方便让JavaScript开发者下载、安装、上传以及管理已经安装的包

- npm install 安装模块

```shell
npm install gulp
npm install gulp -g
```

- npm uninstall 卸载模块

```shell
npm uninstall gulp --save-dev
```

- npm update 更新模块

```shell
npm update [-g] [<pkg>...]
```

- npm outdated 检查模块是否已经过时

```shell
npm outdated [[<@scope>/]<pkg> ...]
```

- npm ls 查看安装的模块


- npm init 在项目中引导创建一个package.json文件
- npm help 查看某条命令的详细帮助
- npm root 查看包的安装路径
- npm config 管理npm的配置路径
- npm cache 管理模块的缓存
- npm start 启动模块
- npm stop 停止模块
- npm restart 重新启动模块
- npm test 测试模块
- npm version 查看模块版本
- npm view 查看模块的注册信息
- npm adduser  用户登录
- npm publish 发布模块
- npm access 在发布的包上设置访问级别