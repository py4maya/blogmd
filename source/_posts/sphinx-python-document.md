---
title: 使用sphinx 生成python 包的注释文档 
date: 2019-03-04 15:30:11
tags: [sphinx]
categories: 开发
---

### 安装shpinx 

```shell
$ pip install sphinx
```

### 创建代码目录，或者切换到代码目录,初始化sphinx 配置

```shell
$  sphinx-quickstart 
```

### 修改 source/conf.py

```python

import os
import sys
sys.path.insert(0, os.path.abspath('../'))

```

### 生成rst 文件:

```shell
sphinx-apidoc -o  ./source code 
```

### 生成Html 文档:

```shell
make html
```


*Makefile 修改*

```makefile

# Minimal makefile for Sphinx documentation
#

# You can set these variables from the command line.
SPHINXOPTS    =
SPHINXBUILD   = sphinx-build
SOURCEDIR     = source
BUILDDIR      = build

# Put it first so that "make" without argument is like "make help".
help:
	@$(SPHINXBUILD) -M help "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

.PHONY: help Makefile

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
%: Makefile
	sphinx-apidoc -o  ./source xsdk
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

```
