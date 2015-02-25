---
layout: post
title: "常规多级分类（产品分类，文章分类）在mysql中的存储方式"
description: "这篇文章就是介绍一下我们最常用的多级分类的实现方法"
category:
tags: [mysql]
---
{% include JB/setup %}     
> 请尊重原创，转载请注

在日常工作中，我们会经常碰到产品分类，文章分类等等`修改不频繁`的多级分类。通常的做法是类似于这样的结构：      

###常规做法


<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/duocengji/1.png" width="500" />


如果按照多级查询的话，采用以下sql语句就可以：

	SELECT t1.name AS lev1, t2.name as lev2, t3.name as lev3, t4.name as lev4
	FROM category AS t1
	LEFT JOIN category AS t2 ON t2.parent = t1.category_id
	LEFT JOIN category AS t3 ON t3.parent = t2.category_id
	LEFT JOIN category AS t4 ON t4.parent = t3.category_id
	WHERE t1.name = 'ELECTRONICS';
	
查询结果如下：

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/duocengji/2.png" width="600" />

缺点：

> 举一个实际的例子吧－－产品分类一共三级，所有的产品关联的分类均是第三级叶子节点。现在的需求是，通过顶级分类，查询所有这个大分类下包含的产品？

###左值右值方法

那我们来看看这种方式，先看原理吧

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/duocengji/3.png" width="600" />

我们用嵌套的方式来表达多层关系，建表的时候加上左右值，结构如下：


<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/duocengji/4.png" width="600" />

那这时候你肯定会问，左右值是怎么计算出来的？请接着看下面两张图：

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/duocengji/5.png" width="600" />

和

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/duocengji/6.png" width="600" />

估计你看完这两张图后，就明白其中的技巧了，那我们来看看这样的结构有什么好处？

我们来解决常规做法中的那个产品分类的问题

> 假设顶级分类选择的就是根节点，然后要查询所有旗下的产品    
> select category_id from category where lft>1 and rgt<20

要是想查询子树也是同样的道理～

> 扩展一下，还可以给每条记录加入父节点id字段，这样也就拥有了基础方法的属性

缺点：

> 如果要加入和删除一个节点，就要重新对所有节点进行左值右值计算！！！！


参考官方的文档：[http://ftp.nchu.edu.tw/MySQL/tech-resources/articles/hierarchical-data.html](http://ftp.nchu.edu.tw/MySQL/tech-resources/articles/hierarchical-data.html)





