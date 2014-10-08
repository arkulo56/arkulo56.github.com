---
layout: post
title: "swift版<多多装修>学些坎坷经历之－－几种页面跳转方式"
description: ""
category:
tags: [swfit]
---
{% include JB/setup %}   
＋ 第一种,很简单，直来直往

```   
//跳转
self.presentViewController  
//返回
self.dismissModalViewController 
```   
＋ 第二种,必须在当前视图有navigation,要不实现不了   

```   
self.navigationController?.pushViewController  
self.navigationController?.popViewController 
```

示例代码   
   
```   
let myStoryBoard = self.storyboard     
let loginView:UIViewController = myStoryBoard?.instantiateViewControllerWithIdentifier("login") as UIViewController                   
//login是视图的storyboard id    
self.navigationController?.pushViewController(loginView, animated: true)    
```
