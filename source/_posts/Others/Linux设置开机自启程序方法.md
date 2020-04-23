---
title: Linux设置开机自启程序方法
tags:
comments: true
mathjax: false
date: 2018-07-13 01:18:30
categories: Others
---

本文讲解在Linux中设置开机自启动程序的方法。

<!-- more -->

---

### 方法1: 在`/etc/rc.local`中添加启动脚本

注意`/etc/rc.local`文件默认是没有执行权限的，修改完要给`rc.local`添加执行权限：

```
chmode +x /etc/rc.local
```

### 方法2: 使用chkconfig添加自启动服务

在`/etc/rc.d/init.d`中创建你的执行脚本，加入叫`myscript`，在这个脚本开头添加下面三行语句：

```
#! /bin/bash  //指定执行该脚本的程序
# chkconfig: 2345 10 90  //设置启动优先级
# description:  //对该服务的描述
```
保存后对该脚本文件添加执行权限，然后在chkconfig中加入该服务。
```
chmod +x myscript
chkconfig --add myscript
```

查看`myscript`服务的启动情况

```
chkconfig --list myscript
```

启动`myscript`服务

```
chkconfig myscript on
```

`chkconfig`命令的更详细内容可以参考：[Linux下chkconfig命令详解](https://www.cnblogs.com/panjun-Donet/archive/2010/08/10/1796873.html)

### 方法3: 使用`systemctl`添加系统服务

参考：[在CentOS 7上利用systemctl添加自定义系统服务](https://blog.csdn.net/yuanguozhengjust/article/details/38019923)