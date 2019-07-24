---
title: centos 搭建nfs 运行环境
date: 2019-07-24 09:33:10
tags: [nfs,centos]
categories: 运维
---

## centos 服务端搭建基本的nfs 挂载环境

* 安装基本的运行环境

```shell
yum install nfs-utils
systemctl enable rpcbind
systemctl start rpcbind
systemctl enable nfs
systemctl start nfs
```

* 防火墙配置

```shell
firewall-cmd --zone=public --permanent --add-service=rpc-bind
firewall-cmd --zone=public --permanent --add-service=mountd
firewall-cmd --zone=public --permanent --add-service=nfs
firewall-cmd --reload
```

* 配置共享目录

```shell
cat  /etc/exports

/data/     192.168.0.0/24(rw,sync,no_root_squash,no_all_squash)

systemctl restart nfs
showmount -e localhost # 查看挂载点
```

## centos 客户端

* 单次挂载

```shell
yum install nfs-utils
systemctl enable rpcbind
systemctl start rpcbind
showmount -e 192.168.0.101 #检查远程挂载点
mkdir /data #创建远程挂载目录
mount -t nfs 192.168.0.101:/data /data
```

* 自动挂载配置

```shell
cat /etc/fstab

192.168.0.101:/data      /data                   nfs     defaults        0 0

systemctl daemon-reload # 重启服务
```
