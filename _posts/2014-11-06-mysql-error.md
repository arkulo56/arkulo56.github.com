---
layout: post
title: "mysql服务宕机，Got error 127/124 when reading table错误尝试解决"
description: ""
category:
tags: [mysql]
---
{% include JB/setup %}     
一台用了很久的服务器mysql数据库版本是5.0.45,最近长提示链接满了`ERROR 1040: Too many connections` 字面意思就是链接太多了，服务器不响应了！那就上服务器上仔细研究一下，看看究竟是什么原因引起的该问题。          

+ ### 登录服务器首先看看错误信息，发现如下：
  
 ` [ERROR] Got error 124 when reading table`不能读某个特定的表，随google查到一篇文章[Debugging error codes 124 and 127](http://forums.mysql.com/read.php?132,191348,191348#msg-191348),其中的一个回答是：    
> Hi! Please test 5.0.67. Most likely it'll solve these problems. Another thing to try is set concurrent_insert=0 in my.cnf. If you decide to upgrade, be sure to mysqldump and reimport those tables to get a 'fresh start'.     

遂仔细查看concurrent_insert的概念
     
+  ### mysql的配置选项：concurrent_insert的概念      
myisam引擎有读写并发的问题，主要是写（更新）的优先级高于读，极端的时候读是一直抢不到所有权。那concurrent_insert就是来设置该行为的。      
	concurrent_insert=0   关闭并发      
	concurrent_insert=1   如果表中没有空数据块时，读写可并发
	concurrent_insert=2   不管有没有空数据块，读写并存，写都在数据文件尾部      

+ ### 解决并发还有其他办法吗？    
1> 使用--low-priority-updates启用mysqld。这将给所有更新(修改)一个表的语句以比SELECT语句低的优先级。在这种情况下，在先前情形的最后的SELECT语句将在INSERT语句前执行。     
2> 为max_write_lock_count设置一个低值，使得在一定数量的WRITE锁定后，给出READ锁定      
3> 使用LOW_PRIORITY属性给于一个特定的INSERT，UPDATE或DELETE较低的优先级     
4> 使用HIGH_PRIORITY属性给于一个特定的SELECT     
5> 使用INSERT DELAYED语句

+ ### 会不会和max_connections参数有关     
用该命令可以查看系统当前设置的max_connections值`show variables like 'max_connections';`    
用该命令可以查看系统过去的最大链接数`show global status like 'Max_used_connections';`     
max_used_connections的值最好不要超过max_connections的80%（该服务器的设置还算合理）     

+ ### 最终的尝试     
采用--low-priority-updates参数提高读的优先权，具体的可以在my.cnf中设置如下：     
> [mysqld]     
low-priority-updates


最终结果等有实验结果后进行补充，各位请视自己系统需求进行设置！


 


