# `<meta>`

> 页面不知道写过多少，到头来发现对一个标签都捉摸不透, 既然碰到了，就总结回顾一下，基础很重要...

## 简介
> The <meta> tag provides metadata about the HTML document. Metadata will not be displayed on the page, but will be machine parsable.

`meta`作为html语言head区的一个辅助性标签，大家有时候都觉得可有可无。其实能够用好meta标签，会给我们带来意想不到的效果。

> `meta` ,常用于定义页面说明，关键字，最后修改日期等，这些元数据将服务于浏览器，布局，重载，搜索引擎和其他网络服务。

`meta` 有两个属性:

- name
- http-equiv

> 不同的属性又有不同的参数值，从而实现了不同的网页功能.

## name

> `name`属性主要用于描述网页，比如网页的关键词，叙述等。与之对应的属性为content, content重的内容是对name填入类型的具体描述，便于搜索引擎抓取。

```html
<meta name="参数" content="具体的描述" >
```

`name`的参数:

- `keywords`(关键字): 用于告诉搜索引擎，你网页的关键字

```html
<meta name="keywords" content="nodejs, es6, react, ..." >
```
<br />

- `description`(网站内容的描述): 用于告诉搜索引擎，你网站的主要内容。

```html
<meta name="description" content="总结前端知识框架..." >
```
<br />

- `viewport`(移动端的窗口): 设计移动端网页
	+ `width`: 控制viewport大小，可以指定一个值：600/device-width(设备的宽度)。
	+ `height`: 和width相对应
	+ `initial-scal`: 初始缩放比例，即当前页面第一次load的时候缩放比例。
	+ `maximum-scal`: 允许用户缩放到的最大比例。
	+ `minimum-scal`: 允许用户缩放到的最小比例。
	+ `user-scaleable`: 用户是否可以收到缩放

> 不同的设备可能会有差异，仔细查一些文档

```html
<meta name="viewport" content="width=device-width, initial-scale=1" >
```
<br />

- `robots`(定义搜索引擎爬虫的索引方式): 告诉爬虫哪些页面需要索引，哪些页面不需要索引。
	+ `none`: 搜索引擎将忽略次网页，等价于noindex, nofollow.
	+ `noindex`: 搜索引擎不索引此网页.
	+ `nofollow`: 搜索引擎不继续通过此网页的链接索引搜索其他的网页.
	+ `all`: 搜索引擎索引此网页.
	+ `follow`: 搜索引擎继续通过此网页的链接索引搜索其它的网页.

> `content`的参数: `all`, `none`, `index`, `noindex`, `follow`, `nofollow`. 默认是all

```html
<meta name="robots" content="none">
```
<br />

- `author`(作者): 标注网页作者

```html
<meta name="author" content="geyulong, gaiyulong@gmail.com">
```
<br />

- `generator`(网页制作软件): 表明网页是什么软件做的

```html
<meta name="generator" content="vim">
```
<br />

- `copyright`(版权): 标注版权信息, 代表网站为**所有

```html
<meta name="copyright" content="geyulong"> 
```
<br />

- `revisit-after`(搜索引擎爬虫重访时间): 如果页面不是经常更新，为了减轻搜索引擎爬虫对服务器带来的压力，可以设置一个爬虫的重访时间，如果重访时间过短，爬虫将它们定义的默认时间来访问。

```html
<meta name="revisit-after" content="8 days" >
```
<br />

- `renderer`(双核浏览器渲染方式): 为双核浏览器准备的，用于指定双核浏览器以何种方式渲染页面，比如360浏览器.

```html
<meta name="renderer" content="webkit"> 
<!--默认webkit -->

<meta name="renderer" content="ie-comp"> 
<!--默认IE兼容模式 -->

<meta name="renderer" content="ie-stand"> 
<!--默认IE标注模式 -->
```
<br />

## http-equiv属性

> `http-equiv`相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，与之对应的属性值为content.

```html
<meta http-equiv="参数" content="参数变量值">
```

`http-equiv`属性主要有以下几种参数:

- `content-Type`(设定网页字符集): 设定网页的字符集，便于浏览器解析与渲染

```html
<meta http-equiv="content-Type" content="text/html;charset-utf-8">
<!-- 旧的HTML,不推荐 -->

<meta charset="utf-8"> 
<!-- HTML5 设定网页字符集的方式，推荐使用UTF-8 -->
```
<br />

- `X-UA-Compatible` (浏览器采取何种版本渲染当前页面): 用于告知浏览器以何种版本来渲染页面(一般都设置为最新模式，在各大框架中这个设置也很常见。)

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge, chrome=1" />
<!--指定IE和Chrome使用最新版本渲染当前页面 -->
```

<br />

- `cache-control` (指定请求和响应遵循的缓存机制): 指导浏览器如何缓存某个响应以及缓存多长时间。 
	+ no-cache: 先发送请求，与服务器确认该资源是否更改，如果未被更改，则使用缓存。
	+ no-store: 不允许缓存，每次都要去服务器上，下在完整的响应。(安全措施)。
	+ public: 缓存所有响应，但并非必须。因为max-age也可以做到相同效果。
	+ private: 职位单个用户缓存，因此不允许任何中继进行缓存。(比如说cdn就不允许缓存private的响应)
	+ max-age: 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求，例如: max-age=60表示响应可以再缓存和重用60秒
	+ no-siteapp: 禁止当前页面在移动端浏览时，被百度自动转码。虽然百度的本意是好的，但是转码效果很多时候却不尽人意，所以可以在head中加入此属性，可以避免.

```html
<meta http-equiv="catch-control" content="no-cache">

<meta http-equiv="catch-control" content="no-siteapp">
```
<br />

- `expires`(网页到期时间): 设定网页到期时间，过期后网页必须到服务器上重新传输。

```html
<meta http-equiv="expires" content="Sunday 30 October 2017 10:59 GMT" />
```
<br />

- `refresh`(自动刷新并指向某页面): 网页将在设定的时间内，自动刷新并调向设定的网址。

```html
<meta http-equiv="refresh" content="2: URL=https://yulongge.git.io">
<!-- 2秒后跳转到我指定的网址 -->
```
<br />

- `Set-Cookie`(cookie设定): 如果网页过期，那么这个网页存在本地的cookie会自动删除。

```html
<meta http-equiv="Set-Cookie" content="name, date">
<!--格式 -->

<meta http-equiv="Set-Cookie" content="User=geyulong;path=/;expires=Sunday, 10-Jan-17 10:00:00 GMT">
<!--例子 -->
```

> 可能还有别的值，上边都是常见的。










