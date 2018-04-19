### 微信点餐

> 用于微信点餐，前端模块文档说明

#### 1 环境配置

由于项目基于php, 服务基于apache，所以要对一些所需的环境进行配置，下边以mac osx系统为主。

##### php环境

在mac环境下，osx系统自带php环境，

```sh
#查看php版本
php -v

#php配置目录(一般在/etc/下)
cd /etc/
sudo cp php.ini.default php.ini # 如果没有php.ini文件的情况下，生成一个
sudo vim php.ini
```

> 保存php.ini的时候，会有权限验证，用适合的编辑器，进行修改

`php.ini`修改内容

```sh
session.save_path = "/tmp" # 开放配置，大约在1356 行
extension=redis.so # 添加此扩展，大约在900行左右有扩展配置块区域，在其中添加
```

> windows 下可自行安装php环境

##### apache 配置

很庆幸，osx系统也自带了apache服务，我们只需要对其进行配置即可

运行`apachectl -v` 查看版本信息

```sh
cd /etc/
vim /et/apache2/httpd.conf

#修改内容(以下内容打开)
LoadModule userdir_module libexec/apache2/mod_userdir.so
LoadModule rewrite_module libexec/apache2/mod_rewrite.so
LoadModule php7_module libexec/apache2/libphp7.so

#开启虚拟主机配置
Include /private/etc/apache2/extra/httpd-vhosts.conf

DocumentRoot "/Users/oubunfei/Sites" #项目地址
<Directory "/Users/oubunfei/Sites">
    #
    # Possible values for the Options directive are "None", "All",
    # or any combination of:
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    #
    # Note that "MultiViews" must be named *explicitly* --- "Options All"
    # doesn't give it to you.
    #
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.4/mod/core.html#options
    # for more information.
    #
    Options FollowSymLinks Multiviews Indexes 
    MultiviewsMatch Any

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   AllowOverride FileInfo AuthConfig Limit
    #
    AllowOverride All

    #
    # Controls who can get stuff from this server.
    #
    Require all granted
</Directory>
```

进入`httpd-vhosts.conf`修改

```sh
#增加一个VirtualHost
<VirtualHost *:80>
    ServerAdmin wxdc #访问地址
    DocumentRoot "/Users/oubunfei/Sites/wxdc/api/public" #项目地址
    ServerName wxdc #访问地址:可以是localhost, 127.0.0.1,或者是自己的项目名称
    ErrorLog "/private/var/log/apache2/error_log"
    CustomLog "/private/var/log/apache2/dummy-host2.example.com-access_log" com$
</VirtualHost>
```

##### phpredis扩展

[下载地址](https://nodeload.github.com/nicolasff/phpredis/zip/master)

```sh
# 1.移动到/usr/local
sudo cp phpredis-master /usr/local/
# 2.cd /usr/local
tar -zxvf phpredis-master.zip
# 3.修改文件夹名称
sudo mv phpredis-master phpredis
# 4.cd phpredis
sudo phpize
# 5.如报错，未安装autoconf
brew install autoconf # cannot find autoconf
# 6.安装完成再执行，phpize
./configure --with-php-config=/usr/bin/php-config
# 7.执行make
sudo make
sudo make install
# 8.在php.ini配置中加入扩展
extension=redis.so
# 9.测试
php -m | grep redis
```

> 在操作中，会遇到文件权限的限制，需要修改mac中的bin权限

```sh
#开机时，进入使用工具设置界面
command + r

#在命令行设置disable
```

##### 配置中问题

1. 确定项目地址对否
2. 对文件的权限


#### 2 项目目录

```js
└ public
	└ wxdc
		├ img    //素材图片
		├ css    //所有样式文件
		├ fonts  //字体图标
		└ js     //所有页面逻辑
			├ bscroll.js  //滑动插件
			├ checkData.js    //检测基础数据异常
			├ constants.js    //常量、数据记录
			├ cooksOrNormsDialog.js   //多规格多做法点餐封装
			├ food-page.js    //菜品详细页逻辑
			├ getLocation.js  //获取经纬度，校验点餐范围
			├ goods-page.js   //点餐页逻辑
			├ jsDebug.js      //监听js报错log记录
			├ main.js     //require加载文件入口
			├ marquee.js  //调起微信、支付宝扫码封装
			├ menu.js     //导航窗口├ mui.js      //ui框架
			├ mui.poppicker.js    //poppicker插件
			├ mutations.js    //购物车缓存、购物车渲染逻辑
			├ order-list-page.js  //订单列表
			├ order-show-page.js  //订单详情页
			├ order-page.js   //订单核对页
			├ pay-page.js     //支付页
			├ payok-page.js   //支付成功页
			├ phone.js        //登录注册
			├ pre-order-page.js   //封装微信、支付宝扫一扫
			├ public.js       //共用方法封装
			├ renderFoods.js  //渲染菜品封装
			├ renderKinids.js //渲染菜类封装
			├ require.js  //require
			├ router.js   //模拟路由入口
			├ search-page.js  //搜索菜品页
			├ setmealsDialog.js   //套餐点餐封装
			├ successOrder.js     //后付下单成功页
			├ welife-recharge-page.js     //微生活充值页
			├ wpicker.js  //用餐人数
			├ wsocko.dc.js    //wsocket封装
			└ zepto.min.js    //zepto
	└ views
		└ WechatOrder
			├ index.php //首页
			├ debug-page.php    //调试窗口（后端打印log）
			├ food-page.php //菜品详细页
			├ goods-page.php    //点餐页
			├ invoice-page.php  //开发票页
			├ loading.php   //loading层
			├ meiTuanNotify.php //美团支付回跳模板页
			├ member-dialog.php //会员支付输入验证码、密码窗口
			├ memo-picker.php   //就餐人数窗口
			├ menu.php      //导航层
			├ norder-page.php   //无订单状态
			├ opinion-page.php  //评价页
			├ order-page.php    //订单核对页
			├ order_detail.php  //订单详情页
			├ order_list.php    //订单列表页
			├ pay-page.php  //支付页
			├ payok-page.php    //支付成功页
			├ payok-new-page.php    //支付成功页（新页面）
			├ peoples.php   //选择就餐人数页
			├ phone.php     //登录注册
			├ pre-order-page.php    //调起微信、支付宝扫二维码点餐页
			├ search-page.php   //搜索菜品页
			├ shopcart.php  //购物车模板
			└ shops-page.php    //搜索门店页
```

#### 3 页面路由

1. 前端通过hash形式模拟路由
2. 路由入口文件router.js，根据不同页面加载对应js文件

	-  / 默认首页
	-  /index 首页
	-  /no_order 无订单
	-  /food菜品详情
	-  /preOrder 预订单
	-  /order 订单核对
	-  /success_order 下单成功
	-  /pay 支付
	-  /wlife_recharge 充值
	-  /phone 登录注册
	-  /payok 支付成功
	-  /invoice 开发票
	-  /opinion 评价
	-  /search 搜索菜品

#### 4 文档参考

- https://blog.csdn.net/wodecc_u/article/details/76714064
