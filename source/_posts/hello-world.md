---
title: 从头到尾搭建hexo博客
---

## 安装hexo 

### 安装nodejs,npm 

``` bash
$ npm install -g hexo
```
有的时候，需要在命令前，添加sudo


### 创建博客目录,并cd到对应的目录

``` bash
$ mkdir blog 
$ cd `pwd`/blog
$ pwd
```


### 生成对应的页面文件 

``` bash
$ hexo generate
```

查看更多: [生成页面](https://hexo.io/docs/generating.html)

### 部署到github

``` bash
$ hexo deploy
```

有时候会报错:ERROR Deployer not found: github
解决办法如下,运行如下命令:

``` bash
$ npm install hexo-deployer-git --save
```

### 生成新的文章:

``` bash
$ hexo new <layout> <title>
```

其中layout的值为: post, page, draft 或者其他的内容

当修改内容以后，需要调用以下命令，重新生成，重新部署:


``` bash
$ hexo g && hexo d 
```


