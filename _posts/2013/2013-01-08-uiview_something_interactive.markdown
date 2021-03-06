---
author: demon
comments: true
date: 2013-01-08 20:58:45
layout: post
slug: uiview_something_interactive
title: UIView动画播放的一些认识
wordpress_id: 132
categories:
- iOS
- 开发
tags:
- animation
- UIView
- userInteractive

---

如果对一个UIView做一个动画，刚开始都是用最原始的写法：

在[UIView beginAnimations:nil context:NULL] 后设置好动画的一些属性之后，[UIView commitAnimations]然后播放，之后用了Block之后发现原来可以这么简单，最简单的动画一句话就可以搞定

    
    + (void)animateWithDuration:(NSTimeInterval)duration 
    				  animations:(void (^)	(void))animations

于是就一直这么用了。但直到前几天才发现如果这么用的话，应用在5.0之前的系统跑可能是有问题的。

假如我们在播放动画的同时，需要相应交互的事件，那么5.0之前就必须设置相应的UIViewAnimationOptions，

    
    + (void)animateWithDuration:(NSTimeInterval)duration
					      delay:(NSTimeInterval)delay
				        options:(UIViewAnimationOptions)options 
    				 animations:(void (^)(void))animations 
	  				 completion:(void (^)(BOOL finished))completion


因为在5.0之前，动画播放的时候，系统会把整个应用内所有的事件相应都停止，在苹果的官方[document](http://developer.apple.com/library/ios/#documentation/uikit/reference/UIView_Class/UIView/UIView.html#//apple_ref/occ/clm/UIView/animateWithDuration:delay:options:animations:completion:)里也可以找到说明：

	During an animation, user interactions are temporarily disabled for the views being animated. (Prior to iOS 5, user interactions are disabled for the entire application.)

假设你的动画时间很长（循环播放淡出淡出等等），那有会造成界面好像卡在那边，不管你怎么点击，事件都不会得到相应。([stackOverFlow](http://stackoverflow.com/questions/3897279/difference-between-uiview-beginanimationscontext-and-uiview-animatewithdura))
