# Vue

## 作者

`尤雨溪`(Evan You)

- [Github](https://github.com/yyx990803)
- [知乎](https://www.zhihu.com/people/evanyou/activities)
- [微博](https://weibo.com/arttechdesign?is_hot=1)

## 简介

`Vue`

`/vju:/`, 类似于View

一套构建用户界面的`渐进式框架`。采用`自底向上增量开发`的设计。它只关注视图层，容易上手，便于与第三方库整合。

对于渐进式(Progressive)的理解: 渐进的，一步一步，不必一竿子把所有东西都用上

> [渐进式前端解决方案](https://mp.weixin.qq.com/s?__biz=MzIwNjQwMzUwMQ==&mid=2247484393&idx=1&sn=142b8e37dfc94de07be211607e468030&chksm=9723612ba054e83db6622a891287af119bb63708f1b7a09aed9149d846c9428ad5abbb822294&mpshare=1&scene=1&srcid=1026oUz3521V74ua0uwTcIWa&from=groupmessage&isappinstalled=0#wechat_redirect)

## 安装

Vue.js不支持IE8及其以下版本，因为Vue.js使用的`ECMAScript5`的特性在IE8下无法模拟。Vue支持所有兼容`ECMAScript5`的浏览器。

> vue.js的数据变动是依赖`Object.defineProperty()` 的

> http://caniuse.com/#feat=es5

- 直接引入`<script>`标签
  + CDN
  + [unpkg](https://unpkg.com/vue) : npm 发布后立即同步
  + [jsDelivr](https://cdn.jsdelivr.net/npm/vue/dist/vue.js) : npm发布后需要一段时间才能同步，所以可能无法获取最新版本
  + [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/2.4.0/vue.js) : 同jsDelivr

Vue会被注册一个全局变量。在开发环境中引入开发环境版本，包含完整的警告和调试模式，对开发更加友好!


- Bower

```shell
bower install vue
```

- NPM

> 在用Vue构建大型应用程序时，推荐使用NPM安装方式，可以很好地与模块打包器(webpack/browserify)配合使用。

```shell
npm install vue
```

在NPM包的`dist/`目录下，你会找到许多不同的构建版本的Vue.js.

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

### Vue实例

> 每个Vue应用都是通过Vue函数创建一个新的Vue实例开始的.

```js
var vm = new Vue({
  // 选项 options
  // ==========
  // 数据
  data: '',
  props: '',
  computed: {}, //不应该使用箭头函数来定义计算属性函数
  methods: {}, //不应该使用箭头函数来定义 method 函数
  watch: { key: function(val, oldVal){} },
  // ==========
  // DOM
  el: '',
  template: '',
  render: function(){}, // [Render 函数]
  // ==========
  // 生命周期
  // ==========
  // 资源
  // ==========
  // 杂项
 })
```

- demo:

```html
<div id="app">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
```

这样就创建了一个实例，创建时你可以传入一个选项对象。

#### 实例的生命的周期

![vue-lifecycle](https://yulongge.github.io/images/vue/vue_lifecycle.png)

它可以总共分为8个阶段：

- beforeCreate（创建前）,
- created（创建后）,
- beforeMount(载入前),
- mounted（载入后）,
- beforeUpdate（更新前）,
- updated（更新后）,
- beforeDestroy（销毁前）,
- destroyed（销毁后）

如果熟悉react的话可以来回想一下:

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

#### 原始HTML

`Mastache` 会将数据解释为不同的文本，而非HTML代码，有时候我们想输出正真的HTML,需要使用`v-html`指令

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
<a v-bind:href="url">...</a>

//监听事件
<a v-on:click="doSomething">...</a>
```

#### 修饰符

修饰符(Modifiers)是以半角句号`.`指明的特殊后缀，用于指出一个指令该以特殊方式绑定。

```html
<!-- .prevent 修饰符告诉v-on指令对于触发的事件调用`event.preventDefault();` -->
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

- bind : 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。
- inserted : 被绑定元素插入父节点时调用 (父节点存在即可调用，不必存在于 document 中)。
- update : 所在组件的 VNode 更新时调用，但是可能发生在其孩子的 VNode 更新之前。指令的值可能发生了改变也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新
- componentUpdated : 所在组件的 VNode 及其孩子的 VNode 全部更新时调用
- unbind : 只调用一次，指令与元素解绑时调用


#### 钩子函数参数

- el : 指令所绑定的元素，可以用来直接操作 DOM
- binding : 一个对象
	+ name : 指令名，不包括 `v-` 前缀
	+ value : 指令的绑定值，例如：`v-my-directive="1 + 1"`, value 的值是 `2`
	+ oldValue : 指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用
	+ expression : 绑定值的字符串形式。例如 `v-my-directive="1 + 1"` ，`expression` 的值是 `"1 + 1"`
	+ arg : 传给指令的参数。例如 `v-my-directive:foo`，arg 的值是 `"foo"`。
	+ modifiers : 一个包含修饰符的对象。例如：`v-my-directive.foo.bar`, 修饰符对象 modifiers 的值是 `{ foo: true, bar: true }`
- vnode : Vue 编译生成的虚拟节点
- oldVnode : 上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用

> 除了el之外，其他的参数都应该是只读的，尽量不要修改他们。

#### 简写

> 大多数情况下，我们可能想在bind 和 update 钩子上做重复动作，并且不想关心其他的钩子函数。

```js
Vue.derective('color-swatch', function(el, binding) {
	el.style.backgroundColor = binding.value
})
```

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

因为是指令，所以只能添加到元素上，所以如果想切换多个元素，可以把一个`<template>`元素当做一个不可见的包裹元素，并在上面使用`v-if`.最终渲染结果将不包含`<template>`元素.

```html
<template v-if="ok">
	<h1>....</h1>
	<h1>....</h1>
	<h1>....</h1>
</template>
```

用`key`管理可复用的元素

Vue会尽可能高效地渲染元素，通常会复用已有的元素而不是从头开始渲染。这么做Vue变得非常快。

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

- `v-show`

> 简单的切换元素css属性display

```html
<h1 v-show="ok">Hello! </h1>
```

> v-show 不支持`<template>` ，也不支持`v-else`


- `v-if` vs `v-show`

`v-if` 是真正的条件渲染，因为他会确保在切换过程中条件块内的事件监听和子组件适当的被销毁和重建。

`v-if` 也是惰性的，如果在初始渲染时条件为假，则什么也不做，一直到条件第一次变为真时，才会开始渲染条件块。

`v-show`就简单的多，不管初始条件是什么，元素总是会被渲染的，并且只是简单地基于css进行切换。

一般来说，`v-if`有更高的切换开销，而`v-show`有更高的初始渲染开销。因此，如果需要频繁的切换，则使用`v-show`，如果在运行时条件很少改变，则使用`v-if`较好.


### 循环语句

`v-for`指令根据一组数组的选项列表进行渲染。`v-for`指令需要使用`item in items`形式的特殊语法， `items`是源数据数组并且`item`是数组元素迭代的别名。

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>

<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>

<div v-for="item of items"></div>
```

`v-for` 还支持一个可选的第二个参数为当前项的索引，也可以ongoing`of` 代替 `in`作为分隔符，因为它是最近JavaScript迭代器的语法。

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

`v-for`还可以用来迭代对象的属性

```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>

<div v-for="(value, key) in object">
  {{ key }}: {{ value }}
</div>

<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```

也可以用第二个参数为键名, 第三个参数为索引

```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```

遍历对象时，是按Object.keys()的结果遍历的。

`key`

Vue用`v-for`渲染列表是，默认用`就地复用`策略。这种模式是高效的，但是只适用于不依赖子组件状态或临时DOM状态的渲染，为了追踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一的key属性，理想的`key`值是有唯一的id，用`v-bind`来绑定`key`值。

```html
<div v-for="item in items" v-bind:key="item.id">....</div>
```

- `数组更新检测`
	- 变异方法
		+ push()
		+ pop()
		+ shift()
		+ unshift()
		+ splice()
		+ sort()
		+ reverse()

	> Vue包含一组观察数组的变异方法，所以它们也将会触发视图更新。

```js
//打开控制台
example1.items.push({ message: 'longgege coming...'});

//会引起列表重新渲染
```

- 替换数组
	+ filter()
	+ concat()
	+ slice()

变异数组会改变原始数组，相比之下也有非变异方法。它们不会改变原始数组，总是返回一个新数组，可以用新数组替换旧数组。

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

幸运的是，Vue为了使得DOM元素得到最大范围的重用实现了一些机智得到，启发式的方法，所以这样替换也是非常高效的操作。

- 注意事项
> 由于JavaScript的限制，Vue不能检测一下变动的数组;

+ 利用索引设置一个项时
+ 修改数组长度时

```js
//利用索引
vm.items[indexOfItem] = newValue;
//解决办法
Vue.set(example .items, indexOfItem, newValue);
example.items.splice(indexOfItem, 1, newValue)

//修改长度
vm.items.length = newLength;
//解决办法
example.items.splice(newLength)
```

- `对象更改检测`

由于JavaScript限制，Vue不能检测对象属性的添加和删除

```js
var vm = new Vue({
	data: {
		user: {
			name: 'longgege'
		}
	}
});

vm.user.name = 'fanjiejie'; //不是响应式的

Vue.set(vm.user, 'age', 18);
this.$set(this.user, 'age', 18)
```

有时候需要为已有对象赋予多个新属性，比如Object.assign()或者_.extend().

```js
Object.assign(this.user, {
	age: 18,
	email: 'gaiyulong@gmail.com'
}) //不响应式

this.user = Object.assign({}, this.user, {
	age: 18,
	email: 'gaiyulong@gmail.com'
})

```

### 监听事件

`v-on`指令监听DOM事件触发。

```html
<div id="example-1">
  <button v-on:>增加 1</button>
  <p>这个按钮被点击了 {{ counter }} 次。</p>
</div>

<div id="example-2">
  	<!-- `greet` 是在下面定义的方法名 -->
  	<button v-on:click="greet">Greet</button>

	<!-- 内联处理器的方法 -->
	<button v-on:click="say('hi')">Say hi</button>
	<!-- 有时也需要在内联语句处理器中访问原生 DOM 事件。可以用特殊变量 $event 把它传入方法 -->
	<button v-on:click="warn('Form cannot be submitted yet.', $event)">
	  Submit
	</button>
</div>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  },
  methods: {
	greet: function(event) {
		//....
	},
	say: function(message) {
		console.log(message, 'msg');
	},
	warn: function (message, event) {
		// 现在我们可以访问原生事件对象
		if (event) event.preventDefault()
		alert(message)
	}
  }
})

example1.greet() // 也可以用JavaScript 直接调用
```

- `事件修饰符`

在事件处理程序中调用 event.preventDefault() 或 event.stopPropagation() 是非常常见的需求。尽管我们可以在 methods 中轻松实现这点，但更好的方式是：methods 只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

Vue 为了解决这个问题，提供了`事件修饰符`,通过`(.)`表示的指令后缀来调用修饰符

- .stop
- .prevent
- .capture
- .self
- .once

```html
!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身 (比如不是子元素) 触发时触发回调 -->
<div v-on:click.self="doThat">...</div>
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 @click.prevent.self 会阻止所有的点击，而 @click.self.prevent 只会阻止元素上的点击。

- `键值修饰符`

在监听键盘事件时，我们经常需要监测常见的键值。Vue 允许为 v-on 在监听键盘事件时添加关键修饰符

```html
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：

```html
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名:

- .enter
- .tab
- .delete
- .esc
- .space
- .up
- .down
- .left
- .right
- .ctrl
- .alt
- .shift
- .meta

### 表单

Vue用v-model指令在表单控件元素上创建双向数据绑定, `v-model`会根据控件类型自动选取正确的方法来更新元素。

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

- 文本
- 多行文本
- 复选框
- 单选按钮
- 选择列表

`文本与多行文本`

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>

<textarea v-model="message" placeholder="add multiple lines" />

```

`复选框`

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>

<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>

```

```js
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```

`单选按钮`

```html
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>

```

```js
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```

`选择列表`

```html
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

```js
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

`多选列表`

```html
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
```

```js
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
```

动态选项用`v-for`渲染

```html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```

```js
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```

- 修饰符

`.lazy`

在默认情况下，v-model 在 input 事件中同步输入框的值与数据 (除了 上述 IME 部分)，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步：

```html
<!-- 在 "change" 而不是 "input" 事件中更新 -->
<input v-model.lazy="msg" >
```

`.number`

如果想自动将用户的输入值转为 Number 类型 (如果原值的转换结果为 NaN 则返回原值)，可以添加一个修饰符 number 给 v-model 来处理输入值：

```html
<input v-model.number="age" type="number">
```

`.trim`

如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入：

```html
<input v-model.trim="msg">
```

### 样式绑定

> 操作元素的class列表和内联样式是数据绑定的一个常见需求，因为他们都是属性，所以可用`v-bind`处理它们,Vue为了防止字符串拼接带来的麻烦和易错，将v-bind用于class 和 style 时，做了增强，表达式结果的类型除了字符串之外，还可以是对象或数组。

- v-bind:class
- v-bind:style

```html
<!--设置一个对象 -->
<div v-bind:class="{active: isActive}"></div>

<!--设置多个属性 -->
<div class="static" v-bind:class="{active: isActive, 'text-hide': isHide}"></div>

<!--直接绑定对象 -->
<div v-bind:class="classObject"></div>
```

```js
new Vue({
  el: '#app',
  data: {
  isActive: true,
  isHide: null
  },
  computed: {
    classObject: function () {
      return {
        active: this.isActive && !this.error,
        'text-danger': this.error && this.error.type === 'fatal',
      }
    }
  }
})
```

以上算是对象绑定，我们还可以绑定数组.

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```js
new Vue({
  el: '#app',
  data: {
  	isActive: true,
    activeClass: 'active',
    errorClass: 'text-danger'
  }
})
```

还可以用三元表达式:

```html
<div v-bind:class="[errorClass ,isActive ? activeClass : '']"></div>
```

内联样式写法：

```html
<div id="app">
    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">xm</div>
</div>
```

也可以直接绑定到对象或数组上：

```html
<div id="app">
  <div v-bind:style="styleObject">cml</div>
  <div v-bind:style="[baseStyles, overridingStyles]">fqq</div>
</div>
```

```js
new Vue({
  el: '#app',
  data: {
    baseStyles: {
      color: 'green',
      fontSize: '30px'
    },
	overridingStyles: {
      'font-weight': 'bold'
    }
  }
})
```


当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform ，Vue.js 会自动侦测并添加相应的前缀。

### 组件

组件(Component)是Vue最强大的功能之一。可以扩展HTML元素，封装可重用的代码。


- 注册全局组件

```js
Vue.component(tagName, options)
```
> - tagName为组件名
> - options 为配置选项

```html
<tagName></tagName>
```

demo:

```html
<div id="app">
    <runoob></runoob>
</div>

<script>
// 注册
Vue.component('runoob', {
  template: '<h1>自定义组件!</h1>'
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```

- 注册局部组件

可以在实例选项中注册局部组件，这样组件只能在这个实例中使用

```html
<div id="app">
    <runoob></runoob>
</div>

<script>
var Child = {
  template: '<h1>自定义组件!</h1>'
}

// 创建根实例
new Vue({
  el: '#app',
  components: {
    // <runoob> 将只在父模板可用
    'runoob': Child
  }
})
</script>
```

- Props
prop 是父组件用来传递数据的一个自定义属性, 父组件的数据需要通过`props`把数据传给子组件，子组件需要显示地用props选项声明"props"

```html
<div id="app">
    <child message="hello!"></child>
</div>

<script>
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 同样也可以在 vm 实例中像 "this.message" 这样使用
  template: '<span>{{ message }}</span>'
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```

动态Props类似于用 v-bind 绑定 HTML 特性到一个表达式，也可以用 v-bind 动态绑定 props 的值到父组件的数据中。每当父组件的数据变化时，该变化也会传导给子组件：

```html
<div id="app">
    <div>
      <input v-model="parentMsg">
      <br>
      <child v-bind:message="parentMsg"></child>
    </div>
</div>
<script>
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 同样也可以在 vm 实例中像 "this.message" 这样使用
  template: '<span>{{ message }}</span>'
})
// 创建根实例
new Vue({
  el: '#app',
  data: {
    parentMsg: '父组件内容'
  }
})
</script>
```

> prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。

- Props验证
组件可以为 props 指定验证要求。prop 是一个对象而不是字符串数组时，它包含验证要求：

```js
Vue.component('example', {
  props: {
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    ...
  }
})
```

type可以是下面原生构造器：

`String`, `Number`, `Boolean`, `Function`, `Object`, `Array`


- 自定义事件
	- 使用 $on(eventName) 监听事件
	- 使用 $emit(eventName) 触发事件

父组件是使用 props 传递数据给子组件，但如果子组件要把数据传递回去，就需要使用自定义事件！
我们可以使用 v-on 绑定自定义事件, 每个 Vue 实例都实现了事件接口(Events interface)

```html
<div id="app">
    <div id="counter-event-example">
      <p>{{ total }}</p>
      <button-counter v-on:increment="incrementTotal"></button-counter>
      <button-counter v-on:increment="incrementTotal"></button-counter>
    </div>
</div>

<script>
Vue.component('button-counter', {
  template: '<button v-on:click="increment">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    increment: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
</script>
```

如果你想在某个组件的根元素上监听一个原生事件。可以使用 .native 修饰 v-on

```html
<my-component v-on:click.native="doTheThing"></my-component>
```

### 计算属性与观察者

`computed`,在处理一些复杂逻辑时，我们可以用计算属性。

```html
<div id="app">
  <p>原始字符串: {{ message }}</p>
  <p>计算后反转字符串: {{ reversedMessage }}</p>
</div>

<script>
var vm = new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```

计算属性的setter和getter

```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ..
```

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter

computed VS method

我们可以通过在表达式中调用方法来达到同样的效果：

```html
<p>Reversed message: "{{ reversedMessage() }}"</p>

<script>
// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
</script>
```

我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

```js
computed: {
  now: function () {
    return Date.now()
  }
}
```

computed VS watch(倾听属性)

Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：侦听属性。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch——特别是如果你之前使用过 AngularJS。然而，通常更好的做法是使用计算属性而不是命令式的 watch 回调

```html
<div id="demo">{{ fullName }}</div>
<script>
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
</script>
```

```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

- 倾听器

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的

```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>

```

```js
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为判定用户停止输入等待的毫秒数
      500
    )
  }
})
```

> 使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

## 衍生

- vue-router
- vuex

## 参考

- https://cn.vuejs.org/
- http://www.runoob.com/vue2/vue-tutorial.html
