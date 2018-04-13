---
title: 比nginx自己更好用的ip 访问控制
date: 2018-01-10 17:46:19
tags: [lua,nginx]
categories: 系统
---

nginx 自己的ip访问控制大家应该都用过，就是下边的这种写法:

```shell
...
allow ip1;
allow ip2;
allow ip3;
deny all;
...
```

当你有一堆IP需要控制的时候，就是一件比较难受的事情了。
所以，使用lua 写了这么一段lua代码。

nginx 自己需要配置支持 lua 去翻其他的代码吧:

```lua
function M.access_by_net(name) 
    local client_ip =  ngx.var.remote_addr or "0.0.0.0"
    local pts,pts_len = U.findAll(client_ip,'%.')
    if pts_len == 3 then
        local network = string.sub(client_ip,0,pts[3])
        local allow_hosts = {}
        local allow_nets = {}

		--- 这部分就可以自己定义了
        if name == "sec" then
            allow_nets = {"10.222.28."}
        elseif name == "es" then
            allow_nets = {}
            allow_hosts = {"10.210.12.17"}
        end 

        if U.inArray(allow_hosts,client_ip) or U.inArray(allow_nets,network) then
            return true
        end 
    end 
    return ngx.exit(403)
end
```

nginx 自己里边配置就比较简单了:
```shell
access_by_lua "tools.access_by_net('es')";
```


