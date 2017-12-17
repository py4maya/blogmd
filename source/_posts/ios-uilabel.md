---
title: ios UILabel 使用
date: 2017-10-28 19:10:51
tags: [ios,UILabel]
categories: 开发
---

##### 简单知识点介绍 :

* UIColor 相关, RGB 模式中,各个值的范围是0~1 , clearColor 为透明色
* 文本对齐方式, label 的 textAlignment 属性，值为 NSTextAlignmentCenter 
* 多行显示文本 numberOfLines 设置文本显示的行数, 0|-1 为不控制显示行数,行数多少 不会超过label的高度
* 自动计算文本所占的宽高， 用字符串的 sizeWithFont 方法


##### 示例代码 :

```c
 //尺寸，颜色 clearColor 为透明色
    //对齐方式设置label的textAlignment 值为  NSTextAlignmentCenter ,
    //colorWithRed用RGB的方式设置颜色，注：R,G,B,alpha的值都是0~1
    //字体设置，设置UIFont的特殊值
    CGRect xsize = [UIScreen mainScreen].bounds;
    UILabel *l = [[UILabel alloc]initWithFrame:CGRectMake(20, 50 , xsize.size.width - 40 , 30 )];
    l.backgroundColor = [UIColor clearColor];
    l.text = @"Hello This is Big World!";
    l.textColor = [UIColor colorWithRed:0.8 green:0 blue:0 alpha:1];
    l.textAlignment = NSTextAlignmentCenter;
    l.font = [UIFont systemFontOfSize:18];
    
    l.shadowColor = [UIColor greenColor];
    l.shadowOffset = CGSizeMake(0, 1);
    
    UILabel *bl = [[UILabel alloc]initWithFrame:CGRectMake(20, 100, xsize.size.width - 40, 200)];
    bl.text = @"      这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字这是一个特别长的文字";
    
    //显示行数，0 或者 -1 不设置
    bl.numberOfLines = 0;
    
    //计算文字所占的size
    CGSize textSize = [bl.text sizeWithFont:bl.font constrainedToSize:CGSizeMake(xsize.size.width - 40, 50000)];
    bl.frame = CGRectMake(20, 100, textSize.width, textSize.height);


    
    [self.view addSubview:bl];
    [self.view addSubview:l];
```
