---
title: golang.org 下载失败问题解决
date: 2019-01-23 08:27:33
tags: [golang]
categories: 
---

#### 第一步： 从github.com 下载对应的镜像包:

```shell
git clone https://github.com/golang/net.git $GOPATH/src/github.com/golang/net
git clone https://github.com/golang/sys.git $GOPATH/src/github.com/golang/sys
git clone https://github.com/golang/tools.git $GOPATH/src/github.com/golang/tools
git clone https://github.com/golang/crypto.git $GOPATH/src/github.com/golang/crypto
```

#### 第二步：

将net、sys、tools三个文件夹放到$GOPATH/src/golang.org/x目录下。 或者Linux下可以创建软连接：
```shell
ln -s $GOPATH/src/github.com/golang $GOPATH/src/golang.org/x
```
