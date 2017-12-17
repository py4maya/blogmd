---
title: graylog 安装docker中
date: 2017-11-22 11:09:12
tags: [graylog,docker]
categories: 系统
---

* 依赖于 docker-compose 

* 创建储存graylog的目录:

```shell
mkdir -p ./graylog/data/journal
mkdir -p ./graylog/config
```

* 编写docker-compose.yml
```shell

version: '2'
services:
  mongo:
    image: "index.alauda.cn/library/mongo:3"
  elasticsearch:
    image: "index.alauda.cn/library/elasticsearch:2.3.2"
    command: "elasticsearch -Des.cluster.name='graylog'"
  elasticsearch1:
    image: "index.alauda.cn/library/elasticsearch:2.3.2"
    command: "elasticsearch -Des.cluster.name='graylog'"
  graylog:
    image: graylog2/server:2.1.0-1
    environment:
      GRAYLOG_PASSWORD_SECRET: xsofdnasdfsgolwa
      #echo -ne "koudai@a1@38t" | sha256sum
      GRAYLOG_ROOT_PASSWORD_SHA2: 8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918 
      GRAYLOG_REST_TRANSPORT_URI: http://172.16.7.58:12900
    depends_on:
      - mongo
      - elasticsearch
      - elasticsearch1
    ports:
      - "172.16.7.58:9002:9000"
      - "172.16.7.58:12900:12900"
      - "172.16.7.58:8514:8514/udp"
    volumes:
      - "./graylog/data/journal:/usr/share/graylog/data/journal"
      #- "./graylog/config:/usr/share/graylog/data/config"

```

* 使用docker-compose启动服务:

```shell
docker-compose up -d
```

* 注:在配置文件中修改graylog的默认用户名密码:

```shell

$ echo -ne "admin" | sha256sum

```

* 从容器中copy出 graylog.conf 配置文件 到 ./graylog/config/graylog.conf  ，然后修改,重新启动
```shell
docker cp graylog_graylog_1:/usr/share/graylog/data/config graylog/
```

