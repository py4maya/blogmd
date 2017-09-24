---
title: xcode 使用cocoapods
date: 2017-09-22 16:01:32
tags: [xcode,pods,brew]
categories: 开发
---

* 安装cocoapods:

```shell
gem sources --remove https://rubygems.org/
gem sources -a https://gems.ruby-china.org/
gem sources -l
sudo gem install cocoapods
```

* 新建xcode project sample 保存到 ~/Documents/samplle

* 新建Podfile 安装相应的库:

```shell
#Podfile:
platform :ios, '7.0'
target 'sample' do
pod 'AFNetworking', '~> 3.1.0'
end

```

* 执行安装:

```shell
cd ~/Documents/sample
pod install
```
