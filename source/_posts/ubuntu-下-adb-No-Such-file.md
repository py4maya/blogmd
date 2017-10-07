---
title: ubuntu 下 adb No Such file
date: 2017-03-03 14:34:19
tags: [ubuntu,adb,android]
categories: 系统
---


1、打开终端，安装 lib32z1（注意最后一位是数字 1 不是字母 l）

``` bash
sudo apt-get install lib32z1
```

2、完成后还需要安装 libstdc++.so.6 这个库（adb需要32位的库）：
``` bash
sudo apt-get install lib32stdc++6
```

OK，现在就可以使用adb命令了。
