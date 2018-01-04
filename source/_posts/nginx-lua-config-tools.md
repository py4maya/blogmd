---
title: 使用lua 扩展nginx 的一个简单模板
date: 2017-12-20 14:51:51
tags: [lua,nginx]
categories: 系统
---

nginx 配置文件先加入require的路径标识:

```shell
lua_package_path '/data1/nginx/lua/?.lua;';
lua_package_cpath '/usr/local/lib/lua/5.1/?.so;';

###代码中可以使用tools对象,及ngx.shared.server （table对象）
init_by_lua 'tools = require "tools"';
lua_shared_dict server 5m;

```

vhost 部分配置:
```shell

server {
    listen 10.13.8.75:80;
	server_name webide.sina.com.cn;
	default_type  text/html;

	location /login {
		content_by_lua "tools.content_by_lua('login')";
		header_filter_by_lua "tools.header_filter_by_lua('login')";
	}

	location / {
		proxy_pass http://127.0.0.1:9000;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		rewrite_by_lua "tools.rewrite_by_lua()";
	}
}

```

对应的lua 代码集中的地方:
```lua
local M = {}

function M.set_by_lua(name) 
	if ( name == 'proxy_addr' )
	then
		return "http://127.0.0.1:9000"
	elseif (name == "another_name")
	then
		return "bye bye "
	end
end


function M.access_by_lua() 
	--- return	ngx.exit(ngx.HTTP_FORBIDDEN) 
	return 
end


function M.rewrite_by_lua()
	--- ngx.exec("/login")
end


function M.content_by_lua(name)
	ngx.say("hello login page page from lua : " .. name)
end


function M.header_filter_by_lua(name)
	
end

function M.body_filter_by_lua(name)
	
end
return M
```


#### 根据不同的server_name 做不同的proxy_pass

* nginx 做泛域名绑定:
```shell
server_name *.webide.sina.com.cn;
```

* lua 获取需要转发的地址:
```lua
function M.set_pass_addr()
    local server_name = ngx.var.host 
    local first_pt = string.find(server_name,'%.') - 1 
    local first_segment = string.sub(server_name,0,first_pt)
    local _type = string.sub(first_segment,0,1)
    local _port = string.sub(first_segment,2,10)
    return "http://127.0.0.1:" .. _port
end
```

* nginx 根据返回的变量做proxy_pass
```shell
set_by_lua $p_pass_addr  "return tools.set_pass_addr()";
proxy_pass $p_pass_addr;
```

* 开发阶段建议把 **lua_cache** 打开:
```shell
lua_code_cache 'off';
```
