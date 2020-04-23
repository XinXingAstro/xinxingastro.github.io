---
title: Hadoop集群搭建即HDFS基本操作
date: 2017-08
categories: 大数据
tags: 
  - Hadoop集群
  - HDFS
comments: true
updated:
---
HDFS就是一个基于分布式、自动冗余、可以动态扩展的大硬盘，Hadoop目前不支持Java9最好装Java7或者Java8。

<!-- more -->
虚拟机系统：centos7

#### JDK安装
`rpm -ivh ./jdk….`
`rpm -qa | grep jdk`
#### Hadoop安装
`tar –xvf ./hadoop-2.7.2.tar.gz`
`tar -zxvf filename.tar -C /specific dir`
#### 配置环境变量
`export JAVA_HOME=/usr/java/default`
`export $PATH:/usr/local/hadoop/bin:/usr/local/hadoop/sbin`
#### 网络配置
`hostnamectl set-hostname NAME`
`ifconfig`
`/etc/sysconfig/network-scripts/ifcfg-enp0s3`
`service network restart`
`/etc/hosts`
`systemctl stop firewalld`
`systemctl disable firewalld`

VirtualBox桥接模式虚拟机连外网配置:

步骤|
-|
1.将各个虚拟机的网络连接方式设置成桥接，映射的网卡应为电脑上连网的那个网卡|
2.修改/etc/sysconfig/network-scripts/ifcfg-enp0s3中如下配置：|
  BOOTPROTO=static|
ip地址获取方式为静态ip（只改这个地方不够，重启后ip会变|
  IPADDR=192.168.3.100|
将ip地址设置为和主机同网段ip|
  NETMASK=255.255.255.0|
设置子网掩码|
  GATEWAY=192.168.3.1|
设置网关（路由器地址）|
3.在/etc/sysconfig/network中设置DNS1和DNS2|
DNS1=192.168.3.1    路由器地址|
DNS2=8.8.8.8|
谷歌默认DNS地址|
4.service network restart|
重启网络服务|

