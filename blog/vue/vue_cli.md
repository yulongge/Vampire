### Vue CLI3

#### 快速原型开发

##### vue server

你可以使用 `vue serve` 和 `vue build` 命令对单个 `*.vue` 文件进行快速原型开发

```sh
npm install -g @vue/cli-service-global
```
> `vue serve` 的缺点就是它需要安装全局依赖，这使得它在不同机器上的一致性不能得到保证。因此这只适用于快速原型开发

vue serve 使用了和 vue create 创建的项目相同的默认设置 (webpack、Babel、PostCSS 和 ESLint)。它会在当前目录自动推导入口文件——入口可以是 main.js、index.js、App.vue 或 app.vue 中的一个。你也可以显式地指定入口文件：

```sh
vue server

Options:

  -o, --open  打开浏览器
  -c, --copy  将本地 URL 复制到剪切板
  -h, --help  输出用法信息

vue server demo.vue
vue server -o demo.vue //直接打开浏览器
```

> 如果需要，你还可以提供一个 index.html、package.json、安装并使用本地依赖、甚至通过相应的配置文件配置 Babel、PostCSS 和 ESLint。

##### vue build

你也可以使用 vue build 将目标文件构建成一个生产环境的包并用来部署

```sh
Options:

  -t, --target <target>  构建目标 (app | lib | wc | wc-async, 默认值：app)
  -n, --name <name>      库的名字或 Web Components 组件的名字 (默认值：入口文件名)
  -d, --dest <dir>       输出目录 (默认值：dist)
  -h, --help             输出用法信息

vue build demo.vue
```

##### vue create

运行以下命令来创建一个新项目

```sh
vue create hello-world
```

你会被提示选取一个 preset。你可以选默认的包含了基本的 Babel + ESLint 设置的 preset，也可以选“手动选择特性”来选取需要的特性

![create](https://cli.vuejs.org/cli-new-project.png)

这个默认的设置非常适合快速创建一个新项目的原型，而手动设置则提供了更多的选项，它们是面向生产的项目更加需要的

![manually](https://cli.vuejs.org/cli-select-features.png)


如果你决定手动选择特性，在操作提示的最后你可以选择将已选项保存为一个将来可复用的 preset。我们会在下一个章节讨论 preset 和插件

> ~/.vuerc 
被保存的 preset 将会存在用户的 home 目录下一个名为 .vuerc 的 JSON 文件里。如果你想要修改被保存的 preset / 选项，可以编辑这个文件

##### vue ui

你也可以通过 vue ui 命令以图形化界面创建和管理项目

![ui](https://cli.vuejs.org/ui-new-project.png)

#### 插件

Vue CLI 使用了一套基于插件的架构。如果你查阅一个新创建项目的 package.json，就会发现依赖都是以 @vue/cli-plugin- 开头的。插件可以修改 webpack 的内部配置，也可以向 vue-cli-service 注入命令。在项目创建的过程中，绝大部分列出的特性都是通过插件来实现的

在现有的项目中安装插件

```sh
vue add @vue/eslint

# 这个和之前的用法等价
vue add @vue/cli-plugin-eslint
```

> vue add 的设计意图是为了安装和调用 Vue CLI 插件。这不意味着替换掉普通的 npm 包。对于这些普通的 npm 包，你仍然需要选用包管理器。

如果不带 @vue 前缀，该命令会换作解析一个 unscoped 的包。例如以下命令会安装第三方插件 vue-cli-plugin-apollo


