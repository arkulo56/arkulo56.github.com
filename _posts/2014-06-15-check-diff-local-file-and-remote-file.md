---
layout: post
title: "check diff local file and remote file"
description: ""
category: 
tags: []
---
{% include JB/setup %}
总觉得rsync同步的时候丢了文件，所有用shell写个小脚本检测一下远程目录和本地目录的文件差异，代码如下：

>代码高亮插件有问题，正在解决中～～～

*如果文件名称中有空格会认为是两个文件，算是个未解决的问题*
