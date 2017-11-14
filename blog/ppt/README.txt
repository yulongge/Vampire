@Author: geyulong
@Date:   2017-11-10T16:33:28+08:00
@Last modified by:   geyulong
@Last modified time: 2017-11-14T11:04:40+08:00



﻿[作用]
直接用markdown文件生成幻灯片

[格式要求]
在需要分页的地方添加3个横线 ---

[生成方法]
markdown-to-slides xxx.md -o xxx.html

[在生成的html中(14行)修改以下样式]
.remark-slide-content {position: relative;}
.remark-slide-content::after {position: absolute; right:-30px; bottom: -30px; z-index: 0;
	display: block; content: ' ';
	width: 300px; height: 300px;
	background: url('frontend.png') no-repeat 0 0;
	opacity: .3;
	transform: rotate(-30deg);
}
ul {line-height: 1.5;}
.remark-slide-content h1 { font-size: 2.5em; text-align: center; margin-top: 100px; white-space: nowrap; }
.remark-slide-content h2 { font-size: 2em; text-align: center; white-space: nowrap; }
table {
	border: 1px;
	border-color: #999;
}
img {
	width: 100%;
	height: 500px;
}
.hljs-default .hljs {
	display: block;
	padding: 0.5em;
	max-height: 500px;
	overflow: auto;
}

```js
	document.getElementsByTagName('table')[0].setAttribute("border", 1);
	document.getElementsByTagName('table')[0].setAttribute("cellspacing", 0);
	document.getElementsByTagName('table')[0].setAttribute("cellpadding", 0);
```
