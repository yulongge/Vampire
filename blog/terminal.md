mac terminal command

#### ls

> 列出文件: ls 参数 目录名

```sh
# 显示中文
ls -w

# 详细信息
ls -l

#包括隐藏文件
ls -a
```

#### cd

> 转换目录: cd 目录名

```sh
#转换目录
cd 目录
```

#### mkdir

> 创建新目录 mkdir 目录名

```sh
#创建目录
mkdir dirname
```

#### rmdir

> 删除一个目录

```sh
rmdir dirname
```

#### mvdir 

> 移动或重命名一个目录

```sh
mvdir dir1 dir1
```

#### dircmp

> 比较两个目录的内容(osx不支持好像)

```sh
dircmp dir1 dir2
```

#### cp

> 拷贝文件：cp 参数 源文件 目标文件

```sh
#拷贝(-R 表示对目录进行递归操作)
cp -R 源文件 目标文件
```

#### rm

> 删除文件: rm 参数 文件

```sh
# 删除文件(-rf 表示递归和强制，如果使用了，系统里就没这个文件)
rm -rf 文件
```

#### mv

> 移动文件: mv 文件

```sh
#移动文件
mv 源目录/文件 目标目录/文件
```

#### chmod

> 更改文件权限: chmod 参数 权限 文件

```sh
# 更改目录权限(-R 表示递归操作)

chmod -R 755 目录
```

#### chown

> 更改文件属性: chown 参数 用户:组 文件

```sh
#把驱动目录下的所有文件属性改成根用户 
chown -R root:wheel /System/Library/Extensions
```

#### diskutil

> 修复系统中文件的权限(严格说这个不是一个unix命令，而是osx一个软件，记得修改或添加的驱动就执行一次)

#### nano

> 文件编辑: nano 文件名

```sh
#编辑文件
nano demo.md

#编辑完后用ctrl + o存盘，ctrl + x 退出

# 同vi
```

#### sh

> 运行脚本命令: sh 脚本文件名

```sh
#clean.sh

rm -rf /System/Library/Extensions.kextcache
rm -rf /System/Library/Extensions.mkext
chown -R root:wheel /System/Library/Extensions
chmod -R 755 /System/Library/Extensions
diskutil repairpermissions /
kextcache -k /System/Library/Extensions/

# sh /clean
```

#### man

> 查看命令的详细帮助

```sh
# 查看ls命令详情

man ls
```

#### pwd

> 显示当前目录的路径名

#### cat

> 显示或链接文件

#### pg

> 分页格式化显示文件内容(pg osx不支持)

#### more

> 分屏显示文件内容

#### od 

> 显示非文本文件的内容， 二进制显示

```sh

od filename 
```

#### find

> 使用匹配表达式查找文件: find . -name "*.c" -print 

#### file

> 显示文件类型

```sh
file filename
```

#### head

> 显示文件的最初几行

```sh
#显示前二十行信息
head -20 filename
```

#### tail

> 显示文件的最后几行信息

```sh
# 显示文件后20行信息
tail -20 filename
```
#### cut

#### colrm

#### paste

#### diff

> 比较并显示两个文件的差异

```sh
diff file1 file2
```

#### sed

#### grep

> 在文件中按模式查找

```sh
grep "[a-zA-Z]" filename
```

#### awk

#### sort

#### uniq

#### comm

#### wc

> 统计文件的字符数，词数和行数

#### nl

> 给文件加上行号

```sh
nl file1 > file2
```

#### passwd

> 修改用户密码

#### umask

> 定义创建文件的权限掩码

#### chgrp

> 改变文件或目录的所属组

#### clock

> 给终端上锁

```sh
xclock -remote
```

#### make

> 维护可执行程序的最新版本 

#### ps

> 显示进程当前状态

```sh
ps u
```

#### kill

> 终止进程

```sh
kill -9 30142
```

#### nice

#### renice

#### date

> 显示系统的当前日期和时间

#### cal

> 显示日历

#### time

> 统计程序的执行时间

#### telnet

> 远程登录

```sh
telnet hpc.sp.net.edu.cn
```

#### rlogin

#### rsh

#### ftp

#### rcp

> 在本地主机与远程主机 之间复制文件

```sh
rcp file1 host1:file2
```

#### ping

> 给一个网络主机发送 回应请求 

#### mall

#### write

#### mesg

#### history

#### r

#### alias

#### unalias

#### uname

> -a

#### clear

#### env

#### who

#### whoami

#### tty

#### stty

#### du

#### df 

#### w

### 参考文档

- https://www.douban.com/note/75797151/
