---
layout: post
title: "apache configure 静态编译和动态编译的通俗解释"
description: ""
category: 
tags: []
---
{% include JB/setup %}
编译apache的时候，要./configure添加编译参数，那到底该添加哪些？都是什么意思？弄清楚这些是很重要的，不过首先我们要明白什么是静态和动态编译的区别:       

静态：把所有的module都包含到httpd中，变成httpd自身的一部分，安装httpd就等于安装好了一切        

动态：module是单独的，httpd编译好后，不包含module，如果想用module,可以通过loadmodule调用module.so        

 

那怎么设定动/静态编译呢，就是在./configure后面到底如何写，才能区别动态和静态？      

静态：默认安装就是静态      

动态：enable-mods-shared=module 或者 enable-module=shared 指定某个module动态编译，再或者enable-mods-shared=most/all设置大部分或者全部module都用动态编译       

 

除此之外，我们还需要注意什么？          

1 为了防止编译安装的时候丢了某些module，我们还是需要让httpd支持动态安装的，所以我们需要--enable-so

2 with-mpm的值常用的是 worker和prefork，去google查查，两者有什么区别，按实际需求设定

3 有些module是不需要的，那就要disable掉，比如--disable-userdir,去google吧

 

所谓的“编译参数优化”        

搜索引擎里有很多apache编译安装参数优化的记录，特别感激分享这些内容的朋友，不过个人有个小小的建议，鞋合适不合适，只有脚知道，所以编译成功的httpd，我们都应该测试一下，是不是像文章中说的那样有效果，不可人云亦云~~~        

 

旁注：技术尚浅，不到之处勿喷~~       