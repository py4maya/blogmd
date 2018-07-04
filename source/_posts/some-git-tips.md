---
title: 一些git的基本操作
date: 2018-05-04 09:37:06
tags: [git]
categories: 开发
---

* 创建分支:
git checkout -b <branch_name>

* 切换分支:
git checkout <branch_name>

* 对比分支的变化:
git log branch1 ^branch2    ##查看branch1 中包含，但是branch2中没有的变化

* 不明确的分支变化:
git log branch1...branch2    ##对比两个branch的变化

* 查看某次提交的详细变化:
git show <change_name>

* 合并某次变化到当前分支:

