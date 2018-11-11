### 建站

说到建站，我们从事web学习的骚年，一般都会迫不及待想去做的一件事。但是实际操作的时候，一头雾水，导致梦想迟迟无法实现。

对于任何我们想要做的事情，不要光想，速度，果断一下，就成了.


#### 服务器选择

一般选择云计算服务器(ECS)还是私有服务器(VPS)?

对我们个人而言，还是前者比较合适.

国内的的一些云平台:

- 阿里云
- 腾讯云
- 百度开发云平台
- 新浪云
- 亚马逊AWS
- 华为云
- 青云
- 盛大云计算

简单介绍一下优缺点:

- 腾讯云: 适合做游戏，庞大的用户群体，
- 亚马逊: 云服务的鼻祖，但是它不太适合国内市场的应用以及速度
- 新浪: 速度和稳定性略有问题
- 阿里云: 国内最成熟的云服务器
- 百度云: 出了很对针对个人的开发



阿里的ECS


[个人入门推荐](https://www.aliyun.com/easybuy?spm=5176.ecssimplebuyv2.405614.1.731a3675WuSK7V)

![yun](https://yulongge.github.io/images/vampire_server/aliyun.png)

#### 区域选择

服务器的区域选择，购买服务器时经常有很多区域可以选择，这点其实是要涉及你的用户群体及你的所在地区，服务器机房跟你的用户群体及你所在的区域越接近效果会越佳.

![area](https://yulongge.github.io/images/vampire_server/area.png)

#### 系统选择

![system](https://yulongge.github.io/images/vampire_server/system.png)

首选建议绝对是linux系统的centos，linux系统在内存及性能的占用上是最优的，最节约资源占用，而且存在较大的学习空间。但是前期如果真的觉得自己无法马上把握好linux的使用建议还是可以选择windows的，毕竟比较好架设环境。

centos建议是不要选择过于新的版本，新版本针对对应的运行环境也必须是新的，但是有的项目的搭建环境并不符合那么高版本的需求，会导致出错，所以建议选择centos6或者5的较为佳

> 选择的镜像：centos7

### 连接服务器

用ssh命令连接

输入 SSH 命令连接：ssh root@实例的(弹性)公网 IP

```shell
 ssh root@xxxxxxxx
 >>> password
```

当然你也可以用别的工具: `putty`

如果选择的window系统，可以用远程桌面的工具进行连接:如:  Microsoft Remote Desktop

#### 域名

[阿里域名购买](https://wanwang.aliyun.com/domain/searchresult/?keyword=geyulong&suffix=.com#/?keyword=geyulong&suffix=com)

![yuming](https://yulongge.github.io/images/vampire_server/ym.png)

现在的域名丰富多样，不同的域名后缀，有不同的含义

域名主要分为国际域名, 国内域名, 国外域名.

- `.com`: Commercial organizations, 商业组织，公司
- `.net`: Network operations and service centers, 网络服务商
- `.top`: 顶级，高端，适用于任何商业，公司，个人
- `.tech`: 科技, 技术
- `.org`: Other organizations, 非盈利组织
- `.gov`: Governmental entities, 政府部门
- `.edu`: Educational institutions, 教育机构
- `.link`: Internet king 互联网之王
- `.red`: 吉祥,红色, 热情
- `.int`: International organizations 国际组织
- `.mil`: Military(U.S)美国军部
- `.pub`: public 大众, 公众, 知名
- `.info`: 信息网与信息服务
- `.name`: 一般个人注册和使用
- `.mobi`: 全球唯一专为手机及移动终端设备打造的域
- ...

- `.cn`: 国内域名
- `.公司`: 中文中国公司和商业组织域名
- `.网络`: 中文中国网络服务机构域名
- `中文.com`
- ...

> geyulong.tech


#### 域名解析

域名解析是把域名指向网站空间IP，让人们通过注册的域名可以方便地访问到网站的一种服务

![jiexi](https://yulongge.github.io/images/vampire_server/jiexi.png)

![jilu](https://yulongge.github.io/images/vampire_server/jilu.png)

[域名解析文档](https://help.aliyun.com/document_detail/291065259.1996646101.searchclickresult.4c02735beCiDhH)

##### ssl证书

https安全超文本传输协议，即http下加入ssl层，https的安全基础是ssl

所以我们要用https协议，就需要去签发ssl证书

![ssl](https://yulongge.github.io/images/vampire_server/ssl.png)

DV通配符证书: 

通配符 SSL 又叫泛域名证书，英文名称为：Wildcard Certificates。可以保护一个域名以及该域名所有的二级或者三级子域名，不限制子域名数量，且添加新的子域名无须重新审核和另外付费，节约了大量的时间和金钱成本。例如，一个单独的通配符证书就可以保护 www.example.com、blog.example.com 和 store.example.com。通配符证书可以保护通用域名和您在提交申请时指定的级别下的所有子域。只需在通用域名左侧的子域区域添加星号 (*) 即可

单域名免费证书

> 单域名证书

![zh](https://yulongge.github.io/images/vampire_server/zs.png)

> 我们下载下来，等搭建https服务时用

> 云盾证书提供的证书文件后缀是.pem，如果是系统创建的CSR，同时还会伴随证书私钥，文件后缀.key。只有是系统创建的CSR时，证书才支持不同格式的转换。可根据自己的实际需求修改扩展名，比如可将.pem修改成.crt等。

![keys](https://yulongge.github.io/images/vampire_server/ky.png)

#### 备案

域名如果没有实名认证，先实名认证，然后开始备案, 备案是一个比较耗时和麻烦的过程...

> 现在一些平台绑定合法域名时，要求有ICP备案，比如小程序

- 去ICP官网直接备案
- 在域名服务商提供的入口备案

![beian](https://yulongge.github.io/images/vampire_server/beian.png)



#### 部署环境

> 基于centos

- nodejs
- mysql
- git

##### node

wget node包地址

[node版本资源](https://nodejs.org/dist/)  

```sh
wget https://nodejs.org/dist/v8.9.1/node-v.xz
tar xvf node-v8.9.1-linux-x64.tar.xz
node -v
npm -v
```

##### mysql

> 注意mysql的版本，因为服务器配置问题，所以高版本的mysql可能不兼容

```sh
wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
rpm -ivh mysql57-community-release-el7-9.noarch.rpm
yum install mysql-server


server mysql start
server mysql stop
server mysql restart
```

> 安装mysql方式很多，如有问题可以随便google，比如lump

##### git

```sh
yum -y install git
```

[参考文档](https://www.cnblogs.com/liter7/p/6581344.html)

### 搭建项目


- 运行一个node服务
- 创建对应的数据库


##### node 项目

> 用express搭建一个服务端

```js
const createError = require('http-errors');
const express = require('express');
const path = require('path');
const logger = require('morgan');
const ejs = require('ejs');
const walk = require('klaw-sync');
const config = require('./config.js');
const db = require('./utils/db');


var app = express();
app.engine('html', ejs.renderFile);
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'html');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(express.static(path.join(__dirname, 'public')));

//连接数据库
const connection = db.connection();

//遍历路由接口
const routes = walk(config.router_path)
		.map(p=>p.path)
		.filter(path=>/\.js$/.test(path))
        .forEach(part=>require(part)(app, config.router_prefix, connection));

//监听小程序less文件
require('./parseless');

app.use(function(req, res, next) {
	//next(createError(404));
})

module.exports = app;
```

启动一个https服务

```js
var app = require('../app');
var debug = require('debug')('Vampire:server');
var http = require('http');
var https = require('https');
var fs = require('fs');

//console.log(process.env, 'process')
var port = normalizePort(process.env.PORT || '443');
app.set('port', port);

var options = {
    key:fs.readFileSync('./keys/ca.key'),
    cert:fs.readFileSync('./keys/ca.crt')
}
var server = https.createServer(options, app);
```

如果你没有正式的ssl证书，可以用`openssl`生成


> 公钥私钥的生成可用openssl（linux，mac自带，windows上需要自己安装）工具来生成

```sh
//生成CA私钥
openssl genrsa -out ca.key 1024
//生成csr文件
openssl req -new -key ca.key -out ca.csr
//生成自签名证书
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
//生成server.csr文件
openssl req -new -key server.key -out server.csr
//生成带有ca签名的证书
openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt
```

[参考文档](https://blog.csdn.net/f826760951/article/details/67639309)

##### 创建数据库

```sh
mysql -uroot -p

>>> password
```

用我们编写的sql语句，创建所需要的数据库，表等...

```sql
create database vampire;
use vampire;

-- 全局配置表
create table if not exists config (
	id INT NOT NULL AUTO_INCREMENT,
	title varchar(100) NOT NULL,
	primary key (id)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into config (title)
	values ("vampire");

-- user表
create table if not exists user (
	id INT NOT NULL AUTO_INCREMENT,
	user_name varchar(100) NOT NULL,
	pic varchar(100),
	sex varchar(40),
	age INT(10),
	openId varchar(100),
	iphone varchar(20),
	email varchar(20),
	role INT(10),
	login_time DATE,
	primary key (id)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 插入user数据
insert into user (user_name, pic, sex, age, openId, iphone, email, role, login_time)
	values ('GYL', '', '男', 30, '234353453454', '17701389735', 'gaiyulong@gmail.com', 1, '2018-07-27');
```

### 访问服务器

在浏览器地址栏中输入我们的域名访问

> 启动后，https访问不行，是因为还没有添加安全规则

添加安全组规则
配置安全端口，访问公网地址加端口号

![sf](https://yulongge.github.io/images/vampire_server/srule.png)
![adds](https://yulongge.github.io/images/vampire_server/adds.png)

配置我们访问白名单，就可以自由访问了

至此，建站完毕!

### 参考

- http://www.runoob.com/mysql/mysql-install.html
- https://help.aliyun.com/document_detail/291065259.1996646101.searchclickresult.4c02735beCiDhH
- https://www.cnblogs.com/liter7/p/6581344.html
- https://blog.csdn.net/f826760951/article/details/67639309






