### 微信分享说明

> 请注意，不要有诱导分享等违规行为，对于诱导分享行为将永久回收公众号接口权限，详细规则请查看

#### 1.先绑定js安全域名

先登录微信公众平台进入“公众号设置”的“功能设置”里填写“JS接口安全域名

![js](https://yulongge.github.io/images/vampire/jssafe.png)

> 设置三个只能



#### 2.config, 通过config接口注入权限验证配置

> 所有需要使用JS-SDK的页面必须先注入配置信息，否则将无法调用

```js
wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '', // 必填，公众号的唯一标识
    timestamp: , // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名
    jsApiList: [] // 必填，需要使用的JS接口列表
});
```

> appid: 公众号后台可以获取， timestamp, nonceStr, signature需要后台获取，jsApiList需要注册我们所需要用到api接口

#### 3.分享

```js
wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '', // 必填，公众号的唯一标识
    timestamp: , // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名
    jsApiList: ["updateAppMessageShareData", "updateTimelineShareData"] // 必填，需要使用的JS接口列表
});
```


##### 1.自定义“分享给朋友”及“分享到QQ”按钮的分享内容

```js
wx.ready(function () {   //需在用户可能点击分享按钮前就先调用
    wx.updateAppMessageShareData({ 
        title: '', // 分享标题
        desc: '', // 分享描述
        link: '', // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
        imgUrl: '', // 分享图标
        success: function () {
          // 设置成功
        }
});
```

> 下边分享的域名必须在js安全域名下

##### 2.自定义“分享到朋友圈”及“分享到QQ空间”按钮的分享内容

```js
wx.ready(function () {      //需在用户可能点击分享按钮前就先调用
    wx.updateTimelineShareData({ 
        title: '', // 分享标题
        link: '', // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
        imgUrl: '', // 分享图标
        success: function () {
          // 设置成功
        }
});
```

> 原有的 wx.onMenuShareTimeline、wx.onMenuShareAppMessage、wx.onMenuShareQQ、wx.onMenuShareQZone 接口，即将废弃

#### 4.隐藏功能按钮

批量隐藏功能按钮接口

```js
wx.hideMenuItems({
    menuList: [""menuItem:share:appMessage", "menuItem:favorite" ...] 
});
```

隐藏所有非基础按钮接口

```js
wx.hideAllNonBaseMenuItem();
```

所有功能列表可以参考官网

##### 基本类

举报: "menuItem:exposeArticle"

调整字体: "menuItem:setFont"

日间模式: "menuItem:dayMode"

夜间模式: "menuItem:nightMode"

刷新: "menuItem:refresh"

查看公众号（已添加）: "menuItem:profile"

查看公众号（未添加）: "menuItem:addContact"

#### 传播类

发送给朋友: "menuItem:share:appMessage"

分享到朋友圈: "menuItem:share:timeline"

分享到QQ: "menuItem:share:qq"

分享到Weibo: "menuItem:share:weiboApp"

收藏: "menuItem:favorite"

分享到FB: "menuItem:share:facebook"

分享到 QQ 空间/menuItem:share:QZone

#### 保护类

编辑标签: "menuItem:editTag"

删除: "menuItem:delete"

复制链接: "menuItem:copyUrl"

原网页: "menuItem:originPage"

阅读模式: "menuItem:readMode"

在QQ浏览器中打开: "menuItem:openWithQQBrowser"

在Safari中打开: "menuItem:openWithSafari"

邮件: "menuItem:share:email"

一些特殊公众号: "menuItem:share:brand"

### 参考

https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115