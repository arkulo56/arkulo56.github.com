---
layout: post
title: "swift版本CoreData多表关联Relationship代码实现"
description: "介绍relationship的代码操作"
category:
tags: [swift]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明


####数据结构

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/relationship/1.png" width="600" />


####关键点

1. 用Create NSManagedObject Subclass自动生成的数据表class文件，需要加入`@objc(Money)`，这样就不报错了
2. 在生成的Credit的class属性mm，类型是NSSet，在给该值赋值的时候应该是这样的 `c1.mm = NSSet(array: [m1]) `


####实际代码


	import UIKit
	import CoreData

	class ViewController: UIViewController {

    var content:NSManagedObjectContext!
    //读取两张表的数据，保存在以下数组中
    var dataArr:Array<AnyObject>! = []
    var dataBrr:Array<AnyObject>! = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
        content = (UIApplication.sharedApplication().delegate as AppDelegate).managedObjectContext
        //初始化的时候添加一次数据
        addData()
        //初始化的时候读取数据
        initData()
        //打印数据保存的真实物理路径
        println(applicationDirectoryPath())
        
        //测试打印数据
        println(self.dataArr[0].valueForKey("bank"))
        println(self.dataBrr[0].valueForKey("acount"))

    }
    
    func addData()
    {
        var c1 = NSEntityDescription.insertNewObjectForEntityForName("Credit", inManagedObjectContext: content!) as Credit
        var m1 = NSEntityDescription.insertNewObjectForEntityForName("Money", inManagedObjectContext: content!) as Money
        
        c1.bank = "lailailai"
        m1.acount = 10
        
        c1.mm = NSSet(array: [m1]) 
        m1.cc = c1
    
        content.save(nil)
    }
    func initData()
    {
        var f = NSFetchRequest()
        var entity = NSEntityDescription.entityForName("Credit", inManagedObjectContext: content!)
        f.entity = entity
        self.dataArr = content.executeFetchRequest(f, error: nil)
        
        var q = NSFetchRequest(entityName: "Money")
        self.dataBrr = content.executeFetchRequest(q, error: nil)
    }

    func applicationDirectoryPath() -> String {
        return NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true).last! as String
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


	}
	
	
以下是一个多条件查询的例子：


        
        var request = NSFetchRequest()
        var entity = NSEntityDescription.entityForName("Credit", inManagedObjectContext: content)
        request.entity = entity
        //where条件
        let wh = NSPredicate(format: "bank=%@", "nini")
        request.predicate = wh
        //排序
        let sort = NSSortDescriptor(key: "bank", ascending: false)
        let sorts = [sort]
        request.sortDescriptors = sorts
        //limit
        request.fetchLimit = 2
        //查询的字段
        request.propertiesToFetch = ["bank"]
        
        //执行查询
        dataArr = content.executeFetchRequest(request, error: nil)
