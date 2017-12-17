---
title: 使用rsyslog 转发文件日志
date: 2017-12-16 20:21:03
tags: [rsyslog,linux]
categories: 系统
---

```shell

module(load="imfile")

ruleset(name="graylog-remote") {
    action(type="omfwd"
        Protocol="udp"
        Target="10.211.101.93"
        Port="521")
    stop
}

input(
    type="imfile"
    File="/data0/nginx/logs/radius_ac.log"
    Facility="user"
    Severity="notice"
    Tag=""
    PersistStateInterval="1"
    Ruleset="graylog-remote"
)
```
