---
title: centos7 修改dns
date: 2017-12-08 15:55:13
tags: [dns,centos]
categories: 系统
---

* 修改系统配置 /etc/resolv.conf:

```shell
nameserver 10.210.12.10
nameserver 172.16.105.248
```

* 重启系统网络:

```shell
systemctl restart NetworkManager.service
```
