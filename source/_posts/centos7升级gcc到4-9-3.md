---
title: centos7升级gcc到4.9.3
date: 2017-03-07 16:54:31
tags: [gcc,centos7]
categories: 系统相关
---

1. 下载源代码:

从[https://gcc.gnu.org/mirrors.html](https://gcc.gnu.org/mirrors.html)选择一个相对快的镜像库,我选择的是
http://www.netgull.com/gcc/releases/

2. 下载必要库:


```shell
$ ./contrib/download_prerequisites
```

3. 编译依赖c++
```shell
$ yum install gcc-c++
```

4. 创建安装文件编译目录:
```shell
$ mkdir objdir  && cd objdir
$ ../configure --enable-languages=c,c++ --disable-multilib
$ make 
$ make install
```

