---
layout: post
title: "UML有用吗？如何把设计模式真正的用到开发中！"
description: "类图时序图是每个程序员的产出物之一"
category:
tags: [设计模式]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明

和开发主管聊天，说新人难带，主要凸显的问题在几个方面：

1. 对于开发文档的理解不够透彻（业务逻辑）
2. 对代码结构没有概念，一边写一边设计（代码结构思考不足，直接动手编码）
3. 不会简化代码，不会使用设计模式
4. Bug太多～～

大家讨论为什么会出现这种情况，总结为新手写代码很多时候是不能发现全局的，因此在编码之前还是需要仔细的规划一下自己的代码的，那用什么来规划呢？

***在写代码前，应该根据产品文档,编写模块的类图和时序图，根据类图和时序图进行编码***

优点：

1. 将准备编写的代码图形化，可视化，不容易出现偏差
2. 在编码前就能分析代码结构和细节，容易提前发现设计上的漏洞
3. 类图和时序图可以作为开发人员的产出物提交给版本库
4. 这是CodeReview的依据
5. 其他人接手做二次开发较为容易
6. 类图看多了就知道该如何使用设计模式了

***强烈建议所有开发人员，尽可能的在编码之前做类图和时序图，完成后再动手编码***

###设计模式：类图＋时序图

在这里总结一下常用设计模式的类图和时序图，如果你经常画类图的画，一定能看出来应该用什么模式！

***

**创建型模式：**

* [简单工厂模式](https://github.com/arkulo56/thought/issues/43)
* [工厂方法模式](https://github.com/arkulo56/thought/issues/44)
* [抽象工厂模式](https://github.com/arkulo56/thought/issues/47)
* [建造者模式](https://github.com/arkulo56/thought/issues/48)
* [单例模式](https://github.com/arkulo56/thought/issues/49)


**结构型模式：**

* [适配器模式](https://github.com/arkulo56/thought/issues/50)
* [桥接模式](https://github.com/arkulo56/thought/issues/51)
* [装饰者模式](https://github.com/arkulo56/thought/issues/52)
* [外观模式](https://github.com/arkulo56/thought/issues/53)
* [享元模式](https://github.com/arkulo56/thought/issues/54)
* [代理模式](https://github.com/arkulo56/thought/issues/55)

***

**行为型模式：**

* [命令模式](https://github.com/arkulo56/thought/issues/56)

***持续更新中～～～***