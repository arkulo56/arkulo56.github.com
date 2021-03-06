---
layout: post
title: "简单的HashTable原理"
description: ""
category:
tags: [设计模式]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明


php语言内部很多地方使用HashTale来做为保存数据的结构，HashTable也是工作常见和常用的数据结构，下面就来说说它的原理。 

###基本原理

简单的讲，HashTable是一种存储数据的结构，就是用一个key对应一个value，我们可以通过key来查询这个value值。

那我们来构造一个最简单的HashTable，这个表只保存整数，默认它的大小是8位，来看看它的样子


<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/hashtable/1.png" width="300" />


###Hash函数

什么是Hash函数？其实就是传一个value值给这个函数，这个函数对其进行保存，并把保存的位置key返回给调用方。这是HashTable的构造过程。现在我们来做一个简单的Hash函数，用来向刚才设计的HashTable存入数据

	function hashFun($v)
	{
		$key = ($v+1)%8;
		...//保存数据
		return $key
	}

###插入数据

现在HashTable和Hash函数都准备好了，现在开始就插入数据吧。例如插入一个14，经过Hash函数的计算，返回的key的值应该是7，因此我们讲14这个数放置在编号7的位置上


<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/hashtable/2.png" width="300" />

相同的，33，82以及191，都会进入到计算好的位置上



<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/hashtable/3.png" width="300" />


###冲突

整个HashTable一共才8个位置，怎么可能放得下所有的value呢，例如现在要插入2这个数字，经过计算它的key是3，可是在位置3上，已经存在82了，该怎么办？

这里有几种解决办法，我们先来说简单的一种：

1. (2+1)%8 = **3**
2. 3号位置已经被占用
3. 将3这个数字再传给Hash函数进行计算，(3+1)%8 = **4**
4. 4号位是空着的
5. 将原来的2保存至4号位



<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/hashtable/4.png" width="300" />

简单的办法总是有缺点的，这样空间会很容易被占满，即使空间够大，保存的数据越多，碰到冲突的几率越大，而且多次计算的可能性也越大。

那我们重新回到插入2之前，来看看下面的方法是不是会好一些


<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/hashtable/5.png" width="300" />


可以将每一个位置看成一个数据列（链表什么的），然后变成类似于二维的一个关系，所有经过Hash函数返回相同key的value，都会顺序进入到这个二维的数据列中。只要系统的内存是够的，我们就可以一直增加数据进去



<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/hashtable/6.png" width="300" />

这里我可以再插入18，他的位置也是3

###获取数据

我们在这里存入的只是一个整数，实际工作中，可能存入的数据类型就比较复杂了，举个例子吧

> 一个班有很多学生，每个学生要保存相关信息（年龄、籍贯、成绩等等），我们将学生的姓名做为Hash函数的入参，经过计算得出一个HashTable中的位置，然后把他对应的相关信息（结构体）保存进去就可以了，读取的时候，只要把学生的名字再经过一次哈希函数，就知道数据存在哪个位置了


###总结


我们说哈希函数最好的状况下时间复杂度是O(1)，其实就是说，编写哈希函数是HashTable的关键问题，其中冲突解决就成了提高性能的关键因素。冲突，寻找下一个空位，再冲突，再寻找下一个空位...,周而复始，时间复杂度提升。因此，我们因该找到更好更快的解决冲突和查找的算法，不一定要线性的向后排队。






