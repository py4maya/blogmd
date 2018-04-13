---
title: centos7源码安装mysql 5.6
date: 2018-03-14 09:54:44
tags: [mysql,centos7]
categories: [系统]
---

* 下载源代码，去官网找吧
* 安装cmake

```shell
$ yum install cmake -y 
```

* 生成Makefile :
```shell
MYSQL_PREFIX=/data0/mysql

cmake \
-DDOWNLOAD_BOOST=1 \
-DCMAKE_INSTALL_PREFIX=$MYSQL_PREFIX \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS=all \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ZLIB=bundled \
-DWITH_READLINE=1 \
-DWITH_DEBUG=OFF \
-DENABLED_LOCAL_INFILE=1 \
-DENABLED_PROFILING=1 \
-DMYSQL_MAINTAINER_MODE=0 \
-DMYSQL_TCP_PORT=3306 \
-DMYSQL_DATADIR=$MYSQL_PREFIX/data \
-DMYSQL_UNIX_ADDR=$MYSQL_PREFIX/data/mysql.sock 

```

* 初始化数据库:
```shell
$ ./scripts/mysql_install_db --basedir=/data0/mysql/ --datadir=/data0/mysql/data
$ ru
$  chown mysql:mysql -R data/
```

* 启动数据库吧:
```shell
$ 
```
