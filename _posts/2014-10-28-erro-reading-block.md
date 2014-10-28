---
layout: post
title: "服务器不能启动，error reading block"
description: ""
category:
tags: [swfit]
---
{% include JB/setup %}  

公司有一台服务器是mysql的master，刚好昨天我休息，同事说连接数满了，问我是不是可以先重启一下mysql，当时没多想就同意他先试试（自己会新开一篇文章说说遇到这种情况怎么做），结果在重启的过程中，mysql一直在写磁盘，我的同事也没经验，一着急就给服务器重启了！最后的直接结果就是服务器死了，起不来了！    
错误提示：    
`error reading block 12877911 (attempt to read block from file system resulted in short read) while getting next inode from scan`   
这个只能去机房fsck了，还好服务器是有备机的，要不真的就哭了！    
还是那句话：    
> 对在线要求较高的业务逻辑，设计高可用和数据备份等方面是要仔细的，要不一旦遇到问题就麻烦了