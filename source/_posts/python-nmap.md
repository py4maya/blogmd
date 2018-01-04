---
title: python nmap 库的简单实用
date: 2017-11-09 15:06:18
tags: [python,nmap,fping]
categories: 开发
---


###### 实用nmap之前可以借助fping 来踩点，查找存活的主机:
```shell 

yum install fping
fping -g 1.1.1.1/24

```


###### nmap 安装

```shell
yum install nmap
easy_install pip
pip install python-nmap
```

##### 扫描脚本

```python
#coding=utf-8

import json
import nmap

nm = nmap.PortScanner()
ret = nm.scan('----','22')

next_ips = []
for ip in ret['scan'] :
	status = ret['scan'][ip]['tcp'][22]['state']
	if status == 'open' :
		next_ips.append(ip)

for ip in next_ips :
	print "-----"
	print ip
	ret = nm.scan(ip,'0-65535')
	for port in ret['scan'][ip]['tcp'] :
		x = ret['scan'][ip]['tcp'][port]
		if x.get('state') == 'open' :
			## x 是这样一个dict : {"product": "", "state": "open", "version": "", "name": "ssh", "conf": "10", "extrainfo": "protocol 2.0", "reason": "syn-ack", "cpe": ""}
			print json.dumps(x)
	

```
