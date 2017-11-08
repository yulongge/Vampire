# Vue

## 简介

Vue(/vju:/, 类似于View)，一套构建用户界面的渐进式框架。采用`自底向上增量开发`的设计。它只关注视图层，容易上手，便于与第三方库整合。

> 与单文件组件和Vue生态系统支持的库结合使用时，Vue也完全能够为复杂的单页应用程序提供有力驱动

## 安装

Vue.js不支持IE8及其以下版本，因为Vue.js使用的ECMAScript5的特性在IE8下无法模拟。Vue支持所有兼容ECMAScript的浏览器。

http://caniuse.com/#feat=es5

- 直接引入`<script>`标签
	
	> Vue会被注册一个全局变量。在开发环境中引入开发环境版本，包含完整的警告和调试模式，对开发更加友好!
	> CDN
	> - [unpkg](https://unpkg.com/vue) : npm 发布后立即同步
	> - [jsDelivr](https://cdn.jsdelivr.net/npm/vue/dist/vue.js) : npm发布后需要一段时间才能同步，所以可能无法获取最新版本 
	> - [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/2.4.0/vue.js) : 同jsDelivr

- NPM

	在用Vue构建大型应用程序时，推荐使用NPM安装方式，可以很好地与模块打包器(webpack/browserify)配合使用。

	```shell
	npm install vue
	```

在NPM包的dist/目录下，你会找到许多不同的构建版本的Vue.js.

||UMD|CommonJS|ES Module|
|----|----|----|---|
|完整版本(Full)|vue.js|vue.common.js|vue.esm.js|
|只含有运行时版本(Runtime-only)|vue.runtime.js|vue.runtime.common.js|vue.runtime.esm.js|
|完整版本(生产环境)|vue.min.js|||
|只含有运行时版本(生产环境)|vue.runtime.min.js|||

> - 完整版本(Full)：包含编译器(compiler)和运行时(runtime)的构建版本。
> - 编译器(Compiler)：负责将模板字符串编译成 JavaScript 渲染函数(render function)的代码。
> - 运行时(Runtime)：负责创建 Vue 实例(creating Vue instances)、渲染(rendering)和修补虚拟 DOM(patching virtual DOM) 等的代码。基本上，等同于完整版本减去编译器。
> - UMD：UMD 构建版本能够直接在浏览器中通过 `<script>` 标签使用。Unpkg CDN 提供的默认文件 https://unpkg.com/vue，是运行时+编译器(Runtime + Compiler)的 UMD 构建版本。
> - CommonJS：CommonJS 版本用于较早期的打包器(bundler)（例如 browserify 或 webpack 1 等）中。用于这些打包器的默认文件(pkg.main)，是只含有运行时(Runtime only)的 CommonJS 构建版本(vue.runtime.common.js)
> - ES Module：ES 模块版本构建用于现代打包器（例如 webpack 2 或 rollup 等）中。用于这些打包器的默认文件(pkg.module)，是只含有运行时(Runtime only)的 ES Module 构建版本(vue.runtime.esm.js)。


- Bower
	
	```shell
	bower install vue
	```


### 命令行接口工具

Vue.js 提供了一个命令行接口工具，用于快速搭建大型单页面应用程序。能够为现代前端开发工程师流程，带来持久强力的基础框架，只需要几分钟，就可以建立并运行一个带有: 热重载，保存时代码检查以及可直接用于生产环境的构建配置的项目.

```shell
# 安装 vue-cli
npm install --global vue-cli
# 使用 "webpack" 模板创建一个新项目
vue init webpack my-project
# 安装依赖，然后开始！
cd my-project
npm install
npm run dev
```

> 对于初学者来说，不建议使用cli工具，影响我们学习，对于老手来说，也许我们也不屑于使用这个，当然我们都是很有自信的。



## 语法

> Vue实例的创建，语法，事件处理

### Vue实例

> 每个Vue应用都是通过Vue函数创建一个新的Vue实例开始的.

```js
var vm = new Vue({
	//选项
})
```

这样就创建了一个实例，创建时你可以传入一个选项对象。

#### 实例的生命的周期

![vue-lifecycle](https://yulongge.github.io/images/vue/vue_lifecycle.png)

如果熟悉react的话可以对比一下:

![react-lifecycle](https://yulongge.github.io/ppt/img/react.png)

### 模板语法

### 指令

指令(Directives)是带有`v-`前缀的特殊属性，属性值预期是个`单个JavaScript表达式`(`v-for`例外)，职责是，当表达式的值改变时，将其产生连带影响，响应式的作用于DOM.

- v-on
- v-bind
- v-for
- v-if
- v-else
- v-else-if
- v-show
- v-model

#### 自定义指令

```js
//注册全局的
Vue.directive('focus', {
	inserted: function(el) {
		el.focus()
	}
})

//注册局部的
new Vue({
	directive: {
		focus: {
			inserted: function(el) {
				el.focus()
			}
		}
	}
})


//调用
//<input v-focus>
```

#### 指令的钩子函数

- bind
- inserted
- update
- componentUpdated
- unbind

#### 钩子函数参数

- el
- binding
	+ name
	+ value
	+ oldValue
	+ expression
	+ arg
	+ modifiers

- vnode
- oldVnode
	
> 除了el之外，其他的参数都应该是只读的，尽量不要修改他们。

#### 简写

> 大多数情况下，我们可能想在bind 和 update 钩子上做重复动作，并且不想关心其他的钩子函数。

```js
Vue.derective('color-swatch', function(el, binding) {
	el.style.backgroundColor = binding.value
})
```

### Class && Style

> 操作元素的class列表和内联样式是数据绑定的一个常见需求，因为他们都是属性，所以可用`v-bind`处理它们,Vue为了防止字符串拼接带来的麻烦和易错，将v-bind用于class 和 style 时，做了增强，表达式结果的类型除了字符串之外，还可以是对象或数组。

- v-bind:class
- v-bind:style
