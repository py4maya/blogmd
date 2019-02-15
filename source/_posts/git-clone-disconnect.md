---
title: git 断点clone  
date: 2019-02-15 17:30:11
tags: [git]
categories: 开发
---

### 断点续传下载代码:

```shell

$ mkdir git/dir
$ cd git/dir 
$ git init 
$ git fetch https://github.com/docker/distribution.git 
$ git checkout FETCH_HEAD -b master
$ git remote add origin https://github.com/docker/distribution.git

```
