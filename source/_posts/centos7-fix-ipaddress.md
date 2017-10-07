---
title: centos7固定ip
date: 2017-10-05 16:55:14
tags: [centos7]
categories: 系统
---

配置文件 /etc/sysconfig/network-scripts/ifcfg-xxxxx :
```shell

BOOTPROTO="static" #dhcp改为static   
ONBOOT="yes" #开机启用本配置  
IPADDR=192.168.7.106 #静态IP  
GATEWAY=192.168.7.1 #默认网关  
NETMASK=255.255.255.0 #子网掩码  
DNS1=192.168.7.1 #DNS 配置  


##修改为:
BOOTPROTO=static
IPADDR=192.168.122.216
GATEWAY=192.168.122.1
NETMASK=255.255.255.0
DNS1=192.168.122.1 
DNS2=192.168.122.2


```
