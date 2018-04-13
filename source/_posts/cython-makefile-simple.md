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
	rm -f *.c *.o *.so
	rm -rf build

```

* 然后执行,make package=<name> 就会生成一个<name>的文件夹，把他copy到需要的地方,就可以了用了.

* 多个包的时候可以这样设置目录结构:
![sapmle picture](/images/cython-project.jpg)
简单解释如下:
    app.py 用来放置启动的程序，当然，你为了放置繁琐，可以用python -c "import <package> ; package.app.start()" 来运行，默认情况下,程序执行流程如app.py 执行
    dist 用来存放编译好的so文件
    Makefile 项目总的Makefile 一般是一个模板，内容将在下边给出，会遍历子目录里进行make
    README.md 用来写一些说明吧
    .... 其他的一些文件夹，就是你写好的python 程序了,注意，每个文件夹里，需要按照以上的结构来设置

* 总的Makefile 内容如下:
```shell

.PHONY:all clean
exclude_dirs := Makefile README.md dist app.py
dirs := $(shell ls)
dirs := $(filter-out $(exclude_dirs),$(dirs))
all:
	$(foreach N,$(dirs),make -C $(N);)
clean:
	rm -rf dist 
	rm app
	rm app.c
	$(foreach N,$(dirs),make -C $(N) clean;)

dist: all
	mkdir dist
	cython app.py --embed
	$(CC) -o app app.c `python-config --libs` `python-config --includes`
	cp app dist
	$(foreach N,$(dirs),cp -R $(N)/build dist/$(N);)

```

* 执行 make dist . 你将获得一个dist 文件夹. 其中就是你编译好的目标文件. dist/app 就是你的可执行文件。
然后，你就可以开心的把这些so文件拿到其他地方去运行了,至少隐藏了一部分你的源代码，而且，速度上应该比纯python运行快一点儿

* 要是偷懒的的话，可以直接clone 整理好的这个项目: [https://github.com/qixingyue/cython-starter](https://github.com/qixingyue/cython-starter)
