---
title: 使用lua查看http(s)中的请求内容
date: 2017-10-13 14:05:54
tags: [nginx,lua,http,LuaRocks]
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
--add-module=../ngx_devel_kit
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

* 添加必要的lua模块，请使用如下脚本安装LuaRocks
```shell
wget http://luarocks.org/releases/luarocks-2.0.13.tar.gz
tar -xzvf luarocks-2.0.13.tar.gz
cd luarocks-2.0.13/
./configure --prefix=/usr/local/openresty/luajit \
    --with-lua=/usr/local/openresty/luajit/ \
    --lua-suffix=jit \
    --with-lua-include=/usr/local/openresty/luajit/include/luajit-2.1
make
sudo make install
```

* **谨记，谨记** 需要把luarocks 对应的PATH 添加到环境变量中:

```shell
luarocks path 
```

* nginx 配置文件加入require的路径标识:

```shell
lua_package_path '/data1/nginx/lua/?.lua;';
lua_package_cpath '/usr/local/lib/lua/5.1/?.so;';

###代码中可以使用tools对象,及ngx.shared.server （table对象）
init_by_lua 'tools = require "tools"';
lua_shared_dict server 5m;

```


** 注意一点 ** 

配置更新的地址:
/data0/luajit/share/lua/5.1/luarocks/cfg.lua:198 去掉不通的地址.

