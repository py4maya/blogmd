---
title: 使用tcpreplay系列命令做流量转发
date: 2017-09-28 18:20:35
tags: [tcpreplay,tcpdump,centos]
categories: 系统
---

centos 安装:
```shell
$ yum install epel-release 
$ yum install tcpreplay
```

使用tcpdump 抓取数据,及查看数据包命令:
```shell
$ tcpdump -vv  -i eth1 host 192.168.1.123 -w test.cap
$ tcpdump -r test.cap
```


