---
title: pip配置可用的其他更新源
date: 2017-10-05 16:28:26
tags: [pip]
categories: 开发
---

创建当前用户目录下的.pip子目录(~/.pip/),创建文本文件，写入内容为:
```shell
[global]
trusted-host=mirrors.aliyun.com
index-url=http://mirrors.aliyun.com/pypi/simple/
```

---
*注意* 一定要在当前用户目录下创建对应的文件
