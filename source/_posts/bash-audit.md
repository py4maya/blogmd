---
title: shell 命令审计 
date: 2019-04-12 08:52:41
tags: [shell,audit]
categories: 审计
---

### 提前看:

* .bash版本至少3.00或以上
* 接受审计的登录用户默认shell必须为bash

配置方法，使用history ，添加固定的 PROMPT_COMMAND 命令格式, HISTTIMEFORMAT 设置history 的格式。
(如果其他的shell 也支持类似的变量，也可以用类似的方法解决)

### 创建所需文件，及权限限制修改:

```shell
mkdir /var/log/shell_audit
touch /var/log/shell_audit/audit.log
chown nobody:nobody /var/log/shell_audit/audit.log
chmod 002 /var/log/shell_audit/audit.log
chattr +a /var/log/shell_audit/audit.log
```

### 提前设置history 的相关设置:
```shell
# 添加如下内容到 bashrc ,保证用户登录及生效
HISTSIZE=2048
HISTTIMEFORMAT="%Y/%m/%d %T   ";export HISTTIMEFORMAT
export HISTORY_FILE=/var/log/shell_audit/audit.log
export PROMPT_COMMAND='{ code=$?;thisHistID=`history 1|awk "{print \\$1}"`;lastCommand=`history 1| awk "{\\$1=\"\" ;print}"`;user=`id -un`;whoStr=(`who -u am i`);realUser=${whoStr[0]};logDay=${whoStr[2]};logTime=${whoStr[3]};pid=${whoStr[5]};ip=${whoStr[6]};if [ ${thisHistID}x != ${lastHistID}x ];then echo -E `date "+%Y/%m/%d %H:%M:%S"` $user\($realUser\)@$ip[PID:$pid][LOGIN:$logDay $logTime] --- [$PWD]$lastCommand [$code];lastHistID=$thisHistID;fi; } >> $HISTORY_FILE'
```

### 添加相应的logrotate 防止日志过大:

```shell
cat /etc/logrotate.d/shell_audit
/var/log/shell_audit/audit.log {
    weekly
    missingok
    dateext
    rotate 100
    sharedscripts
    prerotate
    /usr/bin/chattr -a /var/log/shell_audit/audit.log
    endscript
    sharedscripts
    postrotate
      /bin/touch /var/log/shell_audit/audit.log
      /bin/chmod 002 /var/log/shell_audit/audit.log
      /bin/chown nobody:nobody /var/log/shell_audit/audit.log
      /usr/bin/chattr +a /var/log/shell_audit/audit.log
    endscript
}

```
### 还可以配合rsyslog 传输到 集中的命令日志服务器,添加实时审计监控:

```shell

ruleset(name="rule_rsyslog") {
    action(type="omfwd"
        Protocol="udp"
        Target="10.211.101.93"
        Port="521")
    stop
}
input(
    type="imfile"
    File="/var/log/shell_audit/audit.log"
    Facility="user"
    Severity="notice"
    Tag=""
    PersistStateInterval="1"
    Ruleset="rule_rsyslog"
)

```

