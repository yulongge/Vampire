# Math

Math 是JavaScript的内置对象，提供了一系列数学常数和数学方法。该对象不是构造函数，不能生成实例，所有的属性和方法都必须在Math对象上调用。

## 属性

Math 对象提供了一下一些只读的数学常数。

- Math.PI: 常数Pi
- Math.E: 常数E
- Math.LN2: 2的自然对数
- Math.LN10: 10的自然对数
- Math.LOG2E: 以2为底额e的对数
- Math.LOG10E: 以10为底的e的对数
- Math.SQRT1_2: 0.5的平方根
- Math.SQRT2: 2的平方根

```js
Math.E // 2.718281828459045
Math.LN2 // 0.6931471805599453
Math.LN10 // 2.302585092994046
Math.LOG2E // 1.4426950408889634
Math.LOG10E // 0.4342944819032518
Math.PI // 3.141592653589793
Math.SQRT1_2 // 0.7071067811865476
Math.SQRT2 // 1.4142135623730951
```

## 方法

Math对象提供一些数学方法。
```js
- Math.abs(): 绝对值
- Math.ceil(): 向上取整
- Math.floor(): 向下取整
- Math.round(): 四舍五入
- Math.random(): 随机数(0-1)
- Math.max(): 最大值
- Math.min(): 最小值
- Math.pow(): 指数运算
- Math.sqrt(): 平方根
- Math.log(): 自然对数
- Math.exp(): e的指数
```
- `Math.abs`返回参数的绝对值

    ```js
    Math.abs(-1) //1
    Math.abs(1) //1
    ```

- `Math.ceil` 接受一个参数，返回大于该参数的最小整数

- `Math.floor`方法接受一个参数，返回小于该参数的最大整数

- `Math.round`方法用于四舍五入

    ```js
    Math.ceil(3.2) //4
    Math.ceil(-3.2) //-3

    Math.floor(3.2) //3
    Math.floor(-3.2) //-4

    Math.round(3.2) //3
    Math.round(3.5) //4
    Math.round(-3.2) //-3
    Math.round(-3.8) //-4
    Math.round(-3.5) //-3
    ```

    衍生：`toInteger()`: 总是返回一个数值的整数部分

    ```js
    function toInteger(x) {
        x = Number(x);
        return x < 0 ? Math.ceil(x) : Math.floor(x);
    }

    toInteger(3.2) //3
    toInteger(-3.2) //-3
    ```

- `Math.random` 返回0-1之间的一个伪随机数，可能等于0，但是一定小于1`[0,1)`

    ```js
    Math.random() // 0.7151307314634323
    ```

    衍生：

    + `任意范围的随机数生成函数`

        ```js
        function getRandomRange(min, max) {
            return Math.random() * (max - min) + min;
        }
        getRandomRange(1.5, 6.5); // 2.4942810038223864
        ```

    + `任意范围的随机整数生成函数`

        ```js
        function getRandomInt(min, max) {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }
        getRandomInt(1, 6) //5
        ```

    + `返回随机字符`

        ```js
        function random_str(length) {
            var ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
                ALPHABET += 'abcdefghijklmnopqrstuvwxyz';
                ALPHABET += '0123456789-_';
            var str = '';
            for (var i=0; i < length; ++i) {
                var rand = Math.floor(Math.random() * ALPHABET.length);
                str += ALPHABET.substring(rand, rand + 1);
            }
            return str;
        }

        random_str(6) // "NdQKOr"
        ```

- `Math.max` 和 `Math.min` 接受多个参数，返回最大值和最小值。有趣的是，`Math.min`不传参返回`Infinity`, `Math.max`不传参返回 `-Infinity`

    ```js
    Math.max(2, -1, 5) // 5
    Math.min(2, -1, 5) // -1
    Math.min() // Infinity
    Math.max() // -Infinity
    ```

- `Math.pow` 方法返回以第一个参数为底数、第二个参数为幂的指数值.

    ```js
    Math.pow(2, 2) // 4
    Math.pow(2, 3) // 8

    //计算圆的面积
    var radius = 20;
    var area = Math.PI * Math.pow(radius, 2);
    ```

- Math.sqrt方法返回参数值的平方根。如果参数是一个负值，则返回NaN

    ```js
    Math.sqrt(4) // 2
    Math.sqrt(-4) // NaN
    ```

- `Math.log`方法返回以e为底的自然对数值

    ```js
    Math.log(Math.E) // 1
    Math.log(10) // 2.302585092994046
    ```

- `Math.exp`方法返回常数e的参数次方

    ```js
    Math.exp(1) // 2.718281828459045
    Math.exp(3) // 20.085536923187668   
    ```
- 三角函数
    > Math对象还提供一系列三角函数方法

    ```js
    Math.sin(0) // 返回参数的正弦 : 0
    Math.cos(0) // 返回参数的余弦: 1
    Math.tan(0) // 返回参数的正切: 0
    Math.asin(1) // 返回参数的反正弦（弧度值）: 1.5707963267948966
    Math.acos(1) // 返回参数的反余弦（弧度值）: 0
    Math.atan(1) // 返回参数的反正切（弧度值）: 0.7853981633974483
    ```
