---
title: centos7下使用firewalld基本指南
date: 2018-04-12 10:08:24
tags: [firewalld]
categories: [系统]
---

* 安装firewalld ，设置为开机启动，启动之:
```shell
$ yum install firewalld
$ systemctl enable firewalld
$ systemctl start firewalld
```

* 查看zones 
```shell
$ firewall-cmd --get-zones
```

* 创建新的zone :
```shell
$ firewall-cmd --new-zone=goaway --permanent
```

* 添加简单的服务，端口:
```shell
$ firewall-cmd --zone=goaway --add-port=80/tcp --permanent
$ firewall-cmd --zone=goaway --add-port=53/udp --permanent
$ firewall-cmd --zone=goaway --add-service=ssh --permanent
```

* 删除简单的服务，端口:
```shell
$ firewall-cmd --zone=goaway --remove-port=80/tcp --permanent
$ firewall-cmd --zone=goaway --remove-port=53/udp --permanent
$ firewall-cmd --zone=goaway --remove-service=ssh --permanent
```

* 设置新的默认zone
```shell
$ firewall-cmd --set-default-zone=goaway
```

* 修改zones文件以后，重新加载:
```shell
$ firewall-cmd --reload
```

* 复杂规则的添加删除:
```shell
$ firewall-cmd --permanent --remove-rich-rule="rule family="ipv4" source address="192.168.142.166" port protocol="tcp" port="11300" accept"
$ firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.142.166" port protocol="tcp" port="11300" accept"
```
