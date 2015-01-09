---
layout: post
title: "ios查看CoreData数据保存SQLite文件路径（swift版本）"
description: ""
category:
tags: [swift]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明


经常用到coredata做本地数据，插入更新删除等操作都需要调试，因此需要解决两个问题：

####问题

1. SQLite文件保存路径在哪里？
2. 用什么工具查看找到的SQLite文件


####答案



    func applicationDirectoryPath() -> String {
        return NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true).last! as String
    }
    
这个函数可以返回文件保存路径

***


sqlitebrower这个软件可以安装在mac上，管理本地数据

***
完了！！