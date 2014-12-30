---
layout: post
title: "教程：swift下使用collectionView＋coreData原理＋代码注释"
description: "通俗易懂的collectionView使用教程，真实的例子"
category:
tags: [swift]
---
{% include JB/setup %}     
> 部分翻译文章


##基本原理部分



<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swiftcollectionview/1.png" width="600" />

从ios6版本开始有了collection view这个控件，程序员们经常会使用该控件，因此我们希望用swift语言重新回顾以下该控件。    

collection view（以下简称cv）与tableview非常的相似，多了scroll horizontal和goodbye scroll两种功能。    

一个vc包含三部分组件：   

1. cells：显示你数据的item
2. Supplementary Views：每个section（可以多组显示）的头部尾部和自定义内容的区域
3. Decoration Views: 背景图等全局视图

>举例：
一个书架，一层放字典，二层放杂志，三层是小说    
cells: 每本书就是一个cell      
supplementary: 每一层以及贴“字典”、“杂志”的地方     
decoration：书架本身

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swiftcollectionview/2.png" width="600">

总结以下思路是：

1. 先给stroyboardy里拉一个collection view（不是controller），并给cell设置一个identifiy
2. 要想在viewcontroller里处理这个cv，就需要让该类做delegate和datasource（UICollectionViewDelegateFlowLayout,UICollectionViewDataSource）
3. cv的初始化，cell的宽高，上图的内容（上下左右），还有代理，还有数据
4. 重写函数：section的数量函数、要显示数据总和函数、每个cell数据显示函数
5. coredata部分：原理见我的[这篇文章](http://www.arkulo.com/2014/12/28/coredata/)
6. coredata函数：demo中只有初始化和增加
7. 给cell一个单独的class，来具体的设定cell的内容 


##代码部分

`viewcontroller.swfit`



	import UIKit
	import CoreData

	class ViewController:
    UIViewController,
    UICollectionViewDelegateFlowLayout,
    UICollectionViewDataSource
    {
    //collectionview
    @IBOutlet  var cv: UICollectionView!

    //数据源
    var dataArr:Array<AnyObject>! = []
    var data:NSManagedObject!
    var content:NSManagedObjectContext!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        //数据读取
        content = (UIApplication.sharedApplication().delegate as AppDelegate).managedObjectContext
        //添加一次数据
        addData()
        //初始化数据
        initData()
        
        
        //布局
        let layout: UICollectionViewFlowLayout = UICollectionViewFlowLayout()
        layout.sectionInset = UIEdgeInsets(top: 20, left: 10, bottom: 10, right: 10)
        layout.itemSize = CGSize(width: 90, height: 90)
        
        //collectionview初始化
        cv = UICollectionView(frame: self.view.frame, collectionViewLayout: layout)//UICollectionView(frame: self.view.frame, collectionViewLayout: layout)
        cv!.dataSource=self
        cv!.delegate=self
        cv!.registerClass(myCollectionViewCell.self, forCellWithReuseIdentifier: "newCell")
        cv!.backgroundColor=UIColor.whiteColor()
        self.view.addSubview(cv)


    }
    //集合的section数量，如果是一个可以不写该函数
    func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
        return 1
    }
    //一个section中包含多少个item
    func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return dataArr.count
    }
    //一个item中的数据，myCollectionView是单独的一个cell类
    func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCellWithReuseIdentifier("newCell", forIndexPath: indexPath) as myCollectionViewCell //这里是cell单独的类名，见第二部分代码
        
        cell.backgroundColor=UIColor.blackColor()
        var z: AnyObject! = dataArr[indexPath.row].valueForKey("zhangdanri")
        var h: AnyObject! = dataArr[indexPath.row].valueForKey("huankuanri")
        cell.textLabel?.text = "\(z)~\(h)"
        cell.textLabel1?.text=dataArr[indexPath.row].valueForKey("bank") as? String
        
        return cell
    }
    
    //数据--------------------------
    //添加一次数据
    func addData()
    {
        var row:AnyObject = NSEntityDescription.insertNewObjectForEntityForName("Credit", inManagedObjectContext: content!)
        var bank="arkulo"
        var zhangdanri=10
        var huankuanri=8

        row.setValue(bank, forKey: "bank")
        row.setValue(zhangdanri, forKey: "zhangdanri")
        row.setValue(huankuanri, forKey: "huankuanri")
        content?.save(nil)
            
    }
    //初始化数据
    func initData()
    {
        var f = NSFetchRequest(entityName: "Credit")
        dataArr = content.executeFetchRequest(f, error: nil)
    }
    
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


	}
	

针对cell的class文件名：`myCollectionViewCell.swift`



	import UIKit

	class myCollectionViewCell: UICollectionViewCell {
    
    required init(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
    }
    
    let textLabel: UILabel!
    let textLabel1: UILabel!
    //let imageView: UIImageView!
    
    override init(frame: CGRect) {
        super.init(frame: frame)

        //一个label
        let textFrame = CGRect(x: 0, y: 32, width: frame.size.width, height: frame.size.height)
        textLabel = UILabel(frame: textFrame)
        textLabel.font = UIFont.systemFontOfSize(UIFont.smallSystemFontSize())
        textLabel.textAlignment = .Center
        textLabel.textColor=UIColor.greenColor()
        contentView.addSubview(textLabel)
        
        //第二个label
        let textFrame1 = CGRect(x: 0, y: -30, width: frame.size.width, height: frame.size.height)
        textLabel1 = UILabel(frame: textFrame1)
        textLabel1.font = UIFont.systemFontOfSize(22)
        textLabel1.textAlignment = .Center
        textLabel1.textColor=UIColor.whiteColor()
        contentView.addSubview(textLabel1)
        
        
    }

	}

在CoreDate中的数据表名为：Credit，数据结构是：

		bank	string
        zhangdanri	int  
        huankuanri	int
  
最终运行效果：    

<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swiftcollectionview/4.png" width="300" />







