---
title: govendor使用安装
date: 2019-07-25 12:57:06
tags: [golang,govendor]
categories: 开发
---

## 安装 

```shell
$ go get -u -v github.com/kardianos/govendor
```

## 添加依赖 

```shell
$ govendor init
$ govendor fetch project_url_with_out_http 
$ cat vendor/vendor.json
```
