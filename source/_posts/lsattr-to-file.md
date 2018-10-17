---
title: lsattr chattr
date: 2018-10-17 14:19:06
tags: [ lsattr,chattr,linux ]
categories: linux
---
Linux的chattr命令， chattr命令用于改变文件属性。这项指令可改变存放在ext2文件系统上的文件或目录属性，这些属性共有以下8种模式：
* a：让文件或目录仅供附加用途。
* b：不更新文件或目录的最后存取时间。
* c：将文件或目录压缩后存放。
* d：将文件或目录排除在倾倒操作之外。
* i：不得任意更动文件或目录。
* s：保密性删除文件或目录。
* S：即时更新文件或目录。
* u：预防以外删除。

lsattr -a 可以查看对应的属性

对于特殊属性的操作
chattr +i name
chattr -i name
