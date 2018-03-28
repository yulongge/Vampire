# RegExp

> 温故而知新 ~~~

## 定义

JavaScript正则表达式有两种定义方式:

#### 构造函数

  ```js
  var reg = new RegExp('<%[^%>]+%>', 'g');
  ```

#### 字面量

  + `g` : `global` , 全文搜索，默认搜索到第一个结果为止
  + `i` : `ignore case` ,忽略大小写，默认大小写敏感
  + `m` : `multiple lines` ,多行搜索(更改^和$的含义，使他们分别在任意一行对待行首和行尾匹配，而不仅是在整个字符串的开头和结尾匹配)

  ```js
 	 var reg = /<%[^%>]%>/g;
  ```



## 元字符

> 正则表达式让人望而却步的一个重要原因就是其转义字符太多了，组合非常多，但是正则表达式的元字符(在正则表达式中具有特殊意义的专用字符，可以用来规定其前导字符)并不多

元字符：`( [ { \ ^ | ? * + . } ] )`

并不是每个元字符都有其特定意义，在不同的组合中元字符有不同的意义。

| 字符 | 含义 |
| --- | --- |
| \t | 水平制表符 |
| \r | 回车符 |
| \n | 换行符 |
| \f | 换页符 |
| \cX | 与X对应的控制字符(Ctrl + X) |
| \v | 垂直制表符 |
| \0 | 空字符 |

## syntax

#### 字符类

一般情况下正则表达式一个字符(转义字符算一个)对应字符串一个字符。

表达式 ab\t的含义：

![reg](https://yulongge.github.io/images/RegExp/reg1.png)

但是我们可以使用元字符[]来构建一个简单的类，所谓类是指:符合某些特征的对象，是一个泛指，而不是特指某个字符了，我们可以使用表达式[abc],把字符a或b或c归为一类，表达式可以匹配这类的字符

![reg2](https://yulongge.github.io/images/RegExp/reg2.png)

元字符[]组合可以创建一个类，我们还可以使用元字符`^`创建反向类/负向类，反向类的意思是不属于XXX类的内容，表达式`[^abc]`表示不是字符a或b或c的内容

![reg3](https://yulongge.github.io/images/RegExp/reg3.png)

#### 范围类

按照上面的说明要是我们希望匹配单个数字那么表达式是这样的[0123456789],如果是字母那么。。。，这样就太麻烦了，正则表示还提供了范围类，我们可以使用`x-y`来连接两个字符表示从x到y的任意字符，这个闭区间，也就是说包含x和y本身，这样匹配小写字母就很简单了`[a-z]`

![reg4](https://yulongge.github.io/images/RegExp/reg4.png)

要想匹配所有字母呢？在[]组成的类内都是可以连写的，我们可以写成`[a-zA-Z]`

![reg5](https://yulongge.github.io/images/RegExp/reg5.png)

#### 预定义类

刚才使用正则表达式我们创建了几个类，来表示数字，字母等，但这样写也是很麻烦，正则表达式为我们提供了几个常用的预定义类来匹配常见的字符。

| 字符 | 等价类          | 含义                             |
| ---- | --------------- | -------------------------------- |
| .    | [^\n\r]         | 除了回车符和换行符之外的所有字符 |
| \d   | [0-9]           | 数字字符                         |
| \D   | [^0-9]          | 非数字字符                       |
| \s   | [\t\n\x0B\f\r]  | 空白符                           |
| \S   | [^\t\n\x0B\f\r] | 非空白符                         |
| \w   | [a-zA-Z_0-9]    | 单词字符(字母，数字，下划线)     |
| \W   | [^a-zA-Z_0-9]   | 非单词字符                       |


有了这些预定义类，写一些正则就很方便了，比如我们希望匹配一个ab+ 数字 + 任意字符的字符串就可以这样写`ab\d`.

![reg6](https://yulongge.github.io/images/RegExp/reg6.png)

#### 边界

正则表达式还提供了几个常用的边界匹配字符

| 字符 | 含义                               |
| ---- | ---------------------------------- |
| ^    | 以XX开头                           |
| $    | 以XX结尾                           |
| \b   | 单词边界，指[a-zA-Z_0-9]之外的字符 |
| \B   | 非单词边界                         |

看个不负责任的邮箱正则匹配(切勿模仿) \w+@\w+\.(com)$

![reg7](https://yulongge.github.io/images/RegExp/reg7.png)


#### 量词

之前我们介绍的方法都是一一匹配的，如果我们希望匹配一个连续出现20次数字的字符串，难道我们需要写成这样 \d\d\d\d...

为此正则表示引入了一些量词.

| 字符   | 含义                         |
| --- | --- |
| ?      | 出现零次或一次(最多出现一次) |
| +      | 出现一次或者多次             |
| *      | 出现零次或多次               |
| {n}    | 出现n次                      |
| {n, m} | 出现n到m次                   |
| {n,}   | 至少出现n次                  |

看几个量词的demo

`\w+\b Byron 匹配 单词+边界 + Byron`

```js
(/\w+\b Byron/).text('Hi Byron'); //true
(/\w+\b Byron/).text('Welcom Byron'); //true
(/\w+\b Byron/).text('HiByron'); //false
```

`\d+\.\d{1,3}` 匹配三位小数的数字

![reg8](https://yulongge.github.io/images/RegExp/reg8.png)

#### 贪婪模式与非贪婪模式

看了上面介绍的量词，也许爱思考的同学会想到关于匹配原则的一些问题，不如{3,5}这个量词，要是在句子中出现了十次，那么他是每次匹配三个还是五个，反正3，4，5都满足条件，量词在默认下是尽可能多的匹配的，也就是大家常说的贪婪模式.

```js
'123456789'.match(/\d{3,5}/g); //["12345", "6789"]
```

既然有贪婪模式，那么肯定有非贪婪模式，让正则表达式尽可能的少匹配，也就是说一旦成功匹配就不再继续尝试，做法很简单，在量词后加?即可。
	
```js
'123456789'.match(/\d{3,5}?/g); //["123", "456", "789"]
```
	
#### 分组

有时候我们希望使用量词的时候匹配多个字符，而不是像上面例子中只是匹配一个，比如希望匹配Byron出先20次的字符串，我如果写成Byron{20}的话匹配的是Byro + n出现20次，怎么把Byron作为一个整体呢？使用 **()** 就可以达到目的，我们称之为分组`(Byron){20}`

![reg9](https://yulongge.github.io/images/RegExp/reg9.png)

如果希望匹配Byron或者Casper出现20次怎么办呢?可以使用 **|** 达到功效。`(Byron|Casper){20}`

![reg10](https://yulongge.github.io/images/RegExp/reg10.png)

我们看到图中有个#1的东东，那是什么呢？使用分组的正则表达式会把匹配项也放到分组中，默认就是按数字编号分发的，各异根据编号获得捕获的分组内容，这个在一些希望具体操作第几个匹配项的函数中很有用.

![reg11](https://yulongge.github.io/images/RegExp/reg11.png)

如果有分组嵌套的情况，外面的组的编号靠前。

![reg12](https://yulongge.github.io/images/RegExp/reg12.png)

有时候我们不希望捕获某些数组，只需要在分组内加上?:就可以了，这并不意味着该分组内容不属于正则表达式，只是不会给这个分组加编号了而已

![reg13](https://yulongge.github.io/images/RegExp/reg13.png)

其实在C#等语言中分组还可以起名字，不过javascript不支持

#### 前瞻

| 表达式       | 含义                   |
| ------------ | ---------------------- |
| exp1(?=exp2) | 匹配后面是exp2的exp1   |
| exp1(?!exp2) | 匹配后面不是exp2的exp1 |

说的确实有些抽象，来个demo good(?=Byron)

![reg14](https://yulongge.github.io/images/RegExp/reg14.png)

demo

```js
(/good(?=Byron)/).exec('goodByron123'); //['good']
(/good(?=Byron)/).exec('goodCasper123'); //null
(/bad(?=Byron)/).exec('goodCasper123');//null
```
通过上面例子可以看出 exp1(?=exp2) 表达式会匹配exp1表达式，但只有其后面内容是exp2的时候才会匹配，也就是两个条件，exp1(?!exp2) 比较类似

`good(?!Byron)`

![reg15](https://yulongge.github.io/images/RegExp/reg15.png)

demo

```js
(/good(?!Byron)/).exec('goodByron123'); //null
(/good(?!Byron)/).exec('goodCasper123'); //['good']
(/bad(?!Byron)/).exec('goodCasper123');//null
```

## Method

![regexp](https://yulongge.github.io/images/RegExp/regexp.png)

RegExp 实例对象有五个属性

1. global:是否全局搜索，默认是false
2. ignoreCase: 是否大小写敏感，默认是false
3. multiline: 多行搜索，默认是false
4. lastIndex: 是当前表达式模式首次匹配内容最后一个字符的下一个位置，每次正则表达式成功匹配时，lastIndex属性值都会随之改变。
5. source:正则表达式的文本字符串。

	除了将正则表达式编译为内部格式从而使执行更快的complie()方法，对象还有我们常用的方法。


#### regObj.test(strObj)

方法用于测试字符串参数中是否存在正则表达式模式，如果存在返回true，否则返回false.

```js
var reg = /\d+\.\d{1,2}$/g;
reg.test('123.45'); //true
reg.test('0.2') //true
reg.test('a.34') //false
reg.test('34.5678'); //false
```

#### regObj.exec(strObj)

方法用于正则表达式模式在字符串中运行查找，如果exec()找到了匹配的文本，则返回一个结果数组，否则返回null。除了数组元素和length属性之外。exec()方法还返回两个属性，index属性声明的是匹配文本的第一个字符的位置。input属性则存放的是被检索的字符串的string。

调用非全局的RegExp对象的exec()时，返回数组的第0个元素是与正则表达式相匹配的文本，第1个元素是与RegExpObject的第一个表达式相匹配的文本，如果有的话，第2个元素是与RegExp对象的第2个表达式相匹配的文本，一次类推。

调用全局的RegExp对象的 exec() 时，它会在 RegExp实例的 lastIndex 属性指定的字符处开始检索字符串 string。当 exec() 找到了与表达式相匹配的文本时，在匹配后，它将把 RegExp实例的 lastIndex 属性设置为匹配文本的最后一个字符的下一个位置。可以通过反复调用 exec() 方法来遍历字符串中的所有匹配文本。当 exec() 再也找不到匹配的文本时，它将返回 null，并把 lastIndex 属性重置为 0。

```js
var reg=/\d/g;
var r=reg.exec('a1b2c3');
console.log(reg.lastIndex); //2
r=reg.exec('a1b2c3');
console.log(reg.lastIndex); //4
```
两次执行结果：

![regexp](https://yulongge.github.io/images/RegExp/result.jpeg)

```js
var reg=/\d/g;
while(r=reg.exec('a1b2c3')){
	console.log(r.index+':'+r[0]);
}
//结果
//1:1
//3:2
//5:3
```

#### strObj.search(RegObj)

search()方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的字符串。search()方法不执行全局匹配，它将忽略标识g。它同时忽略regexp的lastIndex属性，并且总是从字符串的开始进行检索，这意味着它总是返回stringObject的第一个匹配的位置。

```js
'a1b2c3'.search(/\d/g); //1
'a1b2c3'.search(/\d/); //1
```

#### strObj.match(RegObj)

match() 方法将检索字符串 stringObject，以找到一个或多个与 regexp 匹配的文本。但regexp是否具有标志 g对结果影响很大。

如果 regexp 没有标志 g，那么 match() 方法就只能在 strObj 中执行一次匹配。如果没有找到任何匹配的文本， match() 将返回 null。否则，它将返回一个数组，其中存放了与它找到的匹配文本有关的信息。该数组的第 0 个元素存放的是匹配文本，而其余的元素存放的是与正则表达式的子表达式匹配的文本。除了这些常规的数组元素之外，返回的数组还含有两个对象属性。index 属性声明的是匹配文本的起始字符在 stringObject 中的位置，input 属性声明的是对 stringObject 的引用。

```js
var r='aaa123456'.match(/\d/);
```

如果 regexp 具有标志 g，则 match() 方法将执行全局检索，找到 strObj 中的所有匹配子字符串。若没有找到任何匹配的子串，则返回 null。如果找到了一个或多个匹配子串，则返回一个数组。不过全局匹配返回的数组的内容与前者大不相同，它的数组元素中存放的是 strObj 中所有的匹配子串，而且也没有 index 属性或 input 属性。

```js
var r='aaa123456'.match(/\d/g);
```

![match2](https://yulongge.github.io/images/RegExp/match2.png)

#### strObj.replace(regObj, replaceStr)

关于`string`对象的`replace`方法，我们最常用的是传入两个字符串的做饭，但这个做法有个缺陷，只能replace一次。

```js
'abcabcabc'.replace('bc', 'x'); //axabcabc
```

`replace`方法的第一个参数还可以传入RegExp对象，传入正则表达式可以使replace更加强大灵活。

```js
'abcabcabc'.replace(/bc/g, 'X'); //aXaXaX
'abcabcaBC'.replace(/bc/g, 'X'); //aXaXaX
```

如果replace方法的第一个参数传入的是带分组的正则表达式，我们在第二个参数中可以使用$1...$9来获取相应分组内容，比如希望把字符串:
`1<%2%>34<%567%>89` 的`<%x%>`换为`$#x#$`,我们可以这样:

```js
'1<%2%>34<%567%>89'.replace(/<%(\d+)%>/g,'@#$1#@');
//"1@#2#@34@#567#@89"
```

> 当然还有很多方式可以达到这一目的，这里只是演示一下利用分组内容，我们在第二个参数中使用 `@#$1#@`，其中$1表示被捕获的分组内容，在一些js模板函数中可以经常见到这种方式替换字符串。


#### strObj.replace(regObj, function(){})

可以通过修改replace方法的第二个参数，是replace更加强大，在前面介绍中，只能把所有匹配替换为固定内容，但如果希望把一个字符串中所有的数字，都用小括号包起来改怎么办呢.

```js
'2398rufdjg9w45hgiuerhg83ghvif'.replace(/\d+/g,function(r){
	return '('+r+')';
}); 
//"(2398)rufdjg(9)w(45)hgiuerhg(83)ghvif"
```

把replace方法的第二个参数传入一个function，这个function会在每次匹配替换的时候调用，算是每次替换的回调函数，我们使用了回调函数的第一个参数，也就是匹配的内容，其实回调函数一共有四个参数。

+ 第一个参数，是匹配字符串
+ 第二个参数是正则表达式分组内容，没有分组则没有该参数
+ 第三个参数是匹配项在字符串中的index
+ 第四个参数则是原字符串

```js
'<%1%><%2%><%3%>'.replace(/<%([^%>]+)%>/g,function(a,b,c,d){
	console.log(a+'\t'+b+'\t'+c+'\t'+d);
	return b;
}) //123

//<%1%>    1    0    <%1%><%2%><%3%> 
//<%2%>    2    5    <%1%><%2%><%3%> 
//<%3%>    3    10    <%1%><%2%><%3%> 
```

根据这种参数replace可以实现很多强大的功能，尤其是在复杂的字符串替换语句中经常使用。

#### strObj.split(regObj)

我们经常使用split把字符串分割为字符数组

```js
'a,b,c,d'.split(','); //["a", "b", "c", "d"]
```

和replace方法类似，在一些复杂的分割情况下我们可以使用正则表达式解决

```js
'a1b2c3d'.split(/\d/); //["a", "b", "c", "d"]
```

这样就可以按照数字分割字符串了，是不是很强大。

> 熟读此经，平时用到的javascript正则表达式就游刃有余了.

## 参考文章

- http://www.cnblogs.com/dolphinX/p/3486136.html
- http://www.cnblogs.com/dolphinX/p/3486214.html
