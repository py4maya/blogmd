---
title: yaf 多模块配置 
date: 2019-04-03 14:48:48
tags: [yaf,php]
categories: 开发
---

### 配置选项中，添加当前的模块:

```shell
application.modules = 'Test,Index'
```


### 创建相应的目录:

```
[root@hids application]# tree modules/
modules/
└── Test
    └── controllers
        └── Hello.php

2 directories, 1 file
```

### 访问地址:

http://yourhost/prefix/test/hello/world

注: 地址去掉prefix 以后超过三段，才会去识别模块
