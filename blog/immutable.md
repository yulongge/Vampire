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
`用法` : `value.toJS()`
`简介` : value是要转变的数据

```js

```




### 参考资料

- http://facebook.github.io/immutable-js/docs/#/
- https://segmentfault.com/a/1190000010676878
- https://www.cnblogs.com/3body/p/6224010.html
- https://cythilya.github.io/2017/02/12/immutability-immutablejs-seamless-immutable/
- https://segmentfault.com/a/1190000005920644