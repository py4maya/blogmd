---
title: rsync log 添加服务选项 
date: 2019-04-22 16:40:42
tags: [rsync]
categories: 开发
---


```shell
log file = /var/log/rsyncd.log
transfer logging = yes 
log format = %t %a %m %f %b
syslog facility = local3
timeout = 300 

[cold]
hosts allow = 1.1.1.1
read only = no
path = /data1/cold
uid = cold
gid = cold
```
