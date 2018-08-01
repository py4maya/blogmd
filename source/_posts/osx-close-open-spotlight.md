---
title: osx关闭开启spotlight 服务
date: 2018-08-01 01:36:40
tags: [osx,spotlight]
categories:
---

必要的时候，需要关闭spotlight 服务，节省系统资源.

```shell
sudo mdutil -a -i off
```

重新开启服务:
```shell
sudo mdutil -a -i on
```

