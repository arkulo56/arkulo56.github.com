---
layout: post
title: "swift版<<多多装修>>学习坎坷经历之---应用集合视图collectionView"
description: ""
category:
tags: [swift]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明

做web如果要生成一个图片列表是很简单的，php读出数据，在html里做一下循环就搞定了，可是在ios这种移动设备中，如何做图片列表呢？很简单，用collectionView就可以。

一步步的来做吧

###界面
在故事板中添加collection view controller，然后设定其中的一个cell（也就是第一个小方块）的Identifier值为FlickrCell,并且在代码中声明一个常量

```
private let reuseIdentifier = "FlickrCell"
```

下面几个函数就是必须要实现的方法：    

    
    //一共多少组
    override func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
        return 1
    }
    
    //每组多少个
    override func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return 7
    }
    
    //每个cell的数据
    override func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCellWithReuseIdentifier(reuseIdentifier, forIndexPath: indexPath) as UICollectionViewCell
        
        cell.backgroundColor = UIColor.blackColor()
        var label:UILabel! = cell.contentView.viewWithTag(1) as? UILabel
        
        label!.text="12"
        label!.textColor=UIColor.whiteColor()
        
        // Configure the cell
        return cell
    }

    //cell大小，宽和高各50
    func collectionView(collectionView: UICollectionView!,
        layout collectionViewLayout: UICollectionViewLayout!,
        sizeForItemAtIndexPath indexPath: NSIndexPath!) -> CGSize {
            
            return CGSize(width: 50, height: 50)
    }
    
    //所有cell整体相对于容器的边距
    private let sectionInsets = UIEdgeInsets(top: 20.0, left: 20.0, bottom: 50.0, right: 20.0)
    
    func collectionView(collectionView: UICollectionView!,
        layout collectionViewLayout: UICollectionViewLayout!,
        insetForSectionAtIndex section: Int) -> UIEdgeInsets {
            return sectionInsets
    }
    

好了，完成这些就可以成功测试了