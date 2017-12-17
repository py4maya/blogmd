---
title: 一些常用的PostgreSQL-sql
date: 2017-12-04 14:44:07
tags: [PostgreSQL]
categories: 系统
---

常见的几个sql:

* 创建表：

```shell
CREATE TABLE excmdb (
    asset_number character varying(128),
    ips text[],
    product character varying(128),
    administrator character varying(64),
    rack character varying(128),
	manifest character varying(32),
	constraint pk_tbl_primary_asset_manifest primary key (asset_number,manifest)
);
```

* 导出表结构:
```shell
$ pg_dump -h localhost -U cmdb -s cmdb
$ pg_dump -h localhost -U cmdb cmdb -t excmdb   #导出数据
```

* 执行sql文件:
```shell
\i data.sql;
```

* 查看描述:
```
\dt #查看表list
\d excmdb #查看表结构
```

* 数组操作:
```shell
select sample where ips @> '{127.0.0.1}' # 查看数组包含
update sample set ips = ips || '{127.0.0.1}' # 添加元素
UPDATE excmdb SET ips = array_remove(ips,'10.13.2.144') WHERE asset_number = 'D10075981'; #删除元素
select sample where ips ||  '{127.0.0.1}' # 数组元素的in查询
```

