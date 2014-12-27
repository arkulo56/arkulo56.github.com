---
layout: post
title: "swift版<<多多装修>>学习坎坷经历之---本地消息提示notifications"
description: ""
category:
tags: [swift]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明

用swift做本地消息提示，主要是两部分工作就可以：     

1. 请求设备允许使用系统消息提示
2. 定时提醒的代码

###第一部分工作
因为不同的ios版本申请使用代码不同，请注意版本！在AppDelegate.swift文件的   

```
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
```

这个函数中加入以下代码：

`ios8`

```
        let notificationType = UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
        let settings = UIUserNotificationSettings(forTypes: notificationType, categories: nil)
        application.registerUserNotificationSettings(settings)
```

`ios7`

```
	application.registerForRemoteNotificationTypes(UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound)
```

当然可以进行版本查询，然后实现if else方式进行多版本兼容，这个很简单，请google。

###第二部分工作
因为消息提示是不涉及可显示组件，因此在代码(例如：ViewController.swift)任何想实现通知的地方都可以加入，具体的参看该函数：


```
    func createAndFireLocalNotificationAfterSeconds(seconds: Int) {
        
        UIApplication.sharedApplication().cancelAllLocalNotifications()
        let notification = UILocalNotification()
        
        let timeIntervalSinceNow = Double(seconds)
        notification.fireDate = NSDate(timeIntervalSinceNow:timeIntervalSinceNow)
        notification.timeZone = NSTimeZone.systemTimeZone()
        notification.alertBody = "这是要发送的消息"
        
        UIApplication.sharedApplication().scheduleLocalNotification(notification);
        
    }

```

notification变量是消息主题   
NSDate是提取当前时间加上seconds秒（就是希望多少秒之后进行提示）    
NSTimeZone.systemTimeZone这是设置时区     


到这里工作就完成了，在模拟器上做的时候没有效果，我在真机上做的没问题。


