---
title: mysql-slave-config
date: 2019-04-22 18:06:50
tags: [mysql]
categories: 运维
---


### 主库mysql 配置: 

 
 ```
[mysqld]
max_connections = 2048
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
server_id = 4
relay-log-index=slave-relay-bin.index
relay-log=slave-relay-bin 
#slave-skip-errors=all

log_bin = "/data0/mysql/logs/bin.log"
expire_logs_days = 30
max_binlog_size = 512m
binlog_cache_size       = 4m
max_binlog_cache_size   = 512m
```
### 授权语句: 
GRANT REPLICATION SLAVE ON *.* TO 'rep'@'192.168.%'  IDENTIFIED BY '123456';


#### 从库配置： 
```

[mysqld]
max_connections = 2048
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
server_id = 3
relay-log-index=slave-relay-bin.index
relay-log=slave-relay-bin 

```

#### 操作从库语句:
CHANGE MASTER TO
           ->     MASTER_HOST='master_host_name',
            ->     MASTER_USER='replication_user_name',
            ->     MASTER_PASSWORD='replication_password',
            ->     MASTER_LOG_FILE='recorded_log_file_name',
            ->     MASTER_LOG_POS=recorded_log_position;




#### 主从切换之前，可以先把主库，单方面写锁定:

```shell
FLUSH TABLES WITH READ LOCK
UNLOCK TABLES;
```

### 锁定单个库:
```shell
# LOCK TABLES tbl_name [AS alias] {READ [LOCAL] | [LOW_PRIORITY] WRITE}
lock  tables test read;  ## 只锁定一个表
```
