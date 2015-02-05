---
layout: post
title: "swift版textfield输入完毕之后关闭虚拟键盘"
description: "iphone手机端的键盘弹出后，没有办法直接关闭，因此需要特殊的处理一下"
category:
tags: [swift]
---
{% include JB/setup %}     
> 请尊重原创，转载请注

iphone手机端的虚拟键盘弹出后（例如在使用textField的时候），没有直接的键可以关闭它，因此需要做一些处理，其实很简单，代码如下：

让类继承协议：

	class ViewController: UIViewController,
            UITextFieldDelegate｛

然后在下面的代码中实现：

	
	/**
	* 当textField将输入完毕之后，关闭键盘
	*/ 
    func textFieldShouldEndEditing(textField: UITextField) -> Bool {
        textField.resignFirstResponder()
        return true
    }
    
    /**
    * 当键盘打开后，点击textField之外的任何地方，就关闭键盘
    */
    override func touchesBegan(touches: NSSet, withEvent event: UIEvent) {
        self.view.endEditing(true)
        
    }
    
    