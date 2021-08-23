Linux系统用户及组管理

tail命令

使用tail可以在不打开文件的情况下输出文件部分信息

比如查看/etc/passwd最后三行的信息则可以使用如下命令

```
tail -3 /etc/passwd
```

同样的，如果要查看前三行的信息则使用+号，如下

```
tail +3 /etc/passwd
```

相关文件

/etc/passwd 存储用户的基本属性信息，并且对所有用户可读

/etc/group 用户信息的存放地，组名不能重复

/etc/shadow 用户对应的密码信息，只有root拥有读权限

常用命令

```
useradd -m -u "uid" -g "初始组" -G “附加组” -s “登录的shell” 用户
```

-m创建用户主目录 

/etc/passwd 字段解释

```
root:x:0:0:root:/root:/bin/bash
```

用户名：密码占位符：UID:GID:用户描述：用户主目录：登录之后使用的shell

查看系统有哪些shell的命令

```
cat /etc/shells
```

创建用户时指定用户家目录使用 

``` 
useradd -d “家目录路径” 用户
```

这里使用-d 而不是-m，因为-m的意思是创建主目录，并不能指定路径，所以创建用户的时候使用-d比使用-m要好

使用大写的-M参数则是不创建用户主目录



