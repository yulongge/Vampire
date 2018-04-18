requestAnimationFrame中游走

requestAnimationFrame已经来了很久了，一起来唠嗑哈。

> window.requestAnimationFrame() 将告知浏览器你马上要开始动画效果了，后者需要在下次动画前调用相应方法来更新画面。这个方法就是传递给window.requestAnimationFrame()的回调函数。

`long long ago`

以前我们在js中做动画，无外乎setTimeout 和 setInterval.但问题来了：

- 如何确定正确的时间间隔（浏览器、机器硬件的性能各不相同）？
- 毫秒的不精确性怎么解决？
- 如何避免过度渲染（渲染频率太高、tab 不可见等等）？

> 开发者可以用很多方式来减轻这些问题的症状，但是彻底解决，这个、基本、很难。

归根到底，问题的根源在于**时机**。

对于前端开发者来说，setTimeout 和 setInterval 提供的是一个等长的定时器循环（timer loop），但是对于浏览器内核对渲染函数的响应以及何时能够发起下一个动画帧的时机，是完全不了解的。

对于浏览器内核来讲，它能够了解发起下一个渲染帧的合适时机，但是对于任何 setTimeout 和 setInterval 传入的回调函数执行，都是一视同仁的，它很难知道哪个回调函数是用于动画渲染的，因此，优化的时机非常难以掌握。悖论就在于，写 JavaScript 的人了解一帧动画在哪行代码开始，哪行代码结束，却不了解应该何时开始，应该何时结束，而在内核引擎来说，事情却恰恰相反，所以二者很难完美配合，直到 requestAnimationFrame 出现。

requestAnimationFrame 这个名字很好，因为起得非常直白 – request animation frame，对于这个 API 最好的解释就是名字本身了。这样一个 API，你传入的 API 不是用来渲染一帧动画，你上街都不好意思跟人打招呼。

### 基本用法

可以直接调用，也可以通过window来调用，接收一个函数作为回调，返回一个ID值，通过把这个ID值传给window.cancelAnimationFrame()可以取消该次动画

```js
requestAnimationFrame(callback)//callback为回调函数
```

#### Demo

```js
function animationWidth() {
  var div = document.getElementById('box');
  div.style.width = parseInt(div.style.width) + 1 + 'px';

  if(parseInt(div.style.width) < 200) {
    requestAnimationFrame(animationWidth)
  }
}
requestAnimationFrame(animationWidth);
```

![requestanimal](https://yulongge.github.io/images/vampire/requestanimal.gif)

可以看到，requestAnimationFrame接受一个动画执行函数作为参数，这个函数的作用是仅执行一帧动画的渲染，并根据条件判断是否结束，如果动画没有结束，则继续调用requestAnimationFrame并将自身作为参数传入。从示例来看，得到了效果平滑流畅的动画，这样就巧妙地避开了每一帧动画渲染的时间间隔问题

#### 兼容性

![caniuse](https://yulongge.github.io/images/vampire/caniuse.png)

针对于低版本浏览器

- 用setTimeout，setInterval实现
- 安装相应的polyfill

#### 原理

requestAnimationFrame 的实现原理

- 注册回调函数
- 浏览器更新时触发 animate
- animate 会触发所有注册过的 callback

这里的工作机制可以理解为所有权的转移，把触发帧更新的时间所有权交给浏览器内核，与浏览器的更新保持同步。这样做既可以避免浏览器更新与动画帧更新的不同步，又可以给予浏览器足够大的优化空间

### 优势

requestAnimationFrame还有以下优势：

- 与setTimeout相比，requestAnimationFrame最大的优势是由系统来决定回调函数的执行时机。具体一点讲，如果屏幕刷新率是60Hz,那么回调函数就每16.7ms被执行一次，如果刷新率是75Hz，那么这个时间间隔就变成了1000/75=13.3ms，换句话说就是，requestAnimationFrame的步伐跟着系统的刷新步伐走。它能保证回调函数在屏幕每一次的刷新间隔中只被执行一次，这样就不会引起丢帧现象，也不会导致动画出现卡顿的问题
- `CPU节能`：使用setTimeout实现的动画，当页面被隐藏或最小化时，setTimeout 仍然在后台执行动画任务，由于此时页面处于不可见或不可用状态，刷新动画是没有意义的，完全是浪费CPU资源。而requestAnimationFrame则完全不同，当页面处理未激活的状态下，该页面的屏幕刷新任务也会被系统暂停，因此跟着系统步伐走的requestAnimationFrame也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了CPU开销。
- `函数节流`：在高频率事件(resize,scroll等)中，为了防止在一个刷新间隔内发生多次函数执行，使用requestAnimationFrame可保证每个刷新间隔内，函数只被执行一次，这样既能保证流畅性，也能更好的节省函数执行的开销。一个刷新间隔内函数执行多次时没有意义的，因为显示器每16.7ms刷新一次，多次绘制并不会在屏幕上体现出来。

### 参考

- http://web.jobbole.com/91578/
- https://www.cnblogs.com/Wayou/p/requestAnimationFrame.html
- https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame



