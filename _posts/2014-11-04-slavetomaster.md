---
layout: post
title: "mysql:用一个slave顶替坏的master"
description: ""
category:
tags: [mysql]
---
{% include JB/setup %}  

##背景   
数据库的一个master硬件坏了，决定用一个完整的slave顶替这个master，充当一个新的master。此数据库中数据量比较大，在此将过程记录一下。     
##步骤   
1. 首先决定用多个slave中的哪个做为新的master    
查看`master_log_file`和`read_master_log_pos`就可以确定哪个slave同步的数据最多，选这个slave就可以了
2. 配置新的master（就是上一步选好的slave）    
＊ slave stop    
＊ reset slave all   
＊ 在my.cnf中关闭read_only=true(如果没有该选项，就确定该选项是off的就可以)   
＊ 重启mysqld服务，查看show slave status; 这时候应该没有信息了    
3. 配置slave同步新master    
这里的原理就是`slave服务器应该从新的master服务器的哪个binlog文件的哪个pos位置开始同步数据！`，那办法如下：    
＊ 首先查看slave同步的最后的binlog文件是哪个文件，然后找到最后执行的语句是什么？在slave服务器上执行`show master status;`可以看到最后同步的binlog文件    
＊ 然后在相应的文件中查找最后执行的语句`mysqlbinlog /var/lib/mysql/mysql-bin.000003 | tail -n20` 看最后执行的语句
＊ 在新的master服务器的所有binlog文件中查找这个语句（当然先从最新生成的文件中查）`mysqlbinlog mysql-bin.000001 | grep "INSERT INTO stuff VALUES (NULL, 'shin noon yard him', '2013-04-09 07:00:08')" --context 10`  grep "####" 双引号中部分就是上一步得到的执行语句，最后应该看到类似于 `   BEGIN    /*!*/;    \# at 19085 `   
那这里的`mysql-bin.000001`和`19085`就是我们要找的文件和pos点     
4. 设置slave的master属性
＊ slave stop 先停掉salve
＊ CHANGE MASTER TO MASTER_HOST='slave_b.example.com', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=19085
＊ start slave启动slave （如果不放心还可以重启一下服务）    

##比对数据
像背景中所说，数据量很大的情况下，做好了slave转master后，是不是能验证一下两边的数据是否相同呢？可以采取下面的办法：    
`mysqldump -u root important --skip-dump-date | md5sum
38f3cbbdb073619c744465b4c48c0463`    
在master和slave都执行该语句，看看得到的md5值是不是相同，如果相同则数据一致，否则不同     

##测试
如何进行测试想必大家都知道，因为我们采用的是blackhole（黑洞）引擎，在中继上是没有数据的，只能在更下一级的slave上测试数据，不过这里做一个备注，如果你的slave是balckhole，他的binlog format如果是row或者mixed模式，delete和update操作是不写入binlog的，下一级slave也就不会同步，所有测试的时候需要考虑到这个问题，做了delete和update操作没有写binlog还以为配置不成功呢！






