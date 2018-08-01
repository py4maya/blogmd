---
title: centos 下一个安装superset 的shell
date: 2018-07-11 16:55:08
tags: [superset]
categories:
---

```shell
#!/bin/bash

pip install --upgrade setuptools
yum install gcc python-devel g++ gcc-c++ cyrus-sasl cyrus-sasl-devel -y


exit

fabmanager create-admin --app superset

# Initialize the database
superset db upgrade

# Load some data to play with
superset load_examples

# Create default roles and permissions
superset init

# Start the web server on port 8088
superset runserver -p 8088

# To start a development web server, use the -d switch
# superset runserver -d


```
