---
title: nginx vim 设置语法高亮
date: 2019-01-24 07:21:11
tags: [nginx,vim]
categories:

---

#### 下载文件语法插件:
```shell
cd /usr/share/vim/vim74/syntax && wget -O nginx.vim https://vim.sourceforge.io/scripts/download_script.php?src_id=19394
```

#### 设置文件类型：
```shell

vim /usr/share/vim/vim74/filetype.vim 
au BufNewFile,BufRead nginx_path    setf nginx

```
