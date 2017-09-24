---
title: 使用nginx做后端tcp,udp的proxy
date: 2017-09-23 07:51:16
tags: [nginx,stream,proxy]
categories: 系统
---

安装参数:
```shell

 ./configure \
 --prefix=/data1/mginx/ \
 --with-stream \
 --with-stream_realip_module \
 --with-stream_ssl_module \
 --without-http \

 make -j4
 make install
```

stream的配置文件，在nginx主配置文件中添加容器stream
```shell

''''
stream {
	upstream sample_stream {
		server 127.0.0.1:5800 max_fails=3 fail_timeout=5s;	
		server 127.0.0.1:5801 max_fails=3 fail_timeout=5s;	
	}

	server { 
		listen 1812 ; # 可以用指定ip 或者指定协议,比如 listen 10.211.61.88 1812 ; 比如  listen 10.211.61.88 1812 udp;	
		proxy_pass radius_upstream_onetime; #数据转发
		allow 10.210.12.17; #指定基于ip的访问控制
		deny all;
	}
}

http{
}
''''

```

stream 中可以添加lua控制，这样就可以方便的用lua编写tcp，udp 程序,在编译参数中添加stream_lua组件:
```shell
--with-ld-opt="-Wl,-rpath,/data1/luajit/lib" \
--add-module="../stream-lua-nginx-module" \
```

设置stream中生成lua控制:
```shell
content_by_lua_file /data1/code/lua/streams/echo.lua;
```

lua脚本控制，配置一个echo协议，即输入什么返回什么:
```lua
local sock = assert(ngx.req.socket(true))
local ip = ngx.var.remote_addr
local data 

while(1 == 1)
do
	data = sock:receive()  
	ngx.say(data)
end

```
