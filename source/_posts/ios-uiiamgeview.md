---
title: ios 通过UIImageView 显示图片
date: 2017-10-28 19:18:17
tags: [ios,UIImage,UIImageView]
categories: 开发
---
##### 简单知识点:

* 获取资源所在的目录 : [[NSBundle mainBundle] resourcePath]
* 加载图片的方法 initWithContentsOfFile 用路径的方式加载, imageNamed 用named的方法加载（比较占用系统内存）
* 图片显示大小跟载体有关系,设置载体的ContentMode 属性 可以显示不同的拉伸，居中等显示方式


##### 相关代码: 
```c

//当前的资源目录
    NSString *path = [[NSBundle mainBundle] resourcePath];
    NSString *imagePath = [NSString stringWithFormat:@"%@/running.png",path];
    UIImage *image = [[UIImage alloc]initWithContentsOfFile:imagePath];
    
    //尺寸大小跟载体有关
//    UIImage *image = [UIImage imageNamed:@"running"];
    UIImageView *iv = [[UIImageView alloc]initWithFrame:CGRectMake(20, 300, 300, 300)];
    iv.contentMode = UIViewContentModeCenter;
    [iv setImage:image];
    
    [self.view addSubview:iv];
    [self.view sendSubviewToBack:iv];

```

##### 显示效果截图:

![tiny app board](/images/ios_sample_world.png )

