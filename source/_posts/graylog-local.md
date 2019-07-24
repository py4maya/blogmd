---
title: 本地搭建graylog 环境简单注意 
date: 2019-04-29 16:07:12
tags: [graylog]
categories: 系统
---


* 下载文件:

找一个快速一点儿的机房下载:
wget https://packages.graylog2.org/releases/graylog/graylog-2.5.1.tgz

* 编辑修改graylogctl 文件在bin 目录下:

```shell
GRAYLOGCTL_DIR=${GRAYLOGCTL_DIR:=$(dirname "$GRAYLOGCTL")}
GRAYLOG_SERVER_JAR=${GRAYLOG_SERVER_JAR:=graylog.jar}
GRAYLOG_CONF=${GRAYLOG_CONF:=graylog.conf}
GRAYLOG_PID=${GRAYLOG_PID:=graylog.pid}
LOG_FILE=${LOG_FILE:=log/graylog-server.log}
```

* 重命名配置文件:
 graylog.example.conf
 修改几个地方:
 password_secret ： 为自定义的96个字符
 root_username 超级用户昵称
 root_password_sha2 超级用户密码

 ```shell
 echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
 ```

 rest_listen_uri ， web_listen_uri

* 日志rotate 配置:

    Configure Index Set ->  Index Rotation Configuration 一般配置以天为单位就好
* graylog.conf 中还可以单独配置使用的mongodb , elasticsearch 集群地址等


