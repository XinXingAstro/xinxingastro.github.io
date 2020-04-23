---
title: 关于MacOS环境变量和自定义命令
tags:
  - MacOS
comments: true
date: 2018-03-17 00:57:47
categories: Others
---
**OSX系统环境变量加载顺序:**

> **/etc/profile** -> **/etc/paths** -> **~/.bash_porfile** -> **~/.bash_login** -> **~/.profile** -> **~/.bashrc**

<!-- more -->

#### OS X系统环境变量加载顺序

```shell
/etc/profile

/etc/paths

~/.bash_porfile

~/.bash_login

~/.profile

~/.bashrc
```


/etc/profile 和 /etc/paths 系统启动时自动加载。

~/.bash_profile，~/.bash_login，~/.profile按照从前往后的顺序读取，如果~/.bash_profile文件存在，则后面的几个文件就会被忽略不读了，如果~/.bash_profile文件不存在，才会以此类推读取后面的文件。

mac上面重启终端后 ~/.bashrc 不会自动加载。

#### 配置环境变量

`echo $PATH` 查看当前环境变量

`export VALUE_HOME = /.../.../...` 定义新环境变量是一个目录地址

`export PATH=$VALUE_HOME/bin:$PATH` 将/bin目录加入环境变量的头部

`source .bashrc` 执行修改过的配置文件，使环境变量生效

#### 自定义命令

##### 方法一：利用alias在配置文件中自定义命令

`alias ll="ls -la"` 

`alias nb="cd /Users/xinxing/myProjects/Blog;hexo new $1"`

##### 方法二：将存放自定义脚本的目录加入PATH变量中

`export PATH=$PATH:/Users/xinxing/.my_cmd`