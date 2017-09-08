## 献给无法复制的你

![copy](images/array.jpg)

> 数组，对象，对象数组，我该那你们怎么办呢？

> 当跳到坑里时，才发现被你的复制给骗了~~~

![arr1](images/arr1.png)

可以发现对arr2进行修改，arr1也发生了变化，这就是js的浅拷贝模式。

> 对数组，对象，对象数组，进行简单的赋值运算只是创建了一份原有内容的引用，指向的任然是同一块内存区域，修改时会对应修改原内容，而有时候我们的需要独立，彼此互补影响，这就需要对他们进行深拷贝。

- 普通数组
    1. 遍历复制

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

    2. slice()

        > arrayObject.slice(start, end)

        ```js
        arr2 = arr1.slice(0);
        arr2[1] = "c";
        arr1   // => ["a", "b"]
        arr2   // => ["a", "c"]
        ```
