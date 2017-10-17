## Require & Import

> 在模块化的js时代，我们还是要搞清楚一些事情滴。

- `require`
- `module.exports/exports`
- `import`
- `export`

> 看到这几个，有没有晕圈滴感觉！他们源于何处，用于何处?

**node** 的module遵循 **CommonJS** 规范，**requirejs** 遵循 **AMD** 规范，**seajs** 遵循 **cmd** 规范，而 **es6** 的module并没有直接遵循commonJS的规范，而是想取代他们。

我们从古至今说起~

### CommonJS

> **Node** 的module就是遵循了CommonJS

CommonJS的规范诞生比较早，一般都这样写：

```js
//a.js
module.exports = {
    temp1: function() {...},
    temp2: new Object(),
    temp3: "yulong",
    ....
}

//b.js
var myNeed = require('a.js');
var name = myNeed.temp3;
var myAction = myNeed.temp1;
myAction();
```
他的用法就是:

- module.exports 用于导出
- require 用于导入

这中写法适合服务器，直接读取本地文件，加载快，但由于同步关系，在客户端用会造成假死状态，因而诞生了AMD。

> node方便导出功能函数，exports是引用 module.exports的值，所以有了exports


### AMD & CMD

> - AMD ( **Asynchronous Module Definition** ),AMD的规范实现了异步加载模块，requirejs算是代表吧，先定义依赖再加载完后回调执行
> - CMD ( **Common Module Definition** ),CMD的规范也是异步加载模块，seajs算是代表，依赖就近，用的时候再require


```js
//a.js
define(function(require, exports, module) {
    module.exports = {
        temp1: function() {...},
        temp2: new Object(),
        temp3: "yulong",
        ....
    };
});

//b.js
define(function(require, exports, module) {
    var myNeed = require('a.js');
    var name = myNeed.temp3;
    var myAction = myNeed.temp1;
    myAction();
})
```
> 为了保持风格的高度统一，除了在浏览器端的模块中要使用一个define函数来提供模块的闭包以外，其他代码可以完全一致。当然了amd和cmd的define格式很丰富，自己探索吧



### ES6

> ES6发布的module并没有直接采用CommonJS，甚至连require都没有采用，也就是说require仍然只是node的一个私有的全局方法，module.exports也只是node私有的一个全局变量属性，跟标准半毛钱关系都没有

```js
//a.js
export default function() {...}
export var name = "yulong"
const age = "18";
export {age} //{age: age}
...

//b.js
import *  from './a'; //a.js
...

```

> 本以为node8会直接支持import,但是还没有，可能因为v8还不支持吧，期待~

### require/import怎么用

#### 遵循规范

- require 是amd的规范
- import 是es6的一个语法标准，兼容浏览器的话，需要babel转成es5

#### 调用时间

- require是运行时调用，可以放在代码的任何地方
- import 是编译时调用，所以必须放在文件头

#### 内在

- require是赋值过程，其实require的结果就是对象，数字，字符串，函数等，再把结果赋给变量
- import 是解构过程，目前所有的引擎都还没实现import，只能用babel转化为require使用。

> ES7很快也会发布，js引擎们会尽快实现ES6标准的规定，如果一个引擎连标准都实现不了，就会被淘汰，ES6是迟早的事。如果你现在仍然在代码中部署require，那么等到ES6被引擎支持时，你必须升级你的代码，而如果现在开始部署import，那么未来可能只需要做很少的改动.
