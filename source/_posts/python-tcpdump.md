---
title: 使用python的pcap模块构建tcpdump系统
date: 2017-09-26 10:17:43
tags: [python,tcpdump,pcap]
categories: 开发
---

libcap-devel 及 pypcap 安装
```shell
$ yum install libpcap-devel 
$ pip install pypcap
$ pip install dpkt
```

简单的抓取80端口的数据包:
```python
#coding=utf-8

import pcap
import dpkt

pc = pcap.pcap('eth0')

pc.setfilter('tcp port 80')
for ptime,pdata in pc:
	frame = dpkt.ethernet.Ethernet(pdata)
	ip = '%d.%d.%d.%d' % tuple(map(ord,list(frame.data.dst)))
	transfer = frame.data.data.__class__.__name__
	print ip,transfer
	print frame.data.data

```

数据分析的几个关键点:
* pcap.pcap的参数网卡name 一般是ech0
* pc.setfilter 数据包的过滤器,比如 tcp port 80 , udp port 53 filter语法遵循[bpf语法](/bpf/)
* 解析成frame frame = dpkt.ethernet.Ethernet(pdata) 数据的原内容放在frame.data.data中
