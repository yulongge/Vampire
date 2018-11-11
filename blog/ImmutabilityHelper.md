### Immutability Helpers

Mutate a copy of data without changing the original source

> This is a drop-in replacement for react-addons-update

update is a legacy add-on. Use immutability-helper instead.

> React.addons entry point is deprecated as of React v15.5. The add-ons have moved to separate modules, and some of them have been deprecated.

ago 

```js
import update from 'react-addons-update'; // ES6
var update = require('react-addons-update'); // ES5 with npm
```

now 

```js
import update from 'immutability-helper';
 
const state1 = ['x'];
const state2 = update(state1, {$push: ['y']}); // ['x', 'y']
```

Note that this module has nothing to do with React. However, since this module is most commonly used with React, the docs will focus on how it can be used with React.

#### Install

```js
npm install immutability-helper --save
```

#### Overview

React让你可以使用任何你想要的数据管理风格，包括数据可变风格。然而，如果你能够在你应用中讲究性能的部分使用不可变数据，就可以很方便地实现一个快速的shouldComponentUpdate()方法来显著提升你应用的速度。

在JavaScript中处理不可变数据比在语言层面上就设计好要难，像 Clojure。但是，我们提供了一个简单的不可变辅助工具，update()，这就让处理这种类型的数据更加简单了，根本不会改变你数据的表示的形式

#### The main idea

如果你像这样改变数据

```Js
myData.x.y.z = 7;
// or...
myData.a.b.push(9);
```

你无法确定哪个数据改变了，因为之前的副本被覆盖了。相反，你需要创建一个新的myDate副本，仅仅改变需要改变的部分。然后你就能够在shouldComponentUpdate()中使用第三方的相等判断来比较myData的旧副本和新对象：

```js
var newData = deepCopy(myData);
newData.x.y.z = 7;
newData.a.b.push(9);
```

不幸的是，深拷贝是很昂贵的，而且某些时候还不可能完成。你可以通过仅拷贝需要改变的对象，重用未改变的对象来缓解这个问题。不幸的是，在当今的JavaScript里面，这会变得很笨拙：

```js
var newData = extend(myData, {
  x: extend(myData.x, {
    y: extend(myData.x.y, {z: 7}),
  }),
  a: extend(myData.a, {b: myData.a.b.concat(9)})
});
```

虽然这能够非常好地提升性能（因为仅仅浅复制log n个对象，重用余下的），但是写起来很痛苦。看看所有的重复书写！这不仅仅是恼人，也提供了一个巨大的出bug的区域。

update()在这种情形下提供了简单的语法糖，使得写这种代码变得更加简单。代码变为：

```js
var newData = update(myData, {
  x: {y: {z: {$set: 7}}},
  a: {b: {$push: [9]}}
});
```

虽然这种语法糖需要花点精力适应（尽管这是受MongoDB's query language的启发），但是它没有冗余，是静态可分析的，并且比可变的版本少打了很多字

`以$为前缀的键被称作命令。他们“改变”的数据结构被称为目标`

#### Available commands

- `{$push: array}` `push()` all the items in array on the target.
- `{$unshift: array}` `unshift()` all the items in array on the target.
- `{$splice: array of arrays}` for each item in arrays call `splice`() on the target with the parameters provided by the item. 
`Note:` The items in the array are applied sequentially, so the order matters. The indices of the target may change during the operation.
- `{$set: any}` `replace` the target entirely.
- `{$toggle: array of strings}` `toggles` a list of boolean fields from the target object.
- `{$unset: array of strings}` `remove` the list of keys in array from the target object.
- `{$merge: object}` `merge` the keys of object with the target.
- `{$apply: function}` passes in the current value to the function and updates it with the new returned value.
- `{$add: array of objects}` `add` a value to a Map or Set. When adding to a Set you pass in an array of objects to add, when adding to a Map, you pass in [key, value] arrays like so: 
`update- (myMap, {$add: [['foo', 'bar'], ['baz', 'boo']]})`
- `{$remove: array of strings}` `remove` the list of keys in array from a Map or Set.

##### Shorthand $apply syntax

Additionally, instead of a command object, you can pass a function, and it will be treated as if it was a command object with the $apply command: `update({a: 1}, {a: function})`. 

That example would be equivalent to `update({a: 1}, {a: {$apply: function}})`.

#### Examples

`push`

```js
const initialArray = [1, 2, 3];
const newArray = update(initialArray, {$push: [4]}); // => [1, 2, 3, 4]
```

> initialArray is still [1, 2, 3].

`Nested collections`

```js
const collection = [1, 2, {a: [12, 17, 15]}];
const newCollection = update(collection, {2: {a: {$splice: [[1, 1, 13, 14]]}}});
// => [1, 2, {a: [12, 13, 14, 15]}]
```

This accesses collection's index 2, key a, and does a splice of one item starting from index 1 (to remove 17) while inserting 13 and 14.

Updating a value based on its current one

```js
const obj = {a: 5, b: 3};
const newObj = update(obj, {b: {$apply: function(x) {return x * 2;}}});
// => {a: 5, b: 6}
// This is equivalent, but gets verbose for deeply nested collections:
const newObj2 = update(obj, {b: {$set: obj.b * 2}});
```

`Merge`

```js
const obj = {a: 5, b: 3};
const newObj = update(obj, {$merge: {b: 6, c: 7}}); // => {a: 5, b: 6, c: 7}
```

Computed Property Names

Arrays can be indexed into with runtime variables via the ES2015 Computed Property Names feature. An object property name expression may be wrapped in brackets [] which will be evaluated at runtime to form the final property name.

```js
const collection = {children: ['zero', 'one', 'two']};
const index = 1;
const newCollection = update(collection, {children: {[index]: {$set: 1}}});
// => {children: ['zero', 1, 'two']}
```


#### Reference resources

- http://wiki.jikexueyuan.com/project/react/immutability-helpers.html
- https://www.npmjs.com/package/immutability-helper
- https://reactjs.org/docs/addons.html






