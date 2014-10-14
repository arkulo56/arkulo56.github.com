---
layout: post
title: "swift版<<多多装修>>学习坎坷经历之---slide menu 抽屉导航（新）"
description: ""
category:
tags: [swfit]
---
{% include JB/setup %}  

之前一篇文章中，做了一个类似于弹出层的效果，只是当时自己是用NSTimer去控制时间，结果还需要调整没个规定时间内要移动多少像素的位置，很麻烦，而且也不精准。       
今天有时间有重新做了，用了UIView的一个函数animateWithDuration，我的目标是让一个view进行左右移动，那通过这个函数，你只需要设置好view最后的位置以及移动到该位置需要消耗的时间，就可以有系统自动实现移动动画，并且完成动作！      
具体代码如下：     

        var c = self.view0.frame
        if(self.menuSign==0)  //menuSign是一个标示符，0代表侧边栏未打开
        {
            c.origin.x = -230
            UIView.animateWithDuration(0.7,delay:0,usingSpringWithDamping:0.5,initialSpringVelocity:1.0,options:UIViewAnimationOptions.AllowUserInteraction,animations:{
                self.view0.frame = c ;
                self.menuSign = 1
                },completion: { (finished: Bool) -> Void in
                    
            })
        }else
        {
            c.origin.x = 0
            UIView.animateWithDuration(0.7,delay:0,usingSpringWithDamping:0.5,initialSpringVelocity:1.0,options:UIViewAnimationOptions.AllowUserInteraction,animations:{
                self.view0.frame = c ;
                self.menuSign = 0
                },completion: { (finished: Bool) -> Void in
                    
            })
        }
               
               
      
 具体效果请参考ios百度视频左侧导航，具体代码请见 [源代码](https://github.com/arkulo56/pipilu/blob/master/pipilu/flowdetailViewController.swift)。       
 备注：具体函数的功能细节，请参看官网或google     