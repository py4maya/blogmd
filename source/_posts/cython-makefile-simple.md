---
title: cython 环境下使用Makefile 编译简单的包
date: 2018-04-13 17:39:27
tags: [cython,makefile]
categories: 开发
---

* 安装cython
```shell
$ pip install cython
```

* 创建一个文件夹，用来放要写的包里的文件(简单的包，只限单层)
```shell
$ make sample
```

* 试着编写几个py脚本，当然你可以按照规则编写pyx 文件，后缀而已，不用担心:
```shell
$ cat a.py

def infoa():
    print "info a "

$ cat b.py


def infob():
    print "info b "

```

* 编写一个简单的__init__.py 用来包含你生成的so文件:
```shell
$ cat __init__.py

import a
import b
__all__ = [a,b]

```

* 写入这个Makefile吧:
```shell
#### http://www.ruanyifeng.com/blog/2015/02/make.html
#### make package=<name>
.PHONY: clean all

sofiles=$(shell ls *.py | grep -v "__init__")

ifndef package
	package="build"
endif

all : build
	mkdir -p $(package)
	cp __init__.py $(package)
	cp *.so $(package)

build : $(sofiles:.py=.so)

%.c : %.py
	cython $<

%.o : %.c
	$(CC) -c -fPIC $< `python-config --libs` `python-config --includes`

%.so : %.o
	$(CC) -o $@ $< -fPIC  --shared `python-config --libs`

clean:
	rm *.c *.o *.so
```

* 然后执行,make package=<name> 就会生成一个<name>的文件夹，把他copy到需要的地方,就可以了用了.

