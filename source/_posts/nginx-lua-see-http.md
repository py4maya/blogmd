---
title: 使用lua查看http(s)中的请求内容
date: 2017-10-13 14:05:54
tags: [nginx,lua,http]
categories: [系统]
---

原由:查看一个proxy系统客户端的请求内容，该系统为nginx proxy 搭建。

方案:
* 部署新的nginx 
* 内容请求部分添加lua 拦截请求
* 根据nginx lua api 分离http(s) 的请求内容

编译nginx时需要添加lua支持:

```shell

###注，忒别重要
export LUAJIT_LIB=/data1/luajit/lib
export LUAJIT_INC=/data1/luajit/include/luajit-2.0

./configure --prefix=/data0/nginx/ \
--with-stream \
--with-http_ssl_module \
--with-stream_ssl_module \
--with-ld-opt=-Wl,-rpath,/data0/luajit/lib \
--add-module=../lua-nginx-module
```

配置正常的nginx模块,添加lua解析:
```shell
server {

	listen 80;
	location / {
		 access_by_lua_file /data0/nginx/conf/vhost/access.lua;
		 proxy_pass http://$http_host;	
	}
}

```

lua的内容:
```lua

ngx.req.read_body()

local ip = ngx.var.remote_addr
local logfile = io.open("/tmp/data.log","a");

local method = ngx.req.get_method()
local body = ngx.req.get_body_data()

if (body == nil)
then
    body = ''
end

logfile:write("\n ============================  " .. method .. " ========================== " .. "\n")
logfile:write(ngx.req.raw_header() .. "\n")
logfile:write(body ..  "\n")


logfile:flush()
logfile:close()

```

