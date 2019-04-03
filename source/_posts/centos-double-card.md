---
title: centos 双网卡配置有一个网卡不通 
date: 2019-03-22 12:32:47
tags: [centos,rp_filter]
categories: 运维
---


根本原因:

Linux默认启用了反向路由检查

如果2个网卡在一个Lan里面,那么服务器可能从eth0或者eth1发现网关, 如果一个包从eth0进入了, 而网关在eth1上, 那么从eth1是出不去的, 就不通了.  反向路由检查要求从哪里来的才能回哪去.

关闭反向路由检查(根据自己的情况替换第二第三行的网卡名):

```shell
echo 0 > /proc/sys/net/ipv4/conf/all/rp_filter
echo 0 > /proc/sys/net/ipv4/conf/eth0/rp_filter
echo 0 > /proc/sys/net/ipv4/conf/eth1/rp_filter
```

每次开机自动关闭反向路由检查, 加入 /etc/rc.local 即可.


