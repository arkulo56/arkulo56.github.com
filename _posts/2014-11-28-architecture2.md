---
layout: post
title: "收集服务器架构之二［简单架构－单独跑php-fpm］"
description: ""
category:
tags: [架构]
---
{% include JB/setup %} 

###架构图：    
![架构图](https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/architecture/2.jpg)    

1. nginx（Tengine）做为负载均衡服务器
2. 在112服务器上实现NFS服务器端，然后113和115两台php服务器客户端共享文件
3. 数据层用mysql或者mariaDB 

来源：[http://blog.linuxeye.com/358.html](http://blog.linuxeye.com/358.html)