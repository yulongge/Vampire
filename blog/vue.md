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

- Bower
	
	```shell
	bower install vue
	```

- NPM

	> 在用Vue构建大型应用程序时，推荐使用NPM安装方式，可以很好地与模块打包器(webpack/browserify)配合使用。

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

```html
<div id="app">
  {{ message }}
</div>

```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

这样就创建了一个实例，创建时你可以传入一个选项对象。

#### 实例的生命的周期

![vue-lifecycle](https://yulongge.github.io/images/vue/vue_lifecycle.png)

如果熟悉react的话可以对比一下:

![react-lifecycle](https://yulongge.github.io/ppt/img/react.png)

### 模板语法

Vue使用基于HTML的模板语法，允许开发者声明式的将DOM绑定至底层Vue实例的数据。所有的Vue.js的模板都是合法的HTML,所以能被遵循规范的浏览器和HTML解析器解析。

在底层，Vue将模板编译成虚拟的DOM渲染函数，结合响应系统，在应用状态改变时，Vue能够智能地计算出重新渲染组件的最小代价并应用到DOM操作上。

当然你也可以不用模板，直接写渲染(render)函数，使用可选的JSX语法。

#### 文本

数据绑定使用了`Mustache`语法(双大括号)

```html
<span>Message: {{ msg }}</span>
```
> Mastache 会将数据解释为不同的文本，而非HTML代码，有时候我们想输出正真的HTML,需要使用`v-html`指令

#### 原始HTML

```html
<div v-html="rawHtml"></div>
```

这个div的内容将会被替换称为属性值rawHtml，直接作为HTML一一会忽略属性值中数据的绑定。

> 渲染任何html是很危险的，很容易导致xss攻击

#### 特性

Mustache 语法不能作用在HTML特性上，这时候应该使用v-bind指令

```html
<div v-bind:id="dynamicId"></div>
<div v-bind:disabled="isButtonDisabled">Button</div>
```

> 适用于布尔类型，如果求值结果是`falsy`(falsy不是false)的值，属性将会被删除

#### 使用JavaScript表达式

对于所有的数据绑定，Vue都提供了完全的JavaScript表达式支持

```js
{{ number + 1 }}
{{ ok ? 'yes' : 'no' }}
{{ message.split('').rerverse().join('')}}
```

```html
<div v-bind:id="'list-' + id"></div>
```

> 这些表达式会在所属的Vue实例的数据作用域下作为JavaScript被解析，有个限制就是，每个绑定都只能包含`单个表达式`.

```js
{{ var a = 1 }}
{{ if(ok) { return message } }}
//语句和流控制，都不会生效
```

### 指令

指令(Directives)是带有`v-`前缀的特殊属性，属性值预期是个`单个JavaScript表达式`(`v-for`例外)，职责是，当表达式的值改变时，将其产生连带影响，响应式的作用于DOM.

- v-on
- v-bind
- v-once
- v-for
- v-if
- v-else
- v-else-if
- v-show
- v-model
- v-html

#### 参数

一些指令能够接收一个`参数`, 在指令名称之后以冒号表示.

```html
// href是参数，v-bind将该元素的href属性与表达式url的值绑定.
<a v-bind: href="url">...</a>

//监听事件
<a v-on:click="doSomething">...</a>
```

#### 修饰符

修饰符(Modifiers)是以半角句号`.`指明的特殊后缀，用于指出一个指令该以特殊方式绑定。

```html
//.prevent 修饰符告诉v-on指令对于触发的事件调用`event.preventDefault();`
<form v-on:submit.prevent="onSubmit">...</form>
```

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

### 条件语句

- v-if
- v-else-if
- v-else
- v-show

```html
<h1 v-if="ok">Yes</h1>
<h2 v-else-if="no">No</h2>
<h3 v-else>Don't known</h3>
```

> 因为是指令，所以只能添加到元素上，所以如果想切换多个元素，可以把一个`<template>`元素当做一个不可见的包裹元素，并在上面使用`v-if`.最终渲染结果将不包含`<template>`元素.

```html
<template v-if="ok">
	<h1>....</h1>
	<h1>....</h1>
	<h1>....</h1>
</template>
```

用`key`管理可复用的元素

> Vue会尽可能高效地渲染元素，通常会复用已有的元素而不是从头开始渲染。这么做Vue变得非常快。

```html
<template v-if="loginType === 'username'">
	<label>Username</label>
	<input placeholder="Enter your username">
</template>
<template v-else>
	<label>Email</label>
	<input placeholder="Enter your email">
</template>

```
代码切换loginType将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，`input`不会被替换掉，仅仅替换了它的placeholder.

但是这样也不总是符合实际需求的，所以Vue为你提供了一种方式来表达`这俩元素是完全独立的，不要复用它们的`.只需添加一个具有唯一值的`key`属性即可:

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

`v-show`

> 简单的切换元素css属性display

```html
<h1 v-show="ok">Hello! </h1>
```

> v-shwo 不支持`<template>` ，也不支持`v-else`

`v-if` vs `v-show`




