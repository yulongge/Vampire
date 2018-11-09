####  Array.isArray(observable([]))?

在做项目时(react + mobx + immutable)，用immutable往observable监听的数组里push一项内容，提示，原对象不是数组！so ... why?

参考资料发现：

> 请记住无论如何 Array.isArray(observable([])) 都将返回 false ，所以无论何时当你需要传递 observable 数组到外部库时，通过使用 Array.slice()在 observable 数组传递给外部库或者内置方法前创建一份浅拷贝(无论如何这都是最佳实践)总会是一个好主意.换句话说，Array.isArray(observable([]).slice()) 会返回 true

```js
let arry = [1, 2, 3];

const isOk = Array.isArray(observerble(arry));
//false

const isOk = Array.isArray(observerble(arry).slice());
//true
```

> observable 数组可以通过 .slice() 转变成 javascript 数组

如果我们单纯是去判断一个observerble对象是不是数组，可以用mobx提供的方法去判断

`isObservableArray`

判断是不是observerble数组

```js
isObservableArray(thing)
//如果类型匹配的话返回true
```

`sArrayLike`

判断是不是普通数组或者observerble数组

```js
isArrayLike(thing)
//如果给定的 thing 是 javascript 数组或者 observable (MobX的)数组的话，返回 true
```

#### 参考

- https://cn.mobx.js.org/refguide/api
- https://blog.csdn.net/Jane_96/article/details/82462491











