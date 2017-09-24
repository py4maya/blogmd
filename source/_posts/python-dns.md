---
title: 使用python 实现一个dns proxy
date: 2017-09-23 10:23:58
tags: [python,dns,proxy]
categories: 开发
---

dns验证主python脚本。运行基本流程如下:
* 主server 基于gevent.server.DatagramServer , 
* 核心逻辑方法handle
* handle中首先通过DnsParser 解析传过来的dns数据包，如果解析失败，直接返回给用户127.0.0.1
* query.qname 解析出来的是需要解析的域名，本代码通过正则匹配 domain.A中的配置项,如果解析出来，并且查询类型是A记录就直接返回
* 如果没有相应的解析内容选择forward,forward默认为goolge 8.8.8.8 ,如果匹配了domain.innet中那么直接转发innet 
* forward 逻辑封装在udp_send中 ，forward 以后把返回的数据直接扔给用户
* config.LOCALDNS中配置一个转发的Hash 
```python
LOCALDNS = {
		'innet':( "10.210.12.10",53) ,
		'google':( "10.210.12.10",53) 
}
```

* config.server_bind 设置需要绑定的ip 端口 :
```python
port = 53 if len(sys.argv) < 2 else int(sys.argv[1])
server_bind = '10.210.3.100:%d' % (port)
```

* 域名的配置代码如下:
```python
#domain.py
A = {
	"*.sina.com":"10.210.3.100",
	"*.sina.com.cn":"10.210.3.100",
	"*.weibo.com":"10.210.3.100",
	"weibo.com":"10.210.3.100",
	"*.weibo.cn":"10.210.3.100"
}

FORWARD = [
	"autodiscover.staff.sina.com.cn",
	"mail.staff.sina.com.cn" 
]

innet = [
	"*.sina.com.cn",
	"*.weibo.com",
	"*.weibo.cn"
]
```

* 主循环，主逻辑的代码如下:
```python
# dns_server.py
# -*- coding=utf-8 -*-
import os
import struct
from cStringIO import StringIO
from collections import namedtuple
from gevent import socket
from gevent.server import DatagramServer
import domain
import config
import logging

Hex = lambda x : '0x{0:04x}'.format(x) # Hex(256) => "0x0100"

QueryResult = namedtuple("DnsQuery",
	"transactionID,flags,questions,answerRrs \
	authorityRrs,additionalRrs,qname,qtype,qclass"
)

def parseIpList(data):
	assert isinstance(data, basestring)
	iplist = ['.'.join(str(ord(x)) for x in s) for s in re.findall('\xc0.\x00\x01\x00\x01.{6}(.{4})', data) if all(ord(x) <= 255 for x in s)]
	return iplist


def preg_match(preg,real):
	"""
	only support '*'
	>>>preg_match("www.*.test*.com","www.python.test.com")
	True
	>>>preg_match("www.*.test*.com","www.python.tes.com")
	False
	"""
	pre = 0
	for s in preg.split('*'):
		now = real.find(s)
		if now < pre:
			return False
		pre = now +len(s)
	return True

def udp_send(address,data):
	sock = socket.socket(type=socket.SOCK_DGRAM)
	sock.connect(address)
	sock.send(data)
	response, address = sock.recvfrom(8192*4)
	return response,address

class DnsParser:
    
	@classmethod
	def parseQuery(self,query):
		"""
		       6a 02 01 00 00 01                         j.....
		00 00 00 00 00 00 03 77 77 77 03 61 61 61 03 63  .......www.aaa.c
		6f 6d 00 00 01 00 01                             om.....
		
		dns query package like above
		03 77 77 77 : three www
		
		"""
		transactionID,flags,questions,answerRrs,authorityRrs,additionalRrs = map(Hex,struct.unpack("!6H",query[:12]))
		quries = StringIO(query[12:])
		c = struct.unpack("!c",quries.read(1))[0]
		domain = []
		while  c != '\x00':
			n = ord(c)
			domain.append(''.join(struct.unpack("!%sc" % n,quries.read(ord(c)))))
			c = struct.unpack("!c",quries.read(1))[0]
		domain = '.'.join(domain)
		qtype,qclass = map(Hex,struct.unpack("!2H",quries.read()))
		return QueryResult(transactionID,flags,questions,answerRrs,authorityRrs,additionalRrs,domain,qtype,qclass)
	
	@classmethod
	def generateReqponse(self,queryData,ip):
		"""
		only support ipv4
		"""
		return ''.join([
		  queryData[:2],
		  "\x81\x80\x00\x01\x00\x02\x00\x00\x00\x00",
		  queryData[12:],
		  "\xc0\x0c",
		  "\x00\x01",
		  "\x00\x01",
		  "\x00\x00\x00\x1e",
		  "\x00\x04",
		  struct.pack('BBBB',*map(int,ip.split('.')))
		])

class DnsServer(DatagramServer):
	def handle(self,data,address):
		try:
			query = DnsParser.parseQuery(data)
		except Exception,e:
			response = DnsParser.generateReqponse(data,"127.0.0.1")
			self.socket.sendto(response,address)
			return 

		find = False
		for preg,ip in domain.A.iteritems():
			if preg_match(preg,query.qname):
				find = True
				break

		if query.qname  in domain.FORWARD :
			find = False

		if find and query.qtype == "0x0001": #only handle A record
			response = DnsParser.generateReqponse(data,ip)
			self.socket.sendto(response,address)
		else:
			forward = 'google'
			for preg in domain.innet:
				if preg_match(preg,query.qname):
					forward = 'innet'
			response,serveraddress = udp_send(config.LOCALDNS.get(forward),data)

			#try:
			#	iplist = parseIpList(response)
			#	logging.info("  %s " % (iplist))
			#except Exception,e:
			#	pass

			logging.info("%s %s",address,query.qname)
			self.socket.sendto(response,address)

if __name__ == "__main__":
	logging.info("start ok")
	DnsServer(config.server_bind).serve_forever()
```
