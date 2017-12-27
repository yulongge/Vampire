## 函数

函数就是一段可以反复调用的代码块。函数还能接受输入的参数，不同的参数会返回不同的值。

### 声明

1. `function` 命令

	`function`命令声明的代码区块，就是一个函数。`function`命令后面是函数名，函数名后面是一对圆括号，里面是传入函数的参数。函数体放在大括号里面。

    ```js
    function print(web) {
        console.log(web, ' print');
    }
    // print('create a function');
    ```

2. 函数表达式

	除了用`function`命令声明函数，还可以采用变量赋值的写法。

    ```js
    var print = function(web) {
        console.log(web, 'print');
    }
    ```

    这种写法将一个匿名函数赋值给变量。这时，这个匿名函数又称函数表达式（Function Expression），因为赋值语句的等号右侧只能放表达式。

    > 采用函数表达式声明函数时，function命令后面不带有函数名。如果加上函数名，该函数名只在函数体内部有效，在函数体外部无效。

    ```js
    var print = function x(){
      console.log(typeof x);
    };

    x
    // ReferenceError: x is not defined

    print()
    // function
    ```

    这个x只在函数体内部可用，指代函数表达式本身，其他地方都不可用。这种写法的用处有两个，

    - 一是可以在函数体内部调用自身，
    - 二是方便除错（除错工具显示函数调用栈时，将显示函数名，而不再显示这里是一个匿名函数）。

    > 函数的表达式需要在语句的结尾加上分号，表示语句结束。而函数的声明在结尾的大括号后面不用加分号。

3. Function构造函数

    ```js
    var add = new Function(
      'x',
      'y',
      'return x + y'
    );

    // 等同于

    function add(x, y) {
      return x + y;
    }
    ```

    在上面代码中，Function构造函数接受三个参数，除了最后一个参数是add函数的“函数体”，其他参数都是add函数的参数。

    你可以传递任意数量的参数给Function构造函数，只有最后一个参数会被当做函数体，如果只有一个参数，该参数就是函数体。

    ```js
    var foo = new Function(
      'return "hello world"'
    );

    // 等同于

    function foo() {
      return 'hello world';
    }
    ```

    Function构造函数可以不使用new命令，返回结果完全一样。

    总的来说，这种声明函数的方式非常不直观，几乎无人使用。

4. 箭头函数

    ES6 允许使用“箭头”（=>）定义函数。

    ```js
    var f = v => v;

    //等同于

    var f = function(v) {
      return v;
    };
    ```

    箭头函数有几个使用注意点:

    （1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

    （2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

    （3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

    （4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

    上面四点中，第一点尤其值得注意。this对象的指向是可变的，但是在箭头函数中，它是固定的。

    ```js
    function foo() {
      setTimeout(() => {
        console.log('id:', this.id);
      }, 100);
    }

    var id = 21;

    foo.call({ id: 42 });
    // id: 42
    ```

#### 函数重复声明

如果同一个函数被多次声明，后面的声明就会覆盖前面的声明。

```js
function f() {
  console.log(1);
}
f() // 2

function f() {
  console.log(2);
}
f() // 2
```

### 函数的属性和方法

1. name属性

    name属性返回紧跟在function关键字之后的那个函数名。

    ```js
    function f1() {}
    f1.name // 'f1'

    var f2 = function () {};
    f2.name // ''

    var f3 = function myName() {};
    f3.name // 'myName'
    ```

    上面代码中，函数的name属性总是返回紧跟在function关键字之后的那个函数名。对于f2来说，返回空字符串，匿名函数的name属性总是为空字符串；对于f3来说，返回函数表达式的名字（真正的函数名还是f3，myName这个名字只在函数体内部可用）。

    需要注意的是，ES6 对这个属性的行为做出了一些修改。如果将一个匿名函数赋值给一个变量，ES5 的name属性，会返回空字符串，而 ES6 的name属性会返回实际的函数名。

    ```js
    var f = function () {};

    // ES5
    f.name // ""

    // ES6
    f.name // "f"
    ```

2. length 属性

    length属性返回函数预期传入的参数个数，即函数定义之中的参数个数。

    ```js
    function f(a, b) {}
    f.length // 2
    ```

    上面代码定义了空函数f，它的length属性就是定义时的参数个数。不管调用时输入了多少个参数，length属性始终等于2。

    length属性提供了一种机制，判断定义时和调用时参数的差异，以便实现面向对象编程的”方法重载“（overload）。

    ES6，指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，length属性将失真。

    ```js
    (function (a) {}).length // 1
    (function (a = 5) {}).length // 0
    (function (a, b, c = 5) {}).length // 2
    ```

    如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了

    这是因为length属性的含义是，该函数预期传入的参数个数。某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了。同理，后文的 rest 参数也不会计入length属性

    ```js
    (function (a = 0, b, c) {}).length // 0
    (function (a, b = 1, c) {}).length // 1
    ```

3. toString()

    函数的toString方法返回函数的源码。

    ```js
    function f() {
      a();
      b();
      c();
    }

    f.toString()
    // function f() {
    //  a();
    //  b();
    //  c();
    // }
    ```

    函数内部的注释也可以返回。

    ```js
    function f() {/*
      这是一个
      多行注释
    */}

    f.toString()
    // "function f(){/*
    //   这是一个
    //   多行注释
    // */}"
    ```

    利用这一点，可以变相实现多行字符串。

    ```js
    var multiline = function (fn) {
      var arr = fn.toString().split('\n');
      return arr.slice(1, arr.length - 1).join('\n');
    };

    function f() {/*
      这是一个
      多行注释
    */}

    multiline(f);
    // " 这是一个
    //   多行注释"
    ```

### 函数作用域

#### 定义

作用域（scope）指的是变量存在的范围。Javascript只有两种作用域：一种是全局作用域，变量在整个程序中一直存在，所有地方都可以读取；另一种是函数作用域，变量只在函数内部存在。

在函数外部声明的变量就是全局变量（global variable），它可以在函数内部读取。

```js
var v = 1;

function f(){
  console.log(v);
}

f()
// 1
```

上面的代码表明，函数f内部可以读取全局变量v。

在函数内部定义的变量，外部无法读取，称为“局部变量”（local variable）。

```js
function f(){
  var v = 1;
}

v // ReferenceError: v is not defined
```

上面代码中，变量v在函数内部定义，所以是一个局部变量，函数之外就无法读取。

函数内部定义的变量，会在该作用域内覆盖同名全局变量。

```js
var v = 1;

function f(){
  var v = 2;
  console.log(v);
}

f() // 2
v // 1
```

上面代码中，变量v同时在函数的外部和内部有定义。结果，在函数内部定义，局部变量v覆盖了全局变量v。

注意，对于var命令来说，局部变量只能在函数内部声明，在其他区块中声明，一律都是全局变量

```js
if (true) {
  var x = 5;
}
console.log(x);  // 5
```

上面代码中，变量x在条件判断区块之中声明，结果就是一个全局变量，可以在区块之外读取。

#### 函数内部的变量提升

与全局作用域一样，函数作用域内部也会产生“变量提升”现象。var命令声明的变量，不管在什么位置，变量声明都会被提升到函数体的头部。

```js
function foo(x) {
  if (x > 100) {
    var tmp = x - 100;
  }
}
```

上面的代码等同于

```js
function foo(x) {
  var tmp;
  if (x > 100) {
    tmp = x - 100;
  };
}
```

#### 函数本身的作用域

函数本身也是一个值，也有自己的作用域。它的作用域与变量一样，就是其声明时所在的作用域，与其运行时所在的作用域无关。

```js
var a = 1;
var x = function () {
  console.log(a);
};

function f() {
  var a = 2;
  x();
}

f() // 1
```

上面代码中，函数x是在函数f的外部声明的，所以它的作用域绑定外层，内部变量a不会到函数f体内取值，所以输出1，而不是2。

总之，函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。

很容易犯错的一点是，如果函数A调用函数B，却没考虑到函数B不会引用函数A的内部变量。

```js
var x = function () {
  console.log(a);
};

function y(f) {
  var a = 2;
  f();
}

y(x)
// ReferenceError: a is not defined
```

上面代码将函数x作为参数，传入函数y。但是，函数x是在函数y体外声明的，作用域绑定外层，因此找不到函数y的内部变量a，导致报错。

同样的，函数体内部声明的函数，作用域绑定函数体内部。

```js
function foo() {
  var x = 1;
  function bar() {
    console.log(x);
  }
  return bar;
}

var x = 2;
var f = foo();
f() // 1
```

上面代码中，函数foo内部声明了一个函数bar，bar的作用域绑定foo。当我们在foo外部取出bar执行时，变量x指向的是foo内部的x，而不是foo外部的x。正是这种机制，构成了下文要讲解的“闭包”现象

### 参数

1. 概述

    函数运行的时候，有时需要提供外部数据，不同的外部数据会得到不同的结果，这种外部数据就叫参数。

    ```js
    function square(x) {
      return x * x;
    }

    square(2) // 4
    square(3) // 9

    ```

    上式的x就是square函数的参数。每次运行的时候，需要提供这个值，否则得不到结果。

2. 参数的省略

    函数参数不是必需的，Javascript允许省略参数。

    ```js
    function f(a, b) {
      return a;
    }

    f(1, 2, 3) // 1
    f(1) // 1
    f() // undefined

    f.length // 2
    ```

    上面代码的函数f定义了两个参数，但是运行时无论提供多少个参数（或者不提供参数），JavaScript都不会报错。

    被省略的参数的值就变为undefined。需要注意的是，函数的length属性与实际传入的参数个数无关，只反映函数预期传入的参数个数。

    但是，没有办法只省略靠前的参数，而保留靠后的参数。如果一定要省略靠前的参数，只有显式传入undefined。

    ```js
    function f(a, b) {
      return a;
    }

    f( , 1) // SyntaxError: Unexpected token ,(…)
    f(undefined, 1) // undefined
    ```

    上面代码中，如果省略第一个参数，就会报错。

3. 默认值

    通过下面的方法，可以为函数的参数设置默认值。

    ```js
    function f(a){
      a = a || 1;
      return a;
    }

    f('') // 1
    f(0) // 1
    ```

    上面代码的||表示“或运算”，即如果a有值，则返回a，否则返回事先设定的默认值（上例为1）。

    这种写法会对a进行一次布尔运算，只有为true时，才会返回a。可是，除了undefined以外，0、空字符、null等的布尔值也是false。也就是说，在上面的函数中，不能让a等于0或空字符串，否则在明明有参数的情况下，也会返回默认值。

    为了避免这个问题，可以采用下面更精确的写法

    ```js
    function f(a) {
      (a !== undefined && a !== null) ? a = a : a = 1;
      return a;
    }

    f() // 1
    f('') // ""
    f(0) // 0

    //上面代码中，函数f的参数是空字符或0，都不会触发参数的默认值。
    ```

    ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面

    ```js
    function log(x, y = 'World') {
      console.log(x, y);
    }

    log('Hello') // Hello World
    log('Hello', 'China') // Hello China
    log('Hello', '') // Hello
    ```

    使用参数默认值时，函数不能有同名参数。

    ```js
    // 不报错
    function foo(x, x, y) {
      // ...
    }

    // 报错
    function foo(x, x, y = 1) {
      // ...
    }
    // SyntaxError: Duplicate parameter name not allowed in this context
    ```

    与解构赋值默认值结合使用

    参数默认值可以与解构赋值的默认值，结合起来使用。

    ```js
    function foo({x, y = 5}) {
      console.log(x, y);
    }

    foo({}) // undefined 5
    foo({x: 1}) // 1 5
    foo({x: 1, y: 2}) // 1 2
    foo() // TypeError: Cannot read property 'x' of undefined
    ```

    参数默认值的位置

    通常情况下，定义了默认值的参数，应该是函数的尾参数。因为这样比较容易看出来，到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。

    ```js
    // 例一
    function f(x = 1, y) {
      return [x, y];
    }

    f() // [1, undefined]
    f(2) // [2, undefined])
    f(, 1) // 报错
    f(undefined, 1) // [1, 1]

    // 例二
    function f(x, y = 5, z) {
      return [x, y, z];
    }

    f() // [undefined, 5, undefined]
    f(1) // [1, 5, undefined]
    f(1, ,2) // 报错
    f(1, undefined, 2) // [1, 5, 2]
    ```

    如果传入undefined，将触发该参数等于默认值，null则没有这个效果。

    ```js
    function foo(x = 5, y = 6) {
      console.log(x, y);
    }

    foo(undefined, null)
    // 5 null

    ```

4. 传递方式
    函数参数如果是原始类型的值（数值、字符串、布尔值），传递方式是传值传递（passes by value）。这意味着，在函数体内修改参数值，不会影响到函数外部。

    ```js
    var p = 2;

    function f(p) {
      p = 3;
    }
    f(p);

    p // 2
    ```

    上面代码中，变量p是一个原始类型的值，传入函数f的方式是传值传递。因此，在函数内部，p的值是原始值的拷贝，无论怎么修改，都不会影响到原始值。

	但是，如果函数参数是复合类型的值（数组、对象、其他函数），传递方式是传址传递（pass by reference）。也就是说，传入函数的原始值的地址，因此在函数内部修改参数，将会影响到原始值。

    ```js
    var obj = {p: 1};

    function f(o) {
      o.p = 2;
    }
    f(obj);

    obj.p // 2
    ```

    上面代码中，传入函数f的是参数对象obj的地址。因此，在函数内部修改obj的属性p，会影响到原始值。

    注意，如果函数内部修改的，不是参数对象的某个属性，而是替换掉整个参数，这时不会影响到原始值。

    ```js
    var obj = [1, 2, 3];

    function f(o){
      o = [2, 3, 4];
    }
    f(obj);

    obj // [1, 2, 3]
    ```

    上面代码中，在函数f内部，参数对象obj被整个替换成另一个值。这时不会影响到原始值。这是因为，形式参数（o）与实际参数obj存在一个赋值关系。

    ```js
    // 函数f内部
    o = obj;
    ```

    上面代码中，对o的修改都会反映在obj身上。但是，如果对o赋予一个新的值，就等于切断了o与obj的联系，导致此后的修改都不会影响到obj了。

    某些情况下，如果需要对某个原始类型的变量，获取传址传递的效果，可以将它写成全局对象的属性。

    ```js
    var a = 1;

    function f(p) {
      window[p] = 2;
    }
    f('a');

    a // 2
    ```

    上面代码中，变量a本来是传值传递，但是写成window对象的属性，就达到了传址传递的效果。

5. 同名参数

    如果有同名的参数，则取最后出现的那个值。

    ```js
    function f(a, a) {
      console.log(a);
    }

    f(1, 2) // 2
    ```

    上面的函数f有两个参数，且参数名都是a。取值的时候，以后面的a为准。即使后面的a没有值或被省略，也是以其为准。

    ```js
    function f(a, a){
      console.log(a);
    }

    f(1) // undefined
    ```

    调用函数f的时候，没有提供第二个参数，a的取值就变成了undefined。这时，如果要获得第一个a的值，可以使用arguments对象。

    ```js
    function f(a, a) {
      console.log(arguments[0]);
    }

    f(1) // 1
    ```

6. arguments对象

    - 定义

        由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是arguments对象的由来。

        arguments对象包含了函数运行时的所有参数，arguments[0]就是第一个参数，arguments[1]就是第二个参数，以此类推。这个对象只有在函数体内部，才可以使用。

        ```js
        var f = function (one) {
          console.log(arguments[0]);
          console.log(arguments[1]);
          console.log(arguments[2]);
        }

        f(1, 2, 3)
        // 1
        // 2
        // 3
        ```

        正常模式下，arguments对象可以在运行时修改。

        ```js
        var f = function(a, b) {
          arguments[0] = 3;
          arguments[1] = 2;
          return a + b;
        }

        f(1, 1) // 5
        ```

        上面代码中，函数f调用时传入的参数，在函数内部被修改成3和2。

        严格模式下，arguments对象是一个只读对象，修改它是无效的，但不会报错。

        ```js
        var f = function(a, b) {
          'use strict';
          arguments[0] = 3; // 无效
          arguments[1] = 2; // 无效
          return a + b;
        }

        f(1, 1) // 2
        ```

        上面代码中，函数体内是严格模式，这时修改arguments对象就是无效的。

        可以通过arguments对象的length属性，判断函数调用时到底带几个参数。

        ```js
        function f() {
          return arguments.length;
        }

        f(1, 2, 3) // 3
        f(1) // 1
        f() // 0
        ```

    - 与数组的关系

        需要注意的是，虽然arguments很像数组，但它是一个对象。数组专有的方法（比如slice和forEach），不能在arguments对象上直接使用。

        但是，可以通过apply方法，把arguments作为参数传进去，这样就可以让arguments使用数组方法了。

        ```js
        // 用于apply方法
        myfunction.apply(obj, arguments).

        // 使用与另一个数组合并
        Array.prototype.concat.apply([1,2,3], arguments)
        ```

        要让arguments对象使用数组方法，真正的解决方法是将arguments转为真正的数组。下面是两种常用的转换方法：slice方法和逐一填入新数组。

        ```js
        var args = Array.prototype.slice.call(arguments);

        // or

        var args = [];
        for (var i = 0; i < arguments.length; i++) {
          args.push(arguments[i]);
        }
        ```

    - callee属性

        arguments对象带有一个callee属性，返回它所对应的原函数

        ```js
        var f = function(one) {
          console.log(arguments.callee === f);
        }

        f() // true
        ```

        可以通过arguments.callee，达到调用函数自身的目的。这个属性在严格模式里面是禁用的，因此不建议使用。

### 函数的其他知识点

#### 圆括号运算符，return语句和递归

调用函数时，要使用圆括号运算符。圆括号之中，可以加入函数的参数

```js
function add(x, y) {
  return x + y;
}

add(1, 1) // 2
```

函数体内部的return语句，表示返回。JavaScript引擎遇到return语句，就直接返回return后面的那个表达式的值，后面即使还有语句，也不会得到执行。也就是说，return语句所带的那个表达式，就是函数的返回值。return语句不是必需的，如果没有的话，该函数就不返回任何值，或者说返回undefined

函数可以调用自身，这就是递归（recursion）。

```js
function fun()
{
    // 自己调用自己，称为递归调用
    fun();
    console.log("m2");
}
fun();
```

#### 第一等公民

JavaScript语言将函数看作一种值，与其它值（数值、字符串、布尔值等等）地位相同。凡是可以使用值的地方，就能使用函数。比如，可以把函数赋值给变量和对象的属性，也可以当作参数传入其他函数，或者作为函数的结果返回。函数只是一个可以执行的值，此外并无特殊之处。

由于函数与其他数据类型地位平等，所以在JavaScript语言中又称函数为第一等公民。

```js
function add(x, y) {
  return x + y;
}

// 将函数赋值给一个变量
var operator = add;

// 将函数作为参数和返回值
function a(op){
  return op;
}
a(add)(1, 1)
// 2
```

#### 函数名的提升

JavaScript引擎将函数名视同变量名，所以采用function命令声明函数时，整个函数会像变量声明一样，被提升到代码头部

```js
f();

function f() {}
```

表面上，上面代码好像在声明之前就调用了函数f。但是实际上，由于“变量提升”，函数f被提升到了代码头部，也就是在调用之前已经声明了。但是，如果采用赋值语句定义函数，JavaScript就会报错

```js
f();
var f = function (){};
// TypeError: undefined is not a function
```

上面的代码等同于下面的形式。

```js
var f;
f();
f = function () {};
```

上面代码第二行，调用f的时候，f只是被声明了，还没有被赋值，等于undefined，所以会报错。因此，如果同时采用function命令和赋值语句声明同一个函数，最后总是采用赋值语句的定义。

```js
var f = function() {
  console.log('1');
}

function f() {
  console.log('2');
}

f() // 1
```

#### 不能在条件语句中声明函数

根据ECMAScript的规范，不得在非函数的代码块中声明函数，最常见的情况就是if和try语句。

```js
if (foo) {
  function x() {}
}

try {
  function x() {}
} catch(e) {
  console.log(e);
}
```
上面代码分别在if代码块和try代码块中声明了两个函数，按照语言规范，这是不合法的。但是，实际情况是各家浏览器往往并不报错，能够运行。

但是由于存在函数名的提升，所以在条件语句中声明函数，可能是无效的，这是非常容易出错的地方。

```js
if (false) {
  function f() {}
}

f() // 不报错
```

上面代码的原始意图是不声明函数f，但是由于f的提升，导致if语句无效，所以上面的代码不会报错。要达到在条件语句中定义函数的目的，只有使用函数表达式。

```js
if (false) {
  var f = function () {};
}

f() // undefined
```

#### 闭包

闭包（closure）是Javascript语言的一个难点，也是它的特色，很多高级应用都要依靠闭包实现。

要理解闭包，首先必须理解变量作用域。前面提到，JavaScript有两种作用域：全局作用域和函数作用域。函数内部可以直接读取全局变量

```js
var n = 999;

function f1() {
  console.log(n);
}
f1() // 999
```

上面代码中，函数f1可以读取全局变量n。

但是，在函数外部无法读取函数内部声明的变量

```js
function f1() {
  var n = 999;
}

console.log(n)
// Uncaught ReferenceError: n is not defined(
```

上面代码中，函数f1内部声明的变量n，函数外是无法读取的。

如果出于种种原因，需要得到函数内的局部变量。正常情况下，这是办不到的，只有通过变通方法才能实现。那就是在函数的内部，再定义一个函数。

```js
function f1() {
  var n = 999;
  function f2() {
    console.log(n);
  }
  return f2;
}

var result = f1();
result(); // 999
```

闭包就是将函数内部和函数外部连接起来的一座桥梁。

闭包的最大用处有两个，
一个是可以读取函数内部的变量，
另一个就是让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在。请看下面的例子，闭包使得内部变量记住上一次调用时的运算结果。

```js
function createIncrementor(start) {
  return function () {
    return start++;
  };
}

var inc = createIncrementor(5);

inc() // 5
inc() // 6
inc() // 7
```
上面代码中，start是函数createIncrementor的内部变量。通过闭包，start的状态被保留了，每一次调用都是在上一次调用的基础上进行计算。从中可以看到，闭包inc使得函数createIncrementor的内部环境，一直存在。所以，闭包可以看作是函数内部作用域的一个接口。

为什么会这样呢？原因就在于inc始终在内存中，而inc的存在依赖于createIncrementor，因此也始终在内存中，不会在调用结束后，被垃圾回收机制回收。

闭包的另一个用处，是封装对象的私有属性和私有方法。

```js
function Person(name) {
  var _age;
  function setAge(n) {
    _age = n;
  }
  function getAge() {
    return _age;
  }

  return {
    name: name,
    getAge: getAge,
    setAge: setAge
  };
}

var p1 = Person('张三');
p1.setAge(25);
p1.getAge() // 25
```

外层函数每次运行，都会生成一个新的闭包，而这个闭包又会保留外层函数的内部变量，所以内存消耗很大。因此不能滥用闭包，否则会造成网页的性能问题。

#### 立即调用的函数表达式（IIFE）

在Javascript中，一对圆括号()是一种运算符，跟在函数名之后，表示调用该函数。比如，print()就表示调用print函数。

有时，我们需要在定义函数之后，立即调用该函数。这时，你不能在函数的定义之后加上圆括号，这会产生语法错误。

```js
function(){ /* code */ }();
// SyntaxError: Unexpected token (

```

产生这个错误的原因是，function这个关键字即可以当作语句，也可以当作表达式。


```js
/ 语句
function f() {}

// 表达式
var f = function f() {}
```

为了避免解析上的歧义，JavaScript引擎规定，如果function关键字出现在行首，一律解释成语句。因此，JavaScript引擎看到行首是function关键字之后，认为这一段都是函数的定义，不应该以圆括号结尾，所以就报错了。

解决方法就是不要让function出现在行首，让引擎将其理解成一个表达式。最简单的处理，就是将其放在一个圆括号里面。

```js
(function(){ /* code */ }());
// 或者
(function(){ /* code */ })();
```

上面两种写法都是以圆括号开头，引擎就会认为后面跟的是一个表示式，而不是函数定义语句，所以就避免了错误。这就叫做“立即调用的函数表达式”（Immediately-Invoked Function Expression），简称IIFE。

注意，上面两种写法最后的分号都是必须的。如果省略分号，遇到连着两个IIFE，可能就会报错。

```js
var i = function(){ return 10; }();
true && function(){ /* code */ }();
0, function(){ /* code */ }();
```

甚至像下面这样写，也是可以的。

```js
!function(){ /* code */ }();
~function(){ /* code */ }();
-function(){ /* code */ }();
+function(){ /* code */ }();
```

new关键字也能达到这个效果。

```js
new function(){ /* code */ }

new function(){ /* code */ }()
// 只有传递参数时，才需要最后那个圆括号
```

通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的目的有两个：一是不必为函数命名，避免了污染全局变量；二是IIFE内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量

```js
// 写法一
var tmp = newData;
processData(tmp);
storeData(tmp);

// 写法二
(function (){
  var tmp = newData;
  processData(tmp);
  storeData(tmp);
}());
```
