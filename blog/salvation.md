## js杂谈


### 函数参数

大家试想下这样一个函数--函数接受几个参数，但是这几个参数都不是必填的，函数该怎么处理？

```js
function xiaomi(money, food, drink, boyfriend1, ...) {
    //...
}

xiaomi(10, '', 'water', '', ...);
```

如果需求改了，操作函数也要改！函数也要增加一个参数

所以我们可以用对象做参数

```js
function xiaomi(opt) {
    //{...opt}
}
```

总结来说，就是当函数的参数不固定的时候，参数多(三个或者三个以上)的时候，建议用一个对象记录参数，这样会比较方便，也为以后如果参数要改留了条后路

### 对象的拷贝

深拷贝和浅拷贝只针对像Object, Array这样的引用类型数据

- 浅拷贝
  
  浅拷贝是对对象引用地址进行拷贝，并没有开辟新的栈，也就是拷贝后的结果是两个对象指向同一个引用地址，修改其中一个对象的属性，则另一个对象的属性也会改变
  <br />
  ```js
  var myInfo={name:'守候',sex:'男'};
  ```
  <br />
  
  ![shadow copy](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444b153bd43?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
  
  <br />
  
  ```js
  var newInfo=myInfo;
  ```
  ![shadow copy](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444e6487915?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
  
  ```js
  newInfo.sex='女';
  ```
  ![shadow copy](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444e2d6a43b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- 深拷贝
  深拷贝则是开启一个新的栈，两个对象对应两个不同的引用地址，修改一个对象的属性，不会改变另一个对象的属性
  
  <br />
  
  ![shadow copy](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444b153bd43?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
  
  <br />
  
  ![shadow copy](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444ea8bca2c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
  
  <br />
  
  ![shadow copy](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444a56dc84e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
  
  深拷贝也分两种情况，假跟真！
  
  <br />
  
  
- 数据持久化

  immutable就是指在创建变数、赋值后便不可改变，若对其有任何变更(例如：新增、修改、删除)，就会回传一个新值。
  <br />
  
  目前实作immutablity的library中较著名的就是immutable.js和seamless-immutable，由于底层实作方式不同，分别有各自的专长
  <br />
  
  > [浅谈immutable.js和seamless-immutable](https://cythilya.github.io/2017/02/12/immutability-immutablejs-seamless-immutable/)

### 模板字符串

```js
//Bad:
var message = 'Hello ' + name + ', it\'s ' + time + ' now'

//Good:
const message = `Hello ${name}, it's ${time} now`
```
### 解构

```js
//Bad:
var data = { name: 'dys', age: 1 }
var name = data.name,
    age = data.age
    
//Good:
const data = {name:'dys', age:1} 
const {name, age} = data 
```
### 采用函数式编程


### 变量的提升

```js
b() // call b
console.log(a) // undefined

var a = 'Hello world'

function b() {
    console.log('call b')
}
```

因为 JS 只会提升声明。而初始化赋值不会被提升。
并且，声明而不赋值时，变量会被自动初始化为 undefined，所以出现了上面的结果

在生成执行环境时，会有两个阶段。第一个阶段是创建的阶段，JS 解释器会找出需要提升的变量和函数，并且给他们提前在内存中开辟好空间，函数的话会将整个函数存入内存中，变量只声明并且赋值为 undefined，所以在第二个阶段，也就是代码执行阶段，我们可以直接提前使用

var 会产生很多错误，所以在 ES6中引入了 let。let 不能在声明前使用，但是这并不是常说的 let 不会提升，let 提升了，在第一阶段内存也已经为他开辟好了空间，但是因为这个声明的特性导致了并不能在声明前使用

### 作用域

```js
function b() {
    console.log(value)
}

function a() {
    var value = 2
    b()
}

var value = 1
a()
```
可以考虑下 b 函数中输出什么。你是否会认为 b 函数是在 a 函数中调用的，相应的 b 函数中没有声明 value 那么应该去 a 函数中寻找。其实答案应该是 1。
当在产生执行环境的第一阶段时，会生成 [[Scope]] 属性，这个属性是一个指针，对应的有一个作用域链表，JS 会通过这个链表来寻找变量直到全局环境。这个指针指向的上一个节点就是该函数声明的位置，因为 b 是在全局环境中声明的，所以 value 的声明会在全局环境下寻找。如果 b 是在 a 中声明的，那么 log 出来的值就是 2 了


### 异步

JS 是门同步的语言，你是否疑惑过那么为什么 JS 还有异步的写法。

其实 JS 的异步和其他语言的异步是不相同的，本质上还是同步。因为浏览器会有多个 Queue 存放异步通知，并且每个 Queue 的优先级也不同，JS 在执行代码时会产生一个执行栈，同步的代码在执行栈中，异步的在 Queue 中。有一个 Event Loop 会循环检查执行栈是否为空，为空时会在 Queue 中查看是否有需要处理的通知，有的话拿到执行栈中去执行

### 循环中的闭包

一个常见的错误出现在循环中使用闭包。

```js
for(var i = 0; i< 10; i++) {
	setTimeout(function() {
		console.log(i);
	}, 1000);
}
```

上面代码不会输出`0`到`9`, 而是会输出数字`10`十次。

```js
var arr1 = [], i;

for(i = 0; i < 3; i++) {
	arr1.push(function() {
		console.log(i);
	});
}

console.log(i, arr1);
```

闭包为什么可以?
因为function的参数是值传递而不是引用传递。

```js
var arr = [
	{
		a: 1,
		b: 2,
	},
	{
		a: 11,
		b: 22,
	}
]; 

var arr2 = [
	{
		c: 111,
	},
	{
		c: 222,
	}
],
arr.data = arr.map((item, index) => { //參數組賦值
	item.c  = arr2[index].c;
	return item;
})

console.log(arr, 'after init');
```




### 小数点

Math

### 日期

moment

### 箭头函数




