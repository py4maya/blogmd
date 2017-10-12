---
title: ios hello world
date: 2017-10-12 09:19:27
tags: [ios,oc,pods,hello]
categories: [开发]
---

手写UI需要删除项目设置中的Storyboard配置:

General -> Deployment Info -> Main interface 中保持为空

AppDelegate.m中设置window及显示主ViewController:

```c

 	self.window = [[UIWindow alloc]initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    
    UIViewController *mainController = [[ViewController alloc]init];
    self.window.rootViewController = mainController;
    [self.window makeKeyAndVisible];
```

注：需要在AppDelegate.h中引入ViewController.h 

```c
	#import "ViewController.h"
```

在ViewController中加载所需的UI 组件:

```c
 	UILabel* label = [[UILabel alloc]init];
    label.text = @"hello ios world";
    [label sizeToFit];
    label.center = self.view.center;
    [self.view addSubview: label];
```

至此，一个ios的程序基本结构就完成了。结合前边的使用[pods](http://xsec.7ever.cn/2017/09/22/xcode-use-cocoapods/)配置依赖，就可以写ios了。
