---
layout: post
title: "swift版<<多多装修>>学习坎坷经历之---ios8+xcode6+真机(iphone5)详细设置"
description: ""
category:
tags: [swift]
---
{% include JB/setup %}   
> 请尊重原创，转载请注明
   
相信很多朋友和我一样对申请账号、真机测试等设置步骤比较头疼，苹果严谨的同时也增加了一定的难度。     

`申请开发者账号`在这里不再多余描述，网上有很多文章都写的非常详细，照着做就行。我遇到的问题是提示我不能验证身份，直接打电话给400-670-1855，这是中文客服，她会帮你解决所有问题，比较简单的，最后还会给你一个类似解决问题的追踪号码，如果你还有问题打过去，直接报这个号码对方就知道之前是怎么回事了，账号大家都知道了¥688 RMB.          

`真机测试`需要mac、iphone、数据线都准备好，在电脑上下载好xcode，我当前用的是6.0，然后下面我们就开始一步步的讲解这个过程。（虽然步骤看起来有些多，只要耐心的一步步做下来，基本都会成功）      

1. 账号申请好后，去[https://developer.apple.com/](https://developer.apple.com/)导航member center，使用apple id登录，将看到下图，然后点击选择`Certificates,identifiers & Profiles`    
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/2.png" width="600" />  <hr />
2. 随后看到该界面，然后在左侧中选择`Certificates`选项
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/3.png" width="600" />     <hr />
3. 这里到了证书列表页面，可以看到一下界面，随后点击右上角的添加（红色标记）符号，添加证书，列表里是我自己已经添加好的证书。
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/4.png" width="600" /><hr />
4. 打开页面后拉到最下面，可以看到红色标记部分，点击该链接后会下载文件
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/5.png" width="600" />  <hr />   
5. 下载的文件可以看到如此，然后双击该文件进行安装，直接点击安装即可
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/1.png" width="600" />  <hr /> 
6. 安装好后可以在'钥匙串访问中'看到该证书，然后选择该证书，并按照图片上的提示找到`从证书颁发机构请求证书`
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/6.png" width="600" />  <hr />        
7. 打开后看到如下界面，电子邮件填apple id的账号电子邮件，下面一定要选择存储的磁盘
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/7.png" width="600" />  <hr />
8. 完成后会在桌面导出一个文件‘CertificateSigningRequest.certSigningRequest’，这就是CSR证书
9. 现在回到苹果开发网站，就是刚才我们看到的证书添加的那个页面，在页面上部选择ios app development，然后点击最下面的continue
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/8.png" width="600" />  <hr />
10. 在这个页面将刚才得到的CSR证书上传，然后点击Generate生成
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/9.png" width="600" />  <hr />
11. 这里生成证书成功后会提供下载，把证书下载下来
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/10.png" width="600" />  <hr />
12. 这个证书就是`开发者证书`，双击安装他
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/12.png" width="600" />  <hr />
13. 开发证书搞定，现在去添加设备，回到网站，找到左侧导航中的Devices->all,我们点击右上角添加测试真机设备
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/13.png" width="600" />  <hr />
14. 在该页面中给设备随便起个名字，然后填写真机设备的UDID编号（每个手机或ipad都有）
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/14.png" width="600" />  <hr />
15. UDID获取方法（有多种，可以google），用usb链接手机至mac，在itunes里面可以看到该界面
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/31.png" width="600" />  <hr />
16. UDID获取方法，点击上图种红色标记部分，就可以看到我们想要的了
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/32.png" width="600" />  <hr />
17. 设置好设备后，我们要去设置一个`app ids`,在左侧导航中可以找到该选项，打开后也是选择右上角＋号进行添加，将看到该页面，name随便写，Bundle ID一般都是域名倒着写，类似我的是com.80day.www。
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/17.png" width="600" />  <hr />
18. 设置好后，去设置最后一个选项ProvisioningProfile，选中红色标记所示
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/18.png" width="600" />  <hr />
19. 在打开的界面中选择app ids，然后下一步
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/19.png" width="600" />  <hr />
20. 这步选择我们刚才的证书，再下一步
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/20.png" width="600" />  <hr />
21. 接着选择设备，再下一步
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/21.png" width="600" />  <hr />
22. 给profile起个名字吧，然后生成
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/22.png" width="600" />  <hr />
23. 最后下载文件，可以看到如下，先不做操作，开始设置xcode
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/24.png" width="600" />  <hr />
24. 打开xcode，然后选择图中选项
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/25.png" width="600" />  <hr />
25. 看到devices界面后，选中设备右键点击如图选项
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/26.png" width="600" />  <hr />
26. 看到如下界面，点击左下角添加按钮，选择刚才最后下载的证书进行添加
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/27.png" width="600" />  <hr />
27. 然后在打开的项目中，找到info.plist文件，修改其中的Bundle identifier选项为刚才设置的com.80day.www这个值
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/28.png" width="600" />  <hr />
28. 最后再设置一下你的设备目标，我的是ios7.1.2，如果你的版本是ios8，那你就设置成8就好
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/29.png" width="600" />  <hr />
29. 最后成功后可以在下图所示位置看到自己的设备，就可以开始真机测试了
<img src="https://raw.githubusercontent.com/arkulo56/arkulo56.github.com/master/images/swift/30.png" width="600" />  <hr />
30. 点击运行，会提示你想使用该设备必须输入开发者账号和密码，别填错了，也别忘了把手机和电脑连上，到此您就可以看到手机上的项目运行效果了，恭喜！！