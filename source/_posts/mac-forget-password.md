---
title: mac 忘记开机密码
date: 2018-01-12 16:59:21
tags:
categories:
---

*  在单用户模式下启动 Mac 设备。

*  打开终端窗口，然后输入以下命令：

mount -uw /

*  Capitan 或 Yosemite 系统用户分别输入以下两个命令：

launchcti load /System/Library/LaunchDaemons/com.apple.taskgated.plist
launchctl load /System/Library/LaunchDaemons/com.apple.opendirectoryd.plist

而 Mavericks、Mountain Lion 或 Lion 系统用户则输入以下命令：

launchctl load /System/Library/LaunchDaemons/com.apple.opendirectoryd.plist

需要注意的是，如果你不知道用户的短名称，可以输入以下命令来获取用户列表(短名称位于左侧，完整名称位于右侧)：

dscl localonly ls /Local/Default/Users realname

*  输入以下命令，并将 shortname 替换为管理员用户的短名称(用户个人文件夹的名称)：

passwd shortname

系统提示时，输入新的帐户密码，但是你可能看不到任何表明正在键入的指示。

*  输入 reboot 命令以重新启动 Mac。

