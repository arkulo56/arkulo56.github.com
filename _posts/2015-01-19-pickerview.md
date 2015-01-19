---
layout: post
title: "swift版本UIPickerView使用demo"
description: ""
category:
tags: [swift]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明


####效果图

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/Pickerview/1.png" width="300" />

####实现步骤

1. 在storyboard中选中pickerview，然后按住ctrl键，拉线将deleagte和datasource连接至当前controller
2. 拖拽拉线实现`@IBOutlet weak var pickerView: UIPickerView!`
3. 和tableview一样需要实现相应的函数
4. 实现pickerview选中触发函数`@IBAction func setSaraly(sender: UIBarButtonItem) `

####代码如下

	//
	//  salaryViewController.swift
	//  jiandanhuan
	//
	//  Created by 赵勇 on 15-1-15.
	//  Copyright (c) 2015年 赵勇. All rights reserved.
	//

	import UIKit
	import CoreData

	class salaryViewController: UIViewController {

    var dateSel = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31]
    
    @IBOutlet weak var pickerView: UIPickerView!
    @IBOutlet weak var textTF: UITextField!
    //当前选中的row
    var rowSelected:Int!
    //数据objectId
    var obId:NSManagedObjectID!
    //数据句柄
    var content:NSManagedObjectContext!
    var data:Array<AnyObject>! = []
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        //数据读取部分，每次只读一条数据
        content = (UIApplication.sharedApplication().delegate as AppDelegate).managedObjectContext        
        var rq = NSFetchRequest(entityName: "Saraly")
        var whe = NSPredicate(format: "sId=%@", "1")
        rq.predicate = whe
        data = content.executeFetchRequest(rq, error: nil)
        //println(data[0].valueForKey("daySaraly"))
        if(data.count>0)
        {
            var daySet: AnyObject! = data[0].valueForKey("daySaraly")
            obId = data[0].objectID
            textTF.text = "\(daySet)号"
        }else
        {
            initData()  //如果没有数据，就初始化一条数据进数据表
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    func initData()
    {
        var row = NSEntityDescription.insertNewObjectForEntityForName("Saraly", inManagedObjectContext: content!) as Saraly
        row.sId = 1
        row.daySaraly = 1
        content.save(nil)
        obId = row.objectID
    }
    
    
    //pickerview必须实现的函数
    func numberOfComponentsInPickerView(pickerView: UIPickerView) -> Int {
        return 1
    }
        
    func pickerView(pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
        return dateSel.count
    }
    
    // pragma MARK: UIPickerViewDelegate
    
    func pickerView(pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String! {
        return "\(dateSel[row])号"
    }
    //pickerview选中触发函数
    func pickerView(pickerView: UIPickerView!, didSelectRow row: Int, inComponent component: Int)
    {
        rowSelected = row
        textTF.text = "\(dateSel[row])号"
    }
    
	//右上角done触发函数
    @IBAction func setSaraly(sender: UIBarButtonItem) {
    
    	//用objectID修改数据    
        var dd = content.objectWithID(obId)
        dd.setValue(dateSel[rowSelected], forKeyPath: "daySaraly")
        content.save(nil)
        self.tabBarController?.selectedIndex=0
    }
    
    /*
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    override func prepareForSegue(segue: UIStoryboardSegue!, sender: AnyObject!) {
        // Get the new view controller using segue.destinationViewController.
        // Pass the selected object to the new view controller.
    }
    */

	}
