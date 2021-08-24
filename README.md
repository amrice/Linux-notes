# Linux-notes

这是我个人学习Linux过程中做的一些记录。

[TOC]

## 第六章：Linux系统用户及组管理

### 一、用户和组的配置文件

#### tail命令

使用tail或者head命令可以在不打开文件的情况下输出文件部分信息

比如查看/etc/passwd最后三行的信息则可以使用如下命令

```shell
[root@localhost /]# tail -3 /etc/passwd
tcpdump:x:72:72::/:/sbin/nologin
amrice:x:1000:1000:Eyte:/home/amrice:/bin/bash
hx:x:1001:1001::/home/hx:/bin/bash
```

同样的，如果要查看前三行的信息则使用+号，如下

```shell
tail +3 /etc/passwd
```

### head命令

head命令默认显示文件的前十行，-n选项后接数字,可以指定显示文件的行数。

-n选项后面接的数字可以是负数，表示显示文件前面的内容，但是不包括文件最后的n行。例如-n -100表示除了最后的100行内容不显示外，其他内容都显示出来：

```

```



#### 相关文件位置

/etc/passwd 存储用户的基本属性信息，并且对所有用户可读

/etc/group 用户信息的存放地，组名不能重复

/etc/shadow 用户对应的密码信息，只有root拥有读权限

#### 常用命令

```
useradd -m -u "uid" -g "初始组" -G “附加组” -s “登录的shell” 用户
```

"-m"创建用户主目录 

/etc/passwd 字段解释

```
root:x:0:0:root:/root:/bin/bash
```

用户名：密码占位符：UID:GID:用户描述：用户主目录：登录之后使用的shell

#### 查看系统有哪些shell的命令

```
cat /etc/shells
```

#### 创建用户时指定用户家目录命令

``` 
useradd -d “家目录路径” 用户
```

这里使用”-d“ 而不是"-m"，因为"-m"的意思是创建主目录，并不能指定路径，所以创建用户的时候使用"-d"比使用"-m"要好

使用大写的"-M"参数则是不创建用户主目录

#### 使用id命令查看用户及所在组信息

```
id ”用户“
```

#### 查看命令的绝对路径

``` 
which "命令"
```

#### 删除用户

```
userdel -r "要删除的用户"
```

使用这条命令加上参数”-r“则会在执行的时候会同时删除用户的家目录和/var/mail下的目录

### 二、shadow文件讲解

```shell
hx:$6$9N.x557DyMC,,,,,2Yxb2Az0.8y1:18858:0:99999:7:::
```

用户:密码（rehl5用MD5/RHEL6用sha512加密）密码最近更改日期:密码不能更改的天数（距离最近一次更改密码的时间不能小于这个值，0则表示随便修改）:密码过期时间：密码到期钱7天警告：宽限天数（密码过期了多少天后还能更改密码）:账号过期时间:保留字段

#### 密码部分

```
$6$9N.x557DyMCuQxsE$YJr9CzFaUtF/mtLwbaMAgplLf5cV5utmLLg6Rw3Fiw2LLfm2xwdCrXaZkYozPs6pKfQq6EVCpF2Yxb2Az0.8y1
```

密码分为三个部分，第一部分表示使用哪种哈希算法，第二部分用于加密哈希的salt，第三部分是已经加密的哈希

$1 表示MD5

$6 表示SHA-512

$5 表示SHA-256

#### 通过man命令查看文档帮助

```shell
man 5 shadow
man 5 shadow
man 5 group
man 3 crypt
```

#### 给用户添加密码

```
passwd “用户”
```

交互式的命令则是

```
echo “密码” | passwd --stding "用户"
```

#### 通过egrep命令过滤文件内容并输出

```
egrep -v "^$ | ^#" /etc/shadow
```

上面这条命令表示以￥开头或者以#结尾的过滤掉，输出不是以“$”开头或者不是#结尾的信息

去掉“-v”就表示输出以$开头和#结尾的信息

中间的”|“类似英文的”or”以及中文“或”的意思，只需呀满足其中一个条件即可

#### centos7进入单用户模式修改root密码

如果忘记root密码可以进入紧急模式修改密码

在启动等待页面方向键上下选择启动文件，然后按“e”键进入编辑模式

将：

```
ro
```

改成：

```
rw init=/sysroot/bin/sh
```

然后Ctrl+X启动，接着切根

```
:/# chroot /sysroot   #切根
:/# passwd            #修改root密码
:/# *********         #输入密码
:/# touch /.autorelabel 创建.autoelabel文件使配置生效  （课程里没讲到，实践时发现改了密码之后未生效，查找之后发现少了这个步骤）
:/# exit              #退出
:/# reboot            #重启
```

然后正常启动即可

## 第七章：文件系统的基本权限和高级权限管理

### 文件的基本权限

