---
title: Mac安装配置JDK
tags:
  - null
  - null
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2017-08-12 10:40:25
categories: 其他
---

Mac OS下安装配置JDK。

<!-- more -->

---

### 下载安装JDK

首先在Oracle官网下载Mac OS版本的JDK安装包安装，安装完成后可以通过`java -version`命令查看JDK版本号。

### 配置JAVA_HOME, CLASS_PATH环境变量

Mac OS X 10.5以后的版本中Mac系统提供了Java框架支持，在系统中安装多个版本的JDK时，系统会自动检查最新的JDK然后使用其中的JAVA_HOME。如果想删除某个版本的JDK直接删除/Library/Java/JavaVirtualMachines中的某个jdk文件夹，系统会自动检测当前剩下的jdk，然后让/usr/libexec/java_home连接到当前最新版本的jdk中的home文件夹。

安装多个版本的JDK时可以使用下面指令列出所有JAVA_HOME

```shell
/usr/libexec/java_home -V
```

用户的JAVA_HOME环境变量可以随意指定，也可以跟系统一样使用最新JDK版本的JAVA_HOEM，推荐如下设置：

```shell
export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$JAVA_HOME/bin:$PATH
export CLASS_PATH=$JAVA_HOME/lib
```

第二行是将jdk中的bin目录加到PATH环境变量中，由于jdk在安装时已经在/usr/bin下新建了alias所以可以不写。

JDK就配置完成了。