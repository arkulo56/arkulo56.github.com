---
layout: post
title: "收集服务器架构之一"
description: ""
category:
tags: [tcp/ip]
---
{% include JB/setup %}     

###架构图：     
![架构图](https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/architecture/1.jpg)       


1. lvs做负载均衡
2. 两台web服务器用rsync进行文件同步（使用的web服务器和脚本语言不详）
3. mysql实现读写分离
4. 加入一层memcache数据缓存
5. 独立的文件服务器，用apache cache做缓存 

来源：[http://blog.51yip.com/server/912.html](http://blog.51yip.com/server/912.html)