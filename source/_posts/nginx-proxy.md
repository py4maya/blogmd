---
title: 使用nginx制作全代理
date: 2017-09-21 20:47:02
tags: [nginx,linux,proxy]
categories: 系统
---

* 编译nginx,编译脚本:

```shell
./configure \
--prefix=/data0/nginx/ \
--with-stream \
--with-http_ssl_module  \
--with-stream_ssl_module \
make -j4
make install
```

* 主nginx配置文件：

```shell
http {
	resolver 10.212.0.43  valid=3600s;

	include       mime.types;
	default_type  application/octet-stream;

	log_format  main  '$remote_addr $http_host - $remote_user [$time_local] "$request" '
		'$status $body_bytes_sent "$http_referer" '
		'"$http_user_agent" "$http_x_forwarded_for"';
	sendfile        on; 
	keepalive_timeout  65; 
	gzip  on; 
	include vhost/*.conf;
}
```

* http代理比较简单:

```shell
#vhost/proxy.conf
server {
	listen  80;
	error_log logs/proxy_err.log;
	access_log logs/proxy_ac.log main;
	
	location / { 
		proxy_pass http://$http_host;
		proxy_set_header X-RealIP $remote_addr;
		proxy_set_header Host $http_host;
	}   
}
```

* https代理:
生成证书:
```shell
openssl genrsa -des3 -out common.key 1024
openssl req -new -key common.key -out common.csr
openssl rsa -in common.key -out common_nopwd.key
openssl x509 -req -days 365 -in common.csr -signkey common_nopwd.key -out common.crt
echo "ssl on;"
echo "ssl_certificate ssl/common.crt;"
echo "ssl_certificate_key ssl/common_nopwd.key;"
```

* 配置https代理:

```shell
server {
	listen  443;
	error_log logs/proxy_ssl_err.log;
	access_log logs/proxy_ssl_ac.log main;
	
	ssl on;
	ssl_certificate ssl/common.crt;
	ssl_certificate_key ssl/common_nopwd.key;
	
	location / { 
		proxy_pass https://$http_host;
		proxy_set_header Host $http_host;
	}   
}
```

* 最后一步，你绑定需要的hosts就可以把当前nginx机器伪装成需要访问的主机。当前nginx主机通过resolver 来解析需要访问的域名对应的ip
然后，去访问该ip端口对应的数据返回给浏览器。
