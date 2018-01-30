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

```shell
npm ls [[<@scope>/]<pkg> ...]

npm ls -g //查看全局
```


- npm init 在项目中引导创package.json文件
- npm help 查看某条命令的详细帮助

```shell
npm help <term> [<terms..>]
```
例如输入npm help install，系统在默认的浏览器或者默认的编辑器中打开本地nodejs安装包的文件/nodejs/node_modules/npm/html/doc/cli/npm-install.html
```shell
npm help install
```

- npm root 查看包的安装路径

```shell
npm root [-g]
```

- npm config 管理npm的配置路径

基础语法

```shell
npm config set <key> <value> [-g|--global]
npm config get <key>
npm config delete <key>
npm config list
npm config edit
npm get <key>
npm set <key> <value> [-g|--global]
```

对于config这块用得最多应该是设置代理，解决npm安装一些模块失败的问题

例如我在公司内网，因为公司的防火墙原因，无法完成任何模块的安装，这个时候设置代理可以解决

```shell
npm config set proxy=http://dev-proxy.oa.com:8080
```

又如国内的网络环境问题，某官方的IP可能被和谐了，幸好国内有好心人，搭建了镜像，此时我们简单设置镜像

```shell
npm config set registry="http://r.cnpmjs.org"
```
也可以临时配置，如安装淘宝镜像

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

- npm cache 管理模块的缓存

```shell
npm cache add <tarball file>
npm cache add <folder>
npm cache add <tarball url>
npm cache add <name>@<version>

npm cache ls [<path>]

npm cache clean [<path>]
```

- npm start 启动模块

```shell
npm start [-- <args>]

"scripts": {
    "start": "gulp -ws"
}
```

如果package.json文件没有设置start，则将直接启动node server.js

- npm stop 停止模块

```shell
npm stop [-- <args>]
```

- npm restart 重新启动模块

```shell
npm restart [-- <args>]
```

- npm test 测试模块

```shell
npm test [-- <args>]
npm tst [-- <args>]
```

- npm version 查看模块版本

```shell
npm version [<newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease | from-git]

'npm [-v | --version]' to print npm version
'npm view <pkg> version' to view a package's published version
'npm ls' to inspect current package/dependency versions
```

- npm view 查看模块的注册信息

```shell
npm view [<@scope>/]<name>[@<version>] [<field>[.<subfield>]...]

aliases: info, show, v
```

查看模块的依赖关系

```shell
npm view gulp dependencies
```
查看模块的源文件地址

```shell
npm view gulp repository.url
```

查看模块的贡献者，包含邮箱地址

```shell
npm view npm contributors
```

- npm adduser  用户登录

```shell
npm adduser [--registry=url] [--scope=@orgname] [--always-auth]
```

发布模板到npm社区前需要先登录，然后再进入发布的操作

- npm publish 发布模块

```shell
npm publish [<tarball>|<folder>] [--tag <tag>] [--access <public|restricted>]

Publishes '.' if no argument supplied
Sets tag 'latest' if no --tag specified
```

- npm access 在发布的包上设置访问级别

```shell
npm access public [<package>]
npm access restricted [<package>]

npm access grant <read-only|read-write> <scope:team> [<package>]
npm access revoke <scope:team> [<package>]

npm access ls-packages [<user>|<scope>|<scope:team>]
npm access ls-collaborators [<package> [<user>]]
npm access edit [<package>]
```