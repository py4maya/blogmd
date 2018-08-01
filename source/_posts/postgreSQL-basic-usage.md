---
title: postgreSQL基本使用指南
date: 2018-07-06 15:49:01
tags: [postgreSQL]
categories:
---

#### 简单介绍:


PostgreSQL的官方网站是：[https://www.postgresql.org/](https://www.postgresql.org/)

简单工具:

* psql 
  cli 客户端

* pg_ctl
  服务控制工具

* initdb
  数据库初始化工具

#### 初始化:

```shell
useradd -g postgres postgres 
mkdir -p /data0/pgsql/data
chown -R postgres:postgres /data0/pgsql/data
./initdb -E UNICODE -D /mnt/pgsql/data
cat postgresql.conf | grep -n listen_addresses  
```

##### pg_ctl 的一些帮助:

```shell
  pg_ctl init[db] [-D DATADIR] [-s] [-o OPTIONS]
  pg_ctl start    [-D DATADIR] [-l FILENAME] [-W] [-t SECS] [-s]
                  [-o OPTIONS] [-p PATH] [-c]
  pg_ctl stop     [-D DATADIR] [-m SHUTDOWN-MODE] [-W] [-t SECS] [-s]
  pg_ctl restart  [-D DATADIR] [-m SHUTDOWN-MODE] [-W] [-t SECS] [-s]
                  [-o OPTIONS] [-c]
  pg_ctl reload   [-D DATADIR] [-s]
  pg_ctl status   [-D DATADIR]
  pg_ctl promote  [-D DATADIR] [-W] [-t SECS] [-s]
  pg_ctl kill     SIGNALNAME PID
 ```

#### 简单的工具脚本:

* README 中的一些工具脚本:
```shell
./configure
make
su
make install
adduser postgres
mkdir /usr/local/pgsql/data
chown postgres /usr/local/pgsql/data
su - postgres
/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
/usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >logfile 2>&1 &
```

* restart.sh
```shell
su postgres -c "/opt/postgreql/bin/pg_ctl restart -l /opt/postgreql/run/logs/postgreql.log -D /opt/postgreql/run/data "
```

* stop.sh 关闭服务:
```shell
su postgres -c "/opt/postgreql/bin/pg_ctl stop -l /opt/postgreql/run/logs/postgreql.log -D /opt/postgreql/run/data "
```

* 连接主机脚本:
```shell
psql -U cmdb -h localhost -d cmdb ### 不适用任何配置信息，在本机执行pgsql 则使用超级管理员连接数据库
```

* 基本sql:
```shell


CREATE ROLE name;
CREATE USER name;
grantall on schema csm_ca to public;

\du;

GRANT UPDATE ON demo TO demo_role; --赋予demo_role demo表的update权限
GRANT SELECT ON ALL TABLES IN SCHEMA PUBLIC to demo_role; --赋予demo_role所有表的SELECT权限

CREATE DATABASE database_name;
drop database testdb;
CREATE TABLE table_name(  
   column1 datatype,  
   column2 datatype,  
   column3 datatype,  
   .....  
   columnN datatype,  
   PRIMARY KEY( one or more columns )  
);
```

* 基于主机授权:
pg_hba.conf
```shell
host    all             all             127.0.0.1/32            trust
host    all             all             10.210.12.18/32            trust
host    all             all             10.210.12.17/32            trust
host    all             all             ::1/128                 trust
host    replication     all             127.0.0.1/32            trust
host    replication     all             ::1/128                 trust
```
