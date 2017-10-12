---
title: 使用python实现tail
date: 2017-10-09 18:38:58
tags: [python,tail]
categories: 开发
---

关键点:

* 打开指定文件
* 每隔一段时间尝试readline ，有内容print , 否则sleep 
* 移动文件指针到文件末尾

关键代码:

```python

#coding=utf-8

import sys
import time
import re

def watchFile(fname,**kwargs) :

	"""
	watchFile(fname,delay=1,reg=False,handler=None,last_pos=0,last_size=0)
	用来监控文件的变化，可能有少量的数据缺失
	"""

	def echoHandler(items):
		print items

	offset,where = kwargs.get('last_pos',(0,2))
	last_size = kwargs.get('last_size',0)
	handler = kwargs.get('handler',echoHandler)
	delay = kwargs.get('delay',1)
	reg = kwargs.get('reg',False)
	last_pos = 0

	while True:
		try :
			with open(fname,'r') as f:
				if last_pos == 0 :
					f.seek(offset,where)
				else :
					f.seek(last_pos)
				line = f.readline()
				if line != None and line != '': 
					if reg != False :
						items = reg.findall(line)
					else :
						items = [line.strip()]
					if len(items) != 0 :
						handler(items)
				n_pos = f.tell()
				last_pos = n_pos
				f.seek(0,2)
				file_size = f.tell()
				if file_size < last_size :
					last_pos = 0
				last_size = file_size
		except Exception,e:
			pass
		time.sleep(delay)

if __name__ == "__main__" :
	if len(sys.argv) < 2 :
		print "python -u tail.py <file>"
		exit()
	fname = sys.argv[1]
	reg_ = re.compile("(error)|(disconnect)|(blocked)")
	watchFile(fname,reg=reg_)

```

