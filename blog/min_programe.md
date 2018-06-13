### 小程序 - 填坑之路

> 你能走多远，取决于你填坑的能力有多大!

当然，挖坑者和填坑者经常是同一个人，so...我们开始吧。。。

##### 代码构成

###### json

1.app.json

`pages`: 接受一个数组，来指定页面路径。数组的第一项代表小程序的初始页面(即默认页面)
> 文件名不需要些后缀

`window`: 设置小程序的状态栏、导航条、标题、窗口背景色。

- navigationBarTextStyle: 仅支持 black/white
- navigationBarTitleText: 超过字符串长度，小程序会自动加入省略号
- navigationStyle: 只能在app.json设置

`tabBar`: tab应用，可以用tabBar配置项指定tab栏的表现

- 当设置 position 为 top 时，将不会显示 icon
- tabBar 中的 list 是一个数组，只能配置最少2个、最多5个 tab，tab 按数组的顺序排序

`networkTimeout`: 设置各种网络请求的超时时间

- 默认为60000ms
- request, connectSocket, uploadFile, downloadFile的设置

2.page.json

> 只是设置`app.json`中window配置项内容


###### wxml

数据绑定

> wxml中数据绑定用了Mustache语法(双大括号)将变量抱起来。

`关键字(需要在双引号之内)`

```html
<checkbox checked="false"> </checkbox>

<checkbox checked="{{false}}"> </checkbox>
```

> 不要直接写 checked="false"，其计算结果是一个字符串，转成 boolean 类型后代表真值

```html
<view wx:for="{{[1,2,3]}} ">
  {{item}}
</view>

<!--equal -->
<view wx:for="{{[1,2,3] + ' '}}">
  {{item}}
</view>
```
> 花括号和引号之间如果有空格，将最终被解析成为字符串

列表渲染

> 当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。

`wx:for`可以循环数组，对象，字符串

条件渲染

> wx:if 的条件值切换时，框架有一个局部渲染的过程，因为它会确保条件块在切换时销毁或重新渲染

`vs hidden`

一般来说，wx:if 有更高的切换消耗而 hidden 有更高的初始渲染消耗。因此，如果需要频繁切换的情景下，用 hidden 更好，如果在运行时条件不大可能改变则 wx:if 较好


dataset获取

在组件中可以定义数据，这些数据将会通过事件传递给 SERVICE。 书写方式： 以data-开头，多个单词由连字符-链接，不能有大写(大写会自动转成小写)如data-element-type，最终在 event.currentTarget.dataset 中会将连字符转成驼峰elementType

###### wxss

WXSS 的用法类似于 CSS，并在 CSS 的基础上，扩展了 rpx 尺寸单位和样式导入功能。

WXSS 可以使用内联样式，但这样会影响渲染速度。

每个页面自己的 page.wxss 样式表，会覆盖全局样式表 app.wxss。

```
Q：setData方法是有react那样的虚拟dom优化吗？`
A：有做虚拟DOM的优化，但设置相同数据还是会触发新渲染的。
```


###### js

小程序的逻辑层由 JavaScript 语言完成。但因为小程序不在浏览器中运行，所以 JS 在 web 浏览器中的一些函数不能用，如 document、window 等。

> 小程序不支持cookie

###### wxs

> WXS 代码可以编写在 wxml 文件中的 <wxs> 标签内，或以 .wxs 为后缀名的文件内。

在 android 设备中，小程序里的 wxs 与 js 运行效率无差异，而在 ios 设备中，小程序里的 wxs 会比 js 快 2~20倍。


### 小程序逻辑层

- App() 必须在 app.js 中注册，且不能注册多个。
- 不要在定义于 App() 内的函数中调用 getApp() ，使用 this 就可以拿到 app 实例。
- 不要在 onLaunch 的时候调用 getCurrentPages()，此时 page 还没有生成。
- 通过 getApp() 获取实例之后，不要私自调用生命周期函数。


#### 注册页面

> Page() 函数用来注册一个页面。接受一个 object 参数，其指定页面的初始数据、生命周期函数、事件处理函数等

- object 内容在页面加载时会进行一次深拷贝，需考虑数据大小对页面加载的开销

#### 生命周期

```js
app ( {
    // 小程序生命周期的各个阶段
    onLaunch: function(){},//当小程序初始化完成时，会触发 onLaunch（全局只触发一次）
    onShow: function(){},//当小程序启动，或从后台进入前台显示，会触发 onShow
    onHide: function(){},//当小程序从前台进入后台隐藏，会触发 onHide
    onError: function(){},//当小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息
    // 自定义函数或者属性，用 this可以访问
  ...
})

page({
    ...
    // 页面生命周期的各个阶段
    onLoad: function () {}, //生命周期函数--监听页面加载
    onShow: function () {}, //生命周期函数--监听页面初次渲染完成
    onReady: function () {}, //生命周期函数--监听页面显示
    onHide: function () {}, //生命周期函数--监听页面隐藏
    onUnload: function () {}, //生命周期函数--监听页面卸载
})
```

#### 页面相关函数

`enablePullDownRefresh`:记得停下来: wx.stopPullDownRefresh
`onReachBottom`: 如果页面是scroll-view则无效
`setData`: 异步的

```js
this.setData({
    ...
}, callback)
```

> 直接修改 this.data 而不调用 this.setData 是无法改变页面的状态的，还会造成数据不一致。
> 单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据
> 请不要把 data 中任何一项的 value 设为 undefined ，否则这一项将不被设置并可能遗留一些潜在问题

```
Q：setData方法是有react那样的虚拟dom优化吗？
A：有做虚拟DOM的优化，但设置相同数据还是会触发新渲染的。
```
#### 路由

对于路由的触发方式以及页面生命周期函数如下：

|路由方式|触发时机|路由当前页|路由后页面|
|--|--|---|--|
|初始化|小程序打开的第一个页面||onLoad, onShow|
|打开新页面|调用 API wx.navigateTo |onHide|onLoad, onShow|
|页面重定向	|调用 API wx.redirectTo|onUnload|onLoad, onShow|
|页面返回|调用 API wx.navigateBack|onUnload|onShow|
|重启动|调用 API wx.reLaunch|onUnload|onLoad, onShow|
|Tab 切换|调用 API wx.switchTab |||

> [tab](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html)
- navigateTo, redirectTo 只能打开非 tabBar 页面。
- switchTab 只能打开 tabBar 页面。
- reLaunch 可以打开任意页面。
- 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
- 调用页面路由带的参数可以在目标页面的onLoad中获取。

### 小程序事件

事件分为冒泡事件和非冒泡事件

- 冒泡事件：当一个组件上的事件被触发后，该事件会向父节点传递。
- 非冒泡事件：当一个组件上的事件被触发后，该事件不会向父节点传递。

```html
<view id="outer" bindtap="handleTap1">
  outer view
  <view id="middle" catchtap="handleTap2">
    middle view
    <view id="inner" bindtap="handleTap3">
      inner view
    </view>
  </view>
</view>
```

> bind事件绑定不会阻止冒泡事件向上冒泡，catch事件绑定可以阻止冒泡事件向上冒泡

```html
<view id="outer" bind:touchstart="handleTap1" capture-bind:touchstart="handleTap2">
  outer view
  <view id="inner" bind:touchstart="handleTap3" capture-bind:touchstart="handleTap4">
    inner view
  </view>
</view>

<view id="outer" bind:touchstart="handleTap1" capture-catch:touchstart="handleTap2">
  outer view
  <view id="inner" bind:touchstart="handleTap3" capture-bind:touchstart="handleTap4">
    inner view
  </view>
</view>
```
> 可以采用capture-bind、capture-catch关键字，后者将中断捕获阶段和取消冒泡阶段

### 组件

> 一些样式，性能，细节

#### button

> button 的边框样式是用`after`实现的。

```css
button::after {
  border: none;
}
```
#### input

- placeholder样式：style/class
- `_searchEvent`

#### form

```html
<form bindsubmit="submitInfo" report-submit="{{true}}" > 
  ...
</form>
```

这个e.detail.fromId，就是formid，真机才会产生，模拟器中为`the formId is a mock one`

#### swiper

> 如果在 bindchange 的事件回调函数中使用 setData 改变 current 值，则有可能导致 setData 被不停地调用，因而通常情况下请在改变 current 值前检测 source 字段来判断是否是由于用户触摸引起

#### scroll-view

- 使用竖向滚动时，需要给<scroll-view/>一个固定高度，通过 WXSS 设置 height。
- 在滚动 scroll-view 时会阻止页面回弹，所以在 scroll-view 中滚动，是无法触发 onPullDownRefresh
- 若要使用下拉刷新，请使用页面的滚动，而不是 scroll-view ，这样也能通过点击顶部状态栏回到页面顶部

- 子项不设置`display:inline-block`会出现滚动条

#### image

> image组件默认宽度300px、高度225px

#### 自定义组件

```
问: 自定义组件外部view没有正确插入到slot节点内中
答: 目前开发者工具不会展示 slot 里面的内容和 slot 节点本身，请以页面表现为准。我们之后会改进。
```
### API

#### 跳转(redirect, relaunch...)

> 偶尔的wx.redirectTo 和 wx.reLaunch 失效

貌似是跳的太快，真机没反应过来，这个只要加一个setTimeout就可以

```js
setTimeout(function() {
  wx.redirectTo({
    url: ''
  })
}, 200)
```

#### wx.canIUse

> 判断小程序的API，回调，参数，组件等是否在当前版本可用

#### wx.getExtConfig

> wx.getExtConfig 暂时无法通过 wx.canIUse 判断是否兼容，开发者需要自行判断 wx.getExtConfig 是否存在来兼容

```
问: 请问小程序在未绑定第三方平台的时候是否可以使用API “wx.getExtConfig”获取ext.json中的内容？

答: 未绑定的话无法获取的 
```

#### storage

> 本地数据存储的大小限制为 10MB

#### scanCode

> 会隐藏小程序，会调用hide方法

- path:当所扫的码为当前小程序的合法二维码时，会返回此字段，内容为二维码携带的 path

#### wx.requestPayment

- package 参数形式: 

> 支付完成，成功页，也会离开小程序，调取hide方法

#### wx.showToast

字数限制7个
不支持gif图

### 参考

- https://developers.weixin.qq.com/miniprogram/dev/
- http://www.ifanr.com/minapp/786415
- https://www.cnblogs.com/lhyforfront/p/7428684.html

