### immutable

> Immutable data encourages pure functions (data-in, data-out) and lends itself to much simpler application development and enabling techniques from functional programming such as lazy evaluation.

Immutable数据就是一旦创建，就不能更改的数据。每当对Immutable对象进行修改的时候，就会返回一个新的Immutable对象，以此来保证数据的不可变

有人说 Immutable 可以给 React 应用带来数十倍的提升，也有人说 Immutable 的引入是近期 JavaScript 中伟大的发明，因为同期 React 太火，它的光芒被掩盖了。这些至少说明 Immutable 是很有价值的。

##### 我们为什么需要不可更改是数据？

###### Mutable

JavaScript 中的对象一般是可变的（Mutable），因为使用了引用赋值，新的对象简单的引用了原始对象，改变新的对象将影响到原始对象

> 除了基本的类型之外

```js
var obj = {
  a: 1,
  b: 2
};

var obj1 = obj;

obj1.a = 999;
obj.a //999
```

改变了obj1.a的值，同时也会更改到obj.a的值。这样共享的好处就是节省记忆体，坏处就是稍不注意会导致「改A坏B 」的棘手问题.

一般的解法就是使用「深拷贝」(deep copy)而非浅拷贝(shallow copy)，来避免被修改,但是这样造成了 CPU和内存的浪费.

> 对数组，对象，对象数组，进行简单的赋值运算只是创建了一份原有内容的引用，指向的任然是同一块内存区域，修改时会对应修改原内容，而有时候我们的需要独立，彼此互补影响，这就需要对他们进行深拷贝。

###### 普通数组

1.遍历复制

```js
var arr1 = ["a", "b"], arr2 = [];
for (var item in arr1) arr2[item] = arr1[item];
arr2[1] = "c";
arr1   // => ["a", "b"]
arr2   // => ["a", "c"]
```
这种方式简单粗暴，考虑到多维数组，可以用递归来实现

```js
function arrDeepCopy(source){
    var sourceCopy = [];
    for (var item in source) {
        sourceCopy[item] = typeof source[item] === "object" ? arrDeepCopy(source[item]) : source[item]
    }
    return sourceCopy;
}
```

2.slice()

> arrayObject.slice(start, end)，方法返回一个新的数组，包含从 start 到 end （不包括该元素）的 arrayObject 中的元素。该方法并不会修改数组，而是返回一个子数组。

```js
arr2 = arr1.slice(0);
arr2[1] = "c";
arr1   // => ["a", "b"]
arr2   // => ["a", "c"]
```
3.concat()

> arrayObject.concat(arrayX,arrayX,......,arrayX), 该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。

```js
arr2 = arr1.concat();
arr2[1] = "c";
arr1   // => ["a", "b"]
arr2   // => ["a", "c"]
```
###### 对象

1.浅拷贝

```js
var obj = { "a": 1, "b": 2 };
```

```js
var objCopy = obj;
objCopy.b = 3;
obj   // => { "a": 1, "b": 3 }
objCopy   // => { "a": 1, "b": 3 }
```
> 同样，简单的赋值运算只是创建了一份浅拷贝

2.深拷贝

> 而对于对象的深拷贝，没有内置方法可以使用，我们可以自己命名一个函数进行这一操作

```js
var objDeepCopy = function(source){
    var sourceCopy = {};
    for (var item in source) sourceCopy[item] = typeof source[item] === 'object' ? objDeepCopy(source[item]) : source[item];
    return sourceCopy;
}
var objCopy = objDeepCopy(obj);
objCopy.a.a1[1] = "a13";
obj   // => { "a": { "a1": ["a11", "a12"], "a2": 1 }, "b": 2 }
objCopy   // => { "a": { "a1": ["a11", "a13"], "a2": 1 }, "b": 2 }
```

对象数组(复杂型)

```js
var objDeepCopy = function (source) {
    var sourceCopy = source instanceof Array ? [] : {};
    for (var item in source) {
        sourceCopy[item] = typeof source[item] === 'object' ? objDeepCopy(source[item]) : source[item];
    }
    return sourceCopy;
}
var objCopy = objDeepCopy(obj);
objCopy[0].a.a1[1] = "a13";
objCopy[1][1].e = "6";
obj   // => [{ "a": { "a1": ["a11", "a12"], "a2": 1 }, "b": 2 }, ["c", { "d": 4, "e": 5 }]]
objCopy   // => [{ "a": { "a1": ["a11", "a13"], "a2": 1 }, "b": 2 }, ["c", { "d": 4, "e": 6 }]]
```

日常用的库函数

Object.assign()

> es6语法,copy可枚举的属性，也是一种浅copy

```js
  Object.assign(target, ...sources);
```

_clone

> lodash方法,默认浅copy,第二个参数可以设置深浅

```js
  _clone(value, [isDeep], [customizer], [thisArg]);
```

- _cloneDeep

> lodash方法，深copy

```js
  _cloneDeep(value, [customizer], [thisArg]);
```

extend

> jquery方法，第一个参数可以设置是否深浅copy

```js
  jQuery.extend(true, { a : { a : "a" } }, { a : { b : "b" } } );
  jQuery.extend( { a : { a : "a" } }, { a : { b : "b" } } );
```


##### Immutable 可以很好地解决这些问题

### 什么是IMMUTABLE DATA

Immutable Data 就是一旦创建，就不能再被更改的数据。对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。Immutable 实现的原理是 Persistent Data Structure（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 Structural Sharing（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。

![immutable](https://camo.githubusercontent.com/9e129aaf95d2a645a860dc26532796817e8085c0/687474703a2f2f696d672e616c6963646e2e636f6d2f7470732f69322f5442317a7a695f4b5858585858637458465858627262384f5658582d3631332d3537352e676966)


### API

Immutable 的几种数据类型

- List: 有序索引集，类似JavaScript中的Array。
- Map: 无序索引集，类似JavaScript中的Object。
- OrderedMap: 有序的Map，根据数据的set()进行排序。
- Set: 没有重复值的集合。
- OrderedSet: 有序的Set，根据数据的add进行排序。
- Stack: 有序集合，支持使用unshift（）和shift（）添加和删除。
- Range(): 返回一个Seq.Indexed类型的集合，这个方法有三个参数，start表示开始值，默认值为0，end表示结束值，默认为无穷大，step代表每次增大的数值，默认为1.如果start = end,则返回空集合。
- Repeat(): 返回一个vSeq.Indexe类型的集合，这个方法有两个参数，value代表需要重复的值，times代表要重复的次数，默认为无穷大。
- Record: 一个用于生成Record实例的类。类似于JavaScript的Object，但是只接收特定字符串为key，具有默认值。
- Seq: 序列，但是可能不能由具体的数据结构支持。
- Collection: 是构建所有数据结构的基类，不可以直接构建。

> 用的最多就是List和Map，所以在这里主要介绍这两种数据类型的API

`1. fromJS()`

`作用` : 将一个js数据转换为Immutable类型的数据
`用法` : `fromJS(value, converter)`
`简介` : value是要转变的数据，converter是要做的操作。第二个参数可不填，默认情况会将数组准换为List类型，将对象转换为Map类型，其余不做操作。

```js
const obj = Immutable.fromJS({a:'123',b:'234'},function (key, value, path) {
    console.log(key, value, path)
    return isIndexed(value) ? value.toList() : value.toOrderedMap())
})
```

`2. toJS()`

`作用` : 将一个Immutable数据转换为JS类型的数据
`用法` : `value.toJS()`
`简介` : value是要转变的数据

```js

```

`3. is()`

`作用` : 对两个对象进行比较
`用法` : `is(map1,map2)`
`简介` : 和js中对象的比较不同，在js中比较两个对象比较的是地址，但是在Immutable中比较的是这个对象hashCode和valueOf，只要两个对象的hashCode相等，值就是相同的，避免了深度遍历，提高了性能

```js
import { Map, is } from 'immutable'
const map1 = Map({ a: 1, b: 1, c: 1 })
const map2 = Map({ a: 1, b: 1, c: 1 })
map1 === map2   //false
Object.is(map1, map2) // false
is(map1, map2) // true
```

`4. List`

`作用` : 用来创建一个新的List
`用法` : 

```js
List<T>(): List<T>
List<T>(iter: Iterable.Indexed<T>): List<T>
List<T>(iter: Iterable.Set<T>): List<T>
List<K, V>(iter: Iterable.Keyed<K, V>): List<any>
List<T>(array: Array<T>): List<T>
List<T>(iterator: Iterator<T>): List<T>
List<T>(iterable: Object): List<T>
```

`简介` : List() 是一个构造方法，可以用于创建新的 List 数据类型，上面代码演示了该构造方法接收的参数类型，此外 List 拥有两个静态方法：

- List.isList(value)，判断 value 是否是 List 类型
- List.of(...values)，创建包含 ...values 的列表

```js
// 1. 查看 List 长度
const $arr1 = List([1, 2, 3]);
$arr1.size
// => 3

// 2. 添加或替换 List 实例中的元素
// set(index: number, value: T)
// 将 index 位置的元素替换为 value，即使索引越界也是安全的
const $arr2 = $arr1.set(-1, 0);
// => [1, 2, 0]
const $arr3 = $arr1.set(4, 0);
// => [ 1, 2, 3, undefined, 0 ]

// 3. 删除 List 实例中的元素
// delete(index: number)
// 删除 index 位置的元素
const $arr4 = $arr1.delete(1);
// => [ 1, 3 ]

// 4. 向 List 插入元素
// insert(index: number, value: T)
// 向 index 位置插入 value
const $arr5 = $arr1.insert(1, 1.5);
// => [ 1, 1.5, 2, 3 ]

// 5. 清空 List
// clear()
const $arr6 = $arr1.clear();
// => []
```

`5. Map`

`作用` : Map 可以使用任何类型的数据作为 Key 值，并使用 Immutable.is() 方法来比较两个 Key 值是否相等
`简介` : Map() 是 Map 类型的构造方法，行为类似于 List()，用于创建新的 Map 实例，此外，还包含两个静态方法：Map.isMap() 和 Map.of()。下面介绍几个 Map 实例的常用操作

```js
// 1. Map 实例的大小
const $map1 = Map({ a: 1 });
$map1.size
// => 1

// 2. 添加或替换 Map 实例中的元素
// set(key: K, value: V)
const $map2 = $map1.set('a', 2);
// => Map { "a": 2 }

// 3. 删除元素
// delete(key: K)
const $map3 = $map1.delete('a');
// => Map {}

// 4. 清空 Map 实例
const $map4 = $map1.clear();
// => Map {}

// 5. 更新 Map 元素
// update(updater: (value: Map<K, V>) => Map<K, V>)
// update(key: K, updater: (value: V) => V)
// update(key: K, notSetValue: V, updater: (value: V) => V)
const $map5 = $map1.update('a', () => (2))
// => Map { "a": 2 }

// 6. 合并 Map 实例
const $map6 = Map({ b: 2 });
$map1.merge($map6);
// => Map { "a": 1, "b": 2 }
```

OrderedMap 是 Map 的变体，它除了具有 Map 的特性外，还具有顺序性，当开发者遍历 OrderedMap 的实例时，遍历顺序为该实例中元素的声明、添加顺序。

`6. Set`

Set 和 ES6 中的 Set 类似，都是没有重复值的集合，OrderedSet 是 Set 的遍历，可以保证遍历的顺序性。


```js
// 1. 创建 Set 实例
const $set1 = Set([1, 2, 3]);
// => Set { 1, 2, 3 }

// 2. 添加元素
const $set2 = $set1.add(1).add(4);
// => Set { 1, 2, 3, 4 }

// 3. 删除元素
const $set3 = $set1.delete(3);
// => Set { 1, 2 }

// 4. 并集
const $set4 = Set([2, 3, 4, 5, 6]);
$set1.union($set1);
// => Set { 1, 2, 3, 4, 5, 6 }

// 5. 交集
$set1.intersect($set4);
// => Set { 3, 2 }

// 6. 差集
$set1.subtract($set4);
// => Set { 1 }
```

`7. Stack`

Stack 是基于 Signle-Linked List 实现的可索引集合，使用 unshift(v) 和 shift() 执行添加和删除元素的复杂度为 O(1)。

```js
// 1. 创建 Stack 实例
const $stack1 = Stack([1, 2, 3]);
// => Stack [ 1, 2, 3 ]

// 2. 取第一个元素
$stack1.peek()
// => 1

// 2. 取任意位置元素
$stack1.get(2)
// => 3

// 3. 判断是否存在
$stack1.has(10)
// => false
```

`8. Range() 和 Repeat()`

Range(start?, end?, step?) 接收三个可选参数，使用方法如下：

```js
// 1. 不传参
Range();
// => Range [ 0...Infinity ]

// 2. 设置 start 起点
Range(10);
// => Range [ 10...Infinity ]

// 3. 设置 start 起点和 end 终点
Range(10, 20);
// => Range [ 10, 11, 12, 13, 14, 15, 16, 17, 18, 19 ]

// 4. 设置 start 起点、end 终点和 step 步长
Range(10, 20, 3);
// => Range [ 10, 13, 16, 19 ]
```

Repeat(value, times?) 接收两个参数，其中 times 重复次数是可选参数：

```js
Repeat('foo');
// => Repeat [ foo Infinity times ]

Repeat('foo', 3);
// => Repeat [ foo 3 times ]
```

类似 Range() 和 Repeat(value) 这样生成无限长度集合的操作，内部都存在惰性计算的机制，只有真实取值时才会生成相应的结果。使用 ES6 中的 Generator 函数，可以轻松实现一个惰性计算

```js
for (let i = 0; i < 10; i++) {
    console.log(arr.next());
}
// bigArr(0): 0
// => { value: 0, done: false }
// => bigArr(1): 1
// => { value: 1, done: false }
// => bigArr(2): 2
// => { value: 2, done: false }
// => bigArr(3): 3
// => { value: 3, done: false }
// => bigArr(4): 4
// => { value: 4, done: false }
// => bigArr(5): 5
// => { value: 5, done: false }
// => bigArr(6): 6
// => { value: 6, done: false }
// => bigArr(7): 7
// => { value: 7, done: false }
// => bigArr(8): 8
// => { value: 8, done: false }
// => bigArr(9): 9
// => { value: 9, done: false }
```

`9. Record`

Record 在表现上类似于 ES6 中的 Class，但在某些细节上还有所不同。通过 Record() 可以创建一个新的 Record 类，使用该类可以创建具体的 Record 实例，该实例包含在 Record() 构造函数中声明的所有属性和默认值。如果 Record 实例中的某个属性被删除了，则只会讲实例中的属性值恢复为默认值：

```js
// 1. 创建 Record 实例
const A = Record({ a: 1, b: 2 });
const r = new A({ a: 3 });
// => Record { "a": 3, "b": 2 }

// 2. 删除实例属性
const rr = r.remove('a');
// => Record { "a": 1, "b": 2 }
```

此外，Record 实例还具有扩展性

```js
class ABRecord extends Record({a:1,b:2}) {
  getAB() {
    return this.a + this.b;
  }
}

var myRecord = new ABRecord({b: 3})
myRecord.getAB()
// => 4
```

`10. Seq`

Seq 有两个特点：immutable，一旦创建就不能被修改；lazy，惰性求值。在下面的代码中，虽然组合了多种遍历操作，但实际上并不会有任何的求值操作，只是纯粹的声明一个 Seq：

```js
var oddSquares = Immutable.Seq.of(1,2,3,4,5,6,7,8)
    .filter(x => x % 2)
    .map(x => x * x);
```

如果要从 oddSquares 中取出索引为 1 的元素，则执行过程为

```js
console.log(oddSquares.get(1));

// filter(1)
// filter(2)
// filter(3)
// map(3)
// => 9
```
默认情况下 Seq 的惰性计算结果不会被缓存，比如在下面的代码中，由于每个 join() 都会遍历执行 map，所以 map 总共执行了六次

```js
var squares = Seq.of(1,2,3).map(x => x * x);
squares.join() + squares.join();
```

如果开发者知道 Seq 的结果会被反复用到，那么就可以使用 cacheResult() 将惰性计算的结果保存到内存中

```js
var squares = Seq.of(1,2,3).map(x => x * x).cacheResult();
squares.join() + squares.join();
```

`11. Iterable 和 Collection`

Iterable 是键值对形式的集合，其实例可以执行遍历操作，是 immutable.js 中其他数据类型的基类，所有扩展自 Iterable 的数据类型都可以使用 Iterable 所声明的方法，比如 map 和 filter 等。

Collection 是 Concrete Data Structure 的基类，使用该类时需要至少继承其子类中的一个：Collection.Keyed / Collection.Indexed / Collection.Set

### React

在 React 官方文档的《Advanced Performance》 一节中，专门对 React 的性能瓶颈、优化方式做了详细的解析。当一个 React 组件的 props 和 state 发生变化时，React 会根据变化后的 props 和 state 创建一个新的 virtual DOM，然后比较新旧两个 vritual DOM 是否一致，只有当两者不同时，React 才会将 virtual DOM 渲染真实的 DOM 结点，而对 React 进行性能优化的核心就是减少渲染真实 DOM 结点的频率，间接地指出开发者应该准确判断 props 和 state 是否真正发生了变化。

在比对新旧 vritual DOM 和渲染真实 DOM 前，React 为开发者提供了 shouldComponentUpdate() 方法中断接下来的比对和渲染操作，默认情况下，该方法总会返回 true，如果它返回 false，则不执行比对和渲染操作：

```js
// 最简单的实现：
shouldComponentUpdate (nextProps) {
    return this.props.value !== nextProps.value;
}
```

看起来挺简单，实在不然。当我们需要比对的值是对象、数组等引用值时，就会出现问题

```js
// 假设 this.props.value 是 { foo: 'bar' }
// 假设 nextProps.value 是 { foo: 'bar' },
// 显然这两者引用的内存地址不同，但它们具有相同的值，这种时候不应该继续执行渲染
this.props.value !== nextProps.value; // true
```

如果数据是 Immutable Data 的话，那么数据发生变化就会生成新的对象，开发者只需要检查对象应用是否发生变化即可

```js
var SomeRecord = Immutable.Record({ foo: null });
var x = new SomeRecord({ foo: 'bar'  });
var y = x.set('foo', 'baz');
x === y; // false
```

处理这一问题的另一种方式是通过 setter 设置 flag 对脏数据进行检查，但冗杂的代码是在让人头疼

### 总结

Immutable.js 所提供的 Immutable Data 和 JavaScript 固有的 Mutable Data 各有优势，未来 ECAMScript 有可能制定一套原生的 Immutable Data 规范，在这之前，Immutable.js 是一个不错的选择。之前已经写文章熟悉过 Lodash 这一工具库，Immutable 内部也封装了诸多常用的数据操作函数，所以如果让我来选择的话，在 React 技术栈中我会更偏爱 Immutable。


### 参考资料

- http://facebook.github.io/immutable-js/docs/#/
- https://segmentfault.com/a/1190000010676878
- https://www.cnblogs.com/3body/p/6224010.html
- https://cythilya.github.io/2017/02/12/immutability-immutablejs-seamless-immutable/
- https://segmentfault.com/a/1190000005920644