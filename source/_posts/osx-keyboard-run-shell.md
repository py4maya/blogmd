---
title: 苹果系统下通过键盘快捷键运行shell
date: 2017-12-02 16:09:01
tags: [osx,shell,apple script,automator]
categories: 系统
---


* 创建系统服务
运行起automator ,
依次选择：文件->新建->服务
点开资源库->操作->运行Shell脚本或者其他脚本

编写自己的脚本及运行环境

保存成对应的名字，比如sample service .


* 绑定快捷键到系统服务:

选择系统偏好设置，键盘，快捷键，服务，选择刚新创建的服务
添加适当的快捷键，
关闭该窗口就可以用该快捷键执行shell


* automator 创建的服务在 ~/Services 目录,可以自行删减
