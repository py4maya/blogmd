---
title: 使用tcpreplay系列命令做流量转发
date: 2017-09-28 18:20:35
tags: [tcpreplay,tcpdump,centos]
categories: 系统
---


## 注，仅供参考，不要使用在生产环境中

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

使用tcpprep提前解析数据:
```shell
$ tcpprep -i test.cap -a client -o test.cache
```

使用tcprewrite改写数据:
```shell
$ tcprewrite --endpoints=10.210.128.63:10.210.3.100  -i test.cap -c test.cache -o rewrite.cap 
```

使用tcpreplay重放数据:
```shell
$ tcpreplay -c test.cache -i lo -I eth0 to3.100.cap 
```


