---
title: yaf 控制器分层处理
date: 2019-04-03 14:48:48
tags: [yaf,php]
categories: 开发
---

主要分两个处理办法:

##  分模块处理:

配置选项中，添加当前的模块:

```shell
application.modules = 'Test,Index'
```

创建相应的目录:

```shell
[root@hids application]# tree modules/
modules/
└── Test
    └── controllers
        └── Hello.php

2 directories, 1 file
```

访问地址:
http://yourhost/prefix/test/hello/world

* 注: 地址去掉prefix 以后超过三段，才会去识别模块 * 

## 分文件夹存储:

Controller 中创建对应的文件夹:
比如 controllers/Flow/Strategy.php

文件中的类名为 Flow_StrategyController

URL 的访问对应关系: 

* <prefix>/flow_strategy/list  -> class listAction 方法

