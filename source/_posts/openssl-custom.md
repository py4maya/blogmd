---
title: openssl 生成自定义证书
date: 2019-03-15 13:03:28
tags: [openssl]
categories: 开发
---
```shell
openssl genrsa -out server.key 1024
openssl req -new -x509 -days 3650 -key server.key -out server.crt -subj "/C=CN/ST=mykey/L=mykey/O=mykey/OU=mykey/CN=domain1/CN=domain2/CN=domain3"
```
