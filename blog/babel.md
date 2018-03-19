## Babel

Bable 把最新标准编写的 JavaScript 代码向下编译成可以在今天随处可用的版本。这一过程叫 ** 源码到源码** 编译，也被称为转换编译。

> 15年11月， Babel 发布了6.0版本，相对于前一代Babel5, 新一代Babel更加模块化。将所有的转码功能以插件的形式分离出去，默认只提供babel-core. 原本只需要装一个babel,现在必须按照自己的需求配置，灵活性提高的同时也提高了使用者的学习成本。

> `npm i babel` 已经弃用，你能下载到的仅仅是一段console.warn, 告诉你babel6 不再以大杂烩的形式提供转码功能了。

### Babel 能干什么

```js
let fun = () => console.log('babel');
```

转义为:

```js
"use strick";

var fun = function() {
	return console.log('babel');
}
```

不过Babel的用途并不止于此，它支持语法扩展，能支持像React 所用的JSX语法，更重要的是，Babel的一切都是简单的插件，谁都可以创建自己的插件，利用Babel的全部威力去做任何事。

再进一步，Babel自身被分解成了数个核心模块，任何人都可以利用它来创建下一代的JavaScript 工具。

### Babel 的使用

#### babel-cli

Babel 的CLI是一种命令行下使用Babel编译的简单方法。

```shell
npm i -g babel-cli
```

这样我们就可以在命令行模式下使用babel了。

```shell
babel my-es6.js

babel example.js --out-file/-o compiled.js //结果写入到指定的文件
baeel src --out-dir/-d lib //编译整个目录到一个新的目录.

```

#### babel-core

如果你需要以编程的方式来使用Babel，可以使用babel-core这个包。

babel-core 的作用是把js代码分析`ast`, 方便各个插件分析语法进行相应的处理。有些新语法在低版本js中是不存在的，如果箭头函数，rest参数，函数默认值等，这种语言层面不兼容只能通过将代码转换为ast, 分析其语法后再转为最低版本js。

> AST(Abstract Syntax Tree)称为抽象语法树，指的是源代码语法所对应的树状结构，也就是说，对于一种具体编程语言下的源代码，通过构建语法的形式将源代码中的语句映射到树种的每一个节点上。

```shell
npm install babel-core
```

```js
var babel = require('babel-core');

//字符串

babel.transform("code();", options);
// => {code, map, ast}

// 文件
babel.transformFile("filename.js", options, function(err, result) {
	result; //{code, map, ast}
})
//同步
babel.transformFileSync('filename.js', options);
//=> {code, map, ast}
```

### 配置Babel

你或许已经注意到了，目前为止运行Babel，我们并没有翻代码， 而仅仅是把代码从一处拷贝到另一处，原因就是从Babel以后，默认的插件被移除了，如果没有指定一个插件，Babel将会原样输出，不会进行编译。

你可以通过安装插件(plugins)或预设(presets, 也就是一组插件)，来指示Babel去做什么事情。

插件只是单一的功能，例如

- es2015-arrow-functions
- es2015-classes
- es2015-for-of
- es2015-spread

以下安装箭头函数的插件方式

```shell
npm i --save-dev babel-plugin-transform-es2015-arrow-functions
```
如果我们一个个引入功能单一的插件的话显得特别麻烦，通常我们用的更多的是预设，插件和预设通常写入到配置文件中，可以将配置写入package.json的`babel`属性里，或者是一个单独的.babelrc文件。

- `.babelrc`

	在我们告诉Babel该做什么之前，你需要做的就是在项目的根路径下创建.babelrc文件，然后输入一下内容作为开始:

	```json
		{
			"presets": [],
			"plugins": []
		}
	```

	这个文件就是用来让Babel做你要它做的事情的配置文件。


- babel-preset-es2015

	预设babel-preset 系列打包了一组插件，类似于餐厅的套餐，例如`babel-preset-es2015`打包了es6的特性，`babel-preset-stage-0`打包处于strawman阶段的语法。

	> strawman 稻草人语法
	
	我们需要安装`es2015`Babel预设：

	```shell
		npm i --save-dev babel-preset-es2015
	```

	```json
	{
		"presets": [
			"es2015"
		],
		"plugins": []
	}
	```
	同样的还有`babel-preset-2016`, `babel-preset-2017`

- babel-preset-latest

	latest 是一个特殊的presets,包括了es2015, es2016, es2017的插件(以后的es2018也会包括进去), 即总是包含最新的编译插件。

- babel-preset-env

	上面提高的各种preset的问题就是: 它们都太重了， 即包含了过多在某些情况下不需要的功能，比如，现代的浏览器大多支持es6的generator,但是如果你使用babel-preset-es2015，它会将generator函数编译为复杂的es5代码，这是没必要的，但使用babel-preset-env,我们可以声明环境，然后该preset就会只编译包含我们所声明环境缺少的特性代码，因此也是比较推荐的方式。

	```shell
	npm i babel-preset-env --save-dev
	```

	```json
	{
		"presets": ["env"]
	}
	```

	当没有添加任何配置选项时， babel-preset-env默认行为是和babel-preset-latest是一样的。
	
	看它如何使用：

	```js
	//指定支持主浏览器最新的两个版本及IE7+
	"presets": [
		[
			"env", 
			{
				"targets": {
					"browsers": ["last 2 versions", "ie >= 7"]
				}
			}
		]
	]

	//支持超过市场份额5%的浏览器
	"targets": {
		"browsers": "> 5%"
	}

	//某个固定版本的浏览器
	"target": {
		"chrome": 56
	}
	```

	更多的配置: [文档](http://babeljs.io/docs/plugins/preset-env/)

- babel-preset-stage-x

	> 官方预设(preset), 有两种，一个是按年份(babel-preset-2017)，一个是按阶段(babel-preset-stage-0)。 这主要是根据TC39 委员会ECMASCRPIT 发布流程来制定的。TC39 委员会决定，从2016年开始，每年都会发布一个版本，它包括每年期限内完成的所有功能，同时ECMAScript的版本号也按年份编制，就有了ES2016, ES2017。所以也就有了babel-present-2016, babel-preset-2017， 对每一年新增的语法进行转化。babel-preset-latest 就是把所有es2015, es2016, es2017 全部包含在一起了

	最终在阶段4被标准正式采纳。

	以下是四个不同阶段的(打包的)预设:

	- babel-preset-stage-0
	- babel-preset-stage-1
	- babel-preset-stage-2
	- babel-preset-stage-3

	> stage-4预设是不存在的，因为它就是上面的es2017预设。
	
	以上每个预设都依赖于紧随的后期阶段预设，数字越小，阶段越靠后，存在依赖关系，也就是说stage-0 是包括stage-1的，以此类推，也就是说这些stage包含的特性比latest更新的特性还要靠后但还未被写入标准进行发布。
	使用是时候只需要安装你想要的阶段就可以了。

	```shell
	npm i --save-dev babel-preset-stage-2
	```

	然后添加进你的`.babelrc`配置文件，但是要注意如果没有提供es2017相关的预设，preset-stage-x这种阶段性的预设也不能用。



### Babel执行，生成代码

Babel几乎可以编译所有新的的JavaScript语法，但对于API来说却非如此，例如：Promise, Set, Map,等新增对象， Object.assign, Object.entries等语法。

为了达成使用这些新的API的目的，社区又有2个实现流派：babel-polyfill, babel-runtime + babel-plugin-transform-runtime

这两个模块功能几乎相同，就是转码新增api,模拟es6环境，但实现方式完全不同，babel-polyfill的做法就是将全局对象通通污染一遍，比如想在node0.10上用Promise，调用babel-polyfill就会忘global对象上挂上Promise对象，对于普通的业务代码没有关系，但如果用在模块上就有问题了，会把模块使用者的环境污染。

babel-runtime 的做法是自己手动引入helper函数，还是上面的例子，

```js
const Promise = require('babel-runtime/core-js/promise') //就可以引入Promise.
```

但是babel-runtime 也有问题，第一，很不方便， 第二，在代码中直接引入helper函数，以为着不能共享，造成最终打包出来的文件里有很多重复的helper代码，所以babel又开发了babel-plugin-transform-runtime，这个模块将我们的代码重写，如将Promise 重写冲_Promise，然后引入_Promise helper函数，这样就避免了重复打包代码和手动引入模块的痛苦。

- babel-polyfill

polyfill就是在当前运行环境中用来复制(模拟性的复制，而不是拷贝)尚不存在的原生api的代码，能让你提前使用还不可用的apis。

Babel用了优秀的core-js 用作polyfill, 并且还有定制化的regenerator来让generators(生成器), 和async function(异步函数), 正常工作。

要使用polyfill，首先要安装它：

```shell
npm i --save-dev babel-polyfill 
```

然后只需要在文件顶部导入polyfill就可以了

```js
import 
```
