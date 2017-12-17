---
title: ios UIView 基本操作
date: 2017-10-28 17:44:54
tags: [UIView,ios]
categories: 开发
---

##### 简单的知识点:

* 获取屏幕尺寸 : [UIScreen mainScreen].bounds.size 
* bounds 和 frame 的区别, bounds 又叫做线宽，x,y 永远为0 
* UIView 可以设置不同的tag属性，建议唯一 , 父视图的 viewWithTag 方法可以获取到指定tag的UIView
* 层级关系: bringSubviewToFront UIView 移动到最前, sendSubviewToBack , UIView 移动到最下层
* exchangeSubviewAtIndex 可以用来交换不同层的组件
* 遍历子视图 subviews 可以获取到一个NSArray 其中成员都是UIView 

##### 实例代码:
---

```c
 	//UIView 坐标根据父视图来设置，
    //距离上下左右各为x像素的UIView
    //其中20 为顶部的状态栏
    //frame bounds 区别 bounds中x,y永远为0
    //center 取view的中心点 x,y
    //通过获取父视图，设置背景
    CGRect screen = [UIScreen mainScreen].bounds;
    NSInteger x = 20;
    CGPoint center = self.view.center;
    UIView *xv = [[UIView alloc]initWithFrame:CGRectMake(x, x + 20 , screen.size.width - x * 2   , screen.size.height - x * 2 - 20 )];
    xv.backgroundColor = [UIColor blueColor];
    xv.tag = 0;

    NSLog(@" center is  : %f , %f ",center.x,center.y);
    
    [self.view addSubview:xv];
    
    UIView *superView = xv.superview;
    superView.backgroundColor = [UIColor blackColor];
    
    //遍历子视图，根据不同类型设置不同操作
    NSArray *childViews = [self.view subviews];
    for (UIView *sub in childViews) {
        switch (sub.tag) {
            case 0:
                NSLog(@"type 0 view");
                break;
            default:
                NSLog(@"no type view");
                break;
        }
    }
    
    //通过tag 获取view
    UIView *v = [self.view viewWithTag:1];
    if(v) {
        v.backgroundColor = [UIColor greenColor];
    }
    
    //交换不同层的组件
    [self.view exchangeSubviewAtIndex:2 withSubviewAtIndex:3];
    //指定层添加组件,可以添加到最上边，最下边
    [self.view insertSubview:v atIndex:10];
    
    //移到最底层
    [self.view sendSubviewToBack:v];
    //移动到最顶层
    [self.view bringSubviewToFront:v];

```
