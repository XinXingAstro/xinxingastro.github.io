---
title: 在Win10上安装配置MySQL
date: 2017-03-21 11:51:47
tags:
  - MySQL
categories: Others
comments: true
---
由于网上很多在Win10上的MySQL安装教程不对或者对新版MySQL不适用，所以我总结一下我的安装过程
<!-- more -->
#### 下载压缩文件并解压
在官网下载最新的[MySQL Community Server](https://dev.mysql.com/downloads/mysql/)这个版本是免费的（我下载的是mysql-5.7.17-winx64.zip）直接解压到你想要安装的目录，在MySQL的根目录下新建一个`my.ini`文件，将以下代码复制进去**注意要将路径改为自己的安装路径**：  
>[mysql]
>default-character-set=utf8
>[mysqld]
>port = 3306
>basedir=yourMySQLInstallPath
>datadir=yourMySQLInstallPath\data
>max_connections=200
>character-set-server=utf8
>default-storage-engine=INNODB

#### 新建data目录
在MySQL根目录下新建data目录，如果没有这个目录在后面启动服务时会出现  `NET HELPMSG 3534` 错误；

#### 安装MySQL服务并初始化
启动管理员命令行提示符（win + x  a ）， 进入MySQl根目录中的`bin`目录，执行以下命令：  
&emsp;`mysql -install                           //提示Service successfully installed`
&emsp;`mysql --initialize --user=root --console //最后会出现mysql的root用户默认登陆密码`

#### 启动mysql服务:   
&emsp;`net start mysql`

#### 登陆mysql：    
&emsp;`mysql -u root -p      //会提示Enter password 输入刚才出现的登陆密码就可以登陆`

#### 重设登陆密码
进入mysql环境后才可以重设密码： 
&emsp;`set password = password('yournewpassword');`

