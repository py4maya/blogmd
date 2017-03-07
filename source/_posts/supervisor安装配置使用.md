---
title: supervisor安装配置使用
date: 2017-03-07 08:57:50
tags: [python,pip,supervisor]
---

1. 安装supervisor
```shell
$ yum install python-setuptools
$ easy_install pip
$ pip install supervisor
```
这个工具分为两个命令:
* supervisord : supervisor的服务器端部分，启动supervisor就是运行这个命令
* supervisorctl：启动supervisor的命令行窗口。

2. 生成配置文件:
```shell
$ echo_supervisord_conf > /etc/supervisord.conf

```
3. 修改配置文件:
```shell
[program:mobsf]
;process_name = %(program_name)s-%(process_num)d  
;numprocs = 5
command = python /data0/Mobile-Security-Framework-MobSF/manage.py runserver 0.0.0.0:8000
user = root
autostart = true
autorestart = true
startsecs = 5
stderr_logfile = /data0/Mobile-Security-Framework-MobSF/logs/run/error.log
stdout_logfile = /data0/Mobile-Security-Framework-MobSF/logs/run/stdout.log
```
4. 启动命令:
```shell
$ supervisord -c /etc/supervisord.conf 
```
5. 查看状态:
```shell
$ supervisorctl status    
```
6. 修改supervisord.conf以后，重载:
```shell
$ supervisorctl reload 
```

