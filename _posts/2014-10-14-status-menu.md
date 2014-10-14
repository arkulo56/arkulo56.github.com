---
layout: post
title: "swift版<<多多装修>>学习坎坷经历之---ios关闭状态栏"
description: ""
category:
tags: [swfit]
---
{% include JB/setup %}    


这个问题网上其实有很多结果，我为什么写呢，主要是因为自己加的navigation（导航栏）和状态栏有重叠，所以只好无奈的将状态栏隐藏，此篇文章就算是存档吧，看以后是否有更好的解决办法！！       

ios关闭状态栏代码：     

    override func prefersStatusBarHidden() -> Bool {
        return 1
    }
    
在UIViewController里面重写这个函数就可以了，还有推荐一篇文章吧      
[http://ios.9tech.cn/news/2013/1104/38423.html](http://ios.9tech.cn/news/2013/1104/38423.html)