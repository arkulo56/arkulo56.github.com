---
layout: post
title: "给UICollectionViewCell绑定手势（用long press举例）swift版本"
description: ""
category:
tags: [swift]
---
{% include JB/setup %}     
> 请尊重原创，转载请注明


手势绑定功能是最常用的，但是没有经验的朋友，第一次用swift实现对UICollectionView集合中每个cell进行手势绑定，就有些茫然了。如果对swift手势和collectionview的实现还不是很清晰的话，请查看我这两篇[swift版本手势功能简单讲]()以及[教程：swift下使用collectionView＋coreData原理＋代码注释](http://www.arkulo.com/2014/12/30/swiftcollectionview/)，剩下的就是我遇到的较痛苦的问题：如何给UICollectionViewCell绑定手势，`就是给每个cell绑定手势！！`   


####首先得实现代理

	class ViewController:
    	UIViewController,
    	UICollectionViewDelegateFlowLayout,
    	UICollectionViewDataSource,
    	UIGestureRecognizerDelegate    //手势的代理
    {
    	......
    }


####给每个cell实现代码绑定手势

    func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCellWithReuseIdentifier("newCell", forIndexPath: indexPath) as myCollectionViewCell
        
        cell.backgroundColor=UIColor.blackColor()
        var z: AnyObject! = dataArr[indexPath.row].valueForKey("zhangdanri")
        var h: AnyObject! = dataArr[indexPath.row].valueForKey("huankuanri")
        cell.textLabel?.text = "\(z)~\(h)"
        cell.textLabel1?.text=dataArr[indexPath.row].valueForKey("bank") as? String
        //这个tag是很有用的
        cell.tag = indexPath.row
        //最重要是下面这三行，记住是在这里绑定手势的，我刚开始一直不知道
        lp = UILongPressGestureRecognizer(target: self, action: Selector("longPress:"))
        lp.delegate = self
        cell.addGestureRecognizer(lp)
        return cell
    }


####实现long press的callback函数

    func longPress(recognizer: UILongPressGestureRecognizer){
    	//这就是你当前选中的cell的数组下标
    	var index = recognizer.view!.tag
        //代码部分
        if(recognizer.state == .Began)//很重要！！就是按0.5秒触发事件的开始
        {
        	.....
        }
    }
    
    
每个事件都是有状态的，状态也是一种过程：开始－》改变－》结束 ，如果上面的代码不进行开始判断，则长按0.5秒后你的实际代码会执行好几次！！！   

1. Began 
2. Changed
3. Ended
4. Cancelled
5. Failed