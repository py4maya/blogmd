---
title: 像使用虚拟机一样使用docker
date: 2017-11-10 21:11:02
tags: [docker,graph]
categories: 系统
---


##### 前提，系统中已经包含docker，不管你是ubuntu系统或者centos系统。

* pull需要的系统,本实例中使用ubuntu

```shell
docker pull ubuntu
```

* 创建系统初始化脚本,其中包含了，如果对系统做了修改，保存相应的镜像:
  绑定一两个需要对外服务的端口，注意，如果你的机器有公网IP,那么慎用0.0.0.0作为转换IP
  友情提示,使用内网的IP地址监听
  
  脚本会创建一个以$name为名字的镜像及容器 
  首次运行会提示不存在对应的容器，镜像等直接忽略就可以了
```shell
#!/bin/bash

lip=172.16.7.57 ##只暴露端口给内网的IP
name=xbox
p=" -p ${lip}:8080:8080 -p ${lip}:9090:9090 -p {$lip}:5678:5678"


docker commit ${name} ${name}
docker stop ${name}
docker rm ${name}

docker run -it -p ${lip}:8080:8080 -p ${lip}:9090:9090 --name ${name} ubuntu /bin/bash
docker start ${name}

```

* 系统的进入脚本，相当于ssh 到对应的虚拟主机:
 直接用-it 模式运行/bin/bash 就可以

```shell
#!/bin/bash
docker exec -it xbox /bin/bash
```

* 建议安装的脚本:

```shell

apt-get update
apt-get install git
apt-get install vim
apt-get install gcc
apt-get install automake
apt-cache search make
apt-get install make
apt-get install pcre
apt-get install libpcre
apt-cache search pcre
apt-get install libpcre3
apt-get install libpcre3-dev
apt-get install zlib
apt-cache search zlib
apt-get install libghc-zlib-dev
apt-cache search zlib
apt-get install lua-zlib-dev
apt-get install zlib1g-dev

```

* 如果你乐意还可以用Dockerfile的方式创建一个基本镜像:

Dokcerfile 内容如下:
```shell
FROM ubuntu

RUN apt-get update -y && apt-get install git -y && apt-get install vim -y && apt-get install gcc -y && apt-get install automake -y && apt-cache search make -y && apt-get install make -y && apt-get install pcre -y && apt-get install libpcre -y && apt-cache search pcre -y && apt-get install libpcre3 -y && apt-get install libpcre3-dev -y && apt-get install zlib -y && apt-cache search zlib -y && apt-get install libghc-zlib-dev -y && apt-cache search zlib -y && apt-get install lua-zlib-dev -y && apt-get install zlib1g-dev -y 

WROKDIR /root
```

最后运行 docker build . -t xbox 也可以生成对应的镜像


*注:默认的docker目录过于小， 建议修改启动参数中的   --graph=/var/lib/docker 到一个比较大的目录*
