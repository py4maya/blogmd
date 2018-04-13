---
title: python2.7中字符编码的解决办法
date: 2018-01-18 10:00:03
tags: [python]
categories: 开发
---

不想给自己找编码问题麻烦的话，加上如下的代码吧,程序入口处加就可以:

```python

import sys 
reload(sys)
sys.setdefaultencoding("utf-8") 

```

然后，你就可以彻底的跟 字符编码问题bye bye 了。。。。

** 添加上这几行代码后，不会对以前写好的encode有影响，所以，你可以放心大胆的把你之前写过的代码也加上这几行吧  **
