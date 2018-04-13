---
title: 安装nginx lua 控制stream 
date: 2018-02-06 15:48:53
tags: [nginx,lua]
categories: 开发
---

编译前提:

* luajit 
* ngx_devel_kit 
* lua-nginx-module 控制http 的lua 模块
* stream-lua-nginx-module  git clone https://github.com/openresty/stream-lua-nginx-module


编译参数:

```shell
#!/bin/bash

export LUAJIT_LIB=/data0/luajit/lib
export LUAJIT_INC=/data0/luajit/include/luajit-2.0/


./configure \
--prefix=/data0/nginx/ \
--with-stream \
--with-http_ssl_module  \
--with-stream_ssl_module \
--with-ld-opt="-Wl,-rpath,/data0/luajit/lib " \
--add-module="../ngx_devel_kit-0.3.0" \
--add-module="../lua-nginx-module" \
--add-module="../stream-lua-nginx-module" \

make -j4
make install
```
