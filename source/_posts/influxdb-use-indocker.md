---
title: docker中使用influxdb
date: 2017-11-20 09:29:04
tags: [influxdb,docker]
categories: 系统
---

* 拉取脚本:

```shell
$ docker pull daocloud.io/daocloud/influxdb:latest
```

* 基础运行脚本

```shell

lip=172.16.7.1 	##只暴露端口给内网的IP
name=influxdb
port=" -p $lip:8086:8086 -p $lip:2003:2003 -p $lip:8083:8083 "

docker run -it ${port} --name ${name}  daocloud.io/daocloud/influxdb

```



