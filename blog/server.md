### 部署项目

#### 购买服务器

阿里的ECS

[个人入门推荐](https://www.aliyun.com/easybuy?spm=5176.ecssimplebuyv2.405614.1.731a3675WuSK7V)

#### 配置服务器

1. 选择的镜像：centos7
1. 远程连接
2. 登录密码

windows: Putty

远程连接 mac os为例

用ssh命令连接

如果本地设备使用 Linux 或 Mac OS X 系统，按以下步骤远程连接实例。

1. 输入 SSH 命令连接：ssh root@实例的(弹性)公网 IP。
2. 输入实例登录密码。

#### 部署nodejs(CentOS)

wget node包地址

https://nodejs.org/dist/ 这里有各个版本资源选择

```
wget https://nodejs.org/dist/v8.9.1/node-v8.9.1-linux-x64.tar.xz
tar xvf node-v8.9.1-linux-x64.tar.xz
```

### nvm管理版本

还用用n吧

### 随便写个node脚本，启动服务

添加安全组规则
配置安全端口，访问公网地址加端口号

### 域名解析

https://help.aliyun.com/document_detail/29716.html?spm=5176.11065259.1996646101.searchclickresult.4c02735beCiDhH

ssl证书

### git
https://www.cnblogs.com/liter7/p/6581344.html
用yum

```
yum –y install git
```

### express https证书

https://blog.csdn.net/f826760951/article/details/67639309

云盾证书提供的证书文件后缀是.pem，如果是系统创建的CSR，同时还会伴随证书私钥，文件后缀.key。只有是系统创建的CSR时，证书才支持不同格式的转换。可根据自己的实际需求修改扩展名，比如可将.pem修改成.crt等。

下载已有的证书

启动后，https访问不行，是因为还没有添加安全规则




