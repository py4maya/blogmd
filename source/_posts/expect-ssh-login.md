---
title: expect 脚本自动登录ssh服务器
date: 2017-10-05 17:00:33
tags: [expect,ssh]
categories: 系统
---

配置如下的expect脚本:
```shell
#!/usr/bin/expect

set timeout 3600

trap {
 set rows [stty rows]
 set cols [stty columns]
 stty rows $rows columns $cols < $spawn_out(slave,name)
} WINCH

set mode [lindex $argv 0]

switch $mode {

	"" {
		set user xingyue
		set ip "ip1"	
		set port 9022
		set auth key
	}

	"mini" {
		set user xingyue
		set ip "ip2"
		set pwd "your_pwd"
		set port 22
		set auth pwd
	}

}

switch $auth {

	"key" {
		spawn ssh $user@$ip -p $port
		interact
	}

	"pwd" {
		spawn ssh $user@$ip -p $port
		expect "*password:"
		send "$pwd\r"
		interact
	}

}



```
