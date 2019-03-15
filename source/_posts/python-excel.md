---
title: python 打开excel  
date: 2019-03-15 16:41:45
tags: [python,xlrd]
categories: 开发
---

```python

#coding=utf-8

import logging
import xlrd

logging.basicConfig(level=logging.DEBUG,format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',datefmt='%a, %d %b %Y %H:%M:%S')

def xlsLoop(xlsName):
	wb = xlrd.open_workbook(xlsName)
	names = wb.sheet_names()
	
	for name in names:
		sheet = wb.sheet_by_name(name)
		for rownum in range(sheet.nrows):
			row = sheet.row_values(rownum)
			colnum = 0
			for col in row:
				#logging.debug("<%s> [%d,%d] %s",name,rownum,colnum,col)
				colnum += 1 
				yield (name,rownum,colnum,col)


for sheet,rownum,colnum,col in xlsLoop('test.xlsx'):
	logging.info("%s %s %s %s ",sheet,rownum,colnum,col)

```
