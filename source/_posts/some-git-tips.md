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
git log --oneline <branch_name> 
git cherry-pick commit-id   ### 合并某次提交到当前分支
git push origin upstream  

* 删除远程分支:
git branch -d <dev_banch> # 删除本地分支
git push origin --delete <del_branch> #删除远程分支

* 删除远程commits，谨记，错误提交加密信息到github时候需要:   
   操作还是很麻烦的，不小心就丢了代码了，所以小心，小心再小心操作
```shell
git checkout -b <n_branch>   ## 创建临时分支
git reset --hard <commit_id> ## 退回到之前的版本
git push -b origin <n_branch>   # 推送一个远程分支
git cherry-pick                 # 合并指定提交
git branch -d master            #master 分支出错，先删除远程分支
git push origin --delete master #删除master的远程分支
git git checkout -b master      #重新生成master 分支
git push -b origin master       #推送本地master分支到远程 
```

* 合并fork源提交到当前master ，用于fork 的项目:

```shell

$ git fetch https://github.com/opensec-cn/kunpeng.git master  ## 或者其他分支的名字
$ git checkout FETCH_HEAD -b remote # 或者自己定义的名字
$ git checkout master
$ git log remote ^master
$ git cherry-pick commit-id
$ git push origin master
```
