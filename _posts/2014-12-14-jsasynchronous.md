---
layout: post
title: "javascript异步执行"
description: ""
category:
tags: [sso]
---
{% include JB/setup %}    

js是单线程的，执行起来是顺序的，在顺序的业务逻辑中当然没有问题，如果遇到可以并发执行的业务逻辑，如果排队就很低级了！这里我们来简单的解释一下，如何在普通的js代码中实现异步执行（Asynchronous）。    

```
function a()
{
        var n = 0;
        for(var i=1;i<10000000;i++)
        {
                n=i＋n;
        }
        document.getElementById('a1').value=n;
}
function b()
{
        document.getElementById("a2").value=22222;
}
a();
b();
```

看上面的这两个函数，在执行的结果写入两个input时，肯定是a函数执行完毕后，再执行b函数，这就是单线程排队。如何异步执行这两个函数，也就是说，a执行较慢，b不用等待a结束就直接执行。      
##setTimeout  神秘的函数     
这个函数就是异步的关键所在，看代码：     

```
function c(fuc)
{
        setTimeout(function(){
                a();
                fuc;
        },1000);
}
c(b());
```
在这里用setTimeout()执行a函数和b函数，则会产生异步执行，b函数不会等待a执行完毕。     

setTimeout函数的小问题：       

1. 等待的时间不精准，会有延时
2. try..catch不能捕获错误信息   

以上办法还是基于js单线程的概念，如果感兴趣，可以了解一下javascript多线程编程。