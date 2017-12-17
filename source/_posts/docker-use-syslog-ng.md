---
title: docker 容器中使用syslog-ng
date: 2017-11-23 19:06:31
tags: [syslog-ng]
categories: 系统
---

容器中配置的是ubuntu的基础系统.
安装syslog-ng

```shell
apt-get install syslog-ng
```

然后 启动 syslog-ng 

```shell
service syslog-ng start 
```

* 报错1
syslog-ng: Error setting capabilities

按照如下的代码修改启动参数
```shell
cat /etc/default/syslog-ng

##添加这一行,注释打开
SYSLOGNG_OPTS="--no-caps"

```

* 报错2 

Error opening file for reading; filename='/proc/kmsg', error='Operation not permitted (1)'

去掉syslog-ng配置中的system()

```shell

source s_src {
#       system();
       internal();
};

```

