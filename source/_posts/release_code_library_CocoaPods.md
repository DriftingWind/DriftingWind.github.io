title: 发布自己的代码库到CocoaPods
date: 2016-06-24 00:14:54
categories: 支付
tags: [swift,ios,pay,支付宝]
description: swift、pay

---

整理自己的平时工作时的通用代码，方便给他人调用，所以写成pod库

<!--more-->
### 开始前的工作
安装CocoaPods，网上有很多，我就不介绍了jiezhij。


### 创建pod基本模板
使用`pod lib`的命令,然后按照步骤提示一步一步按要求操作，即可完成模板创建
   
    ➜  ~ pod lib create IAWExtensionTool
	Cloning `https://github.com/CocoaPods/pod-template.git` into `IAWExtensionTool`.
	Configuring IAWExtensionTool template.

	------------------------------

	To get you started we need to ask a few questions, this should only take a minute.

	If this is your first time we recommend running through with the guide:
 	- http://guides.cocoapods.org/making/using-pod-lib-create.html
 	( hold cmd and click links to open in a browser. )


	What language do you want to use?? [ Swift / ObjC ]
 	> Swift

	Would you like to include a demo application with your library? [ Yes / No ]
 	> Yes

	Which testing frameworks will you use? [ Quick / None ]
 	> None

	Would you like to do view based testing? [ Yes / No ]
 	> No

	Running pod install on your new library.

	[!] No `Podfile' found in the project directory.

 	Ace! you're ready to go!
 	We will start you off by opening your project in Xcode
  	open 'IAWExtensionTool/Example/IAWExtensionTool.xcworkspace'
	The file /Users/winston/IAWExtensionTool/Example/	IAWExtensionTool.xcworkspace does not exist.

	To learn more about the template see `https://github.com/CocoaPods/pod-	template.git`.
	To learn more about creating a new pod, see `http://guides.cocoapods.org/	making/making-a-cocoapod`.
 这样模板就创建完毕了  
### 修改模板文件，以方便我们XCode用workspace方式打开
进入我们刚刚初始模板的位置，就会发现IAWExtensionTool文件夹。打开文件夹，如下图：
![模板库](/images/E9AC32B3-4EF6-48AA-9006-225DB4F66B36.png)
删除Example 以及IAWExtensionTool 文件夹，这里删除，我们后面会重新创建。

打开xcode,创建IAWExtensionTool名称的`project`，保存在IAWExtensionTool文件夹下。
打开xcode,创建IAWExtensionToolDemo名称的`Cocoa Touch Framework`，保存在IAWExtensionTool文件夹下（不是IAWExtensionTool的project）。


创建Podfile文件,文件内容如下

	# Uncomment this line to define a global platform for your project
	# platform :ios, '9.0'
	source 'https://github.com/CocoaPods/Specs.git'
	platform :ios, '9.0'
	use_frameworks!
	workspace ‘IAWExtensionTool.xcworkspace'
	xcodeproj 'IAWExtensionToolDemo/IAWExtensionToolDemo.xcodeproj'
	target 'IAWExtensionToolDemo' do
  	# Comment this line if you're not using Swift and don't want to use dynamic 	frameworks
  
    	# Pods for IAWExtensionToolDemo
    	pod 'SnapKit', '~> 3.0.2’
    	pod 'Kingfisher'
    	pod 'SVProgressHUD', :git => 'https://github.com/SVProgressHUD/		SVProgressHUD.git'
    	pod "FDFullscreenPopGesture", "~> 1.1"
    	pod 'Alamofire', '~> 4.0.1'
    	pod 'SwiftyJSON', '~> 3.0.0'
    	pod 'AlamofireObjectMapper', '~> 4.0'
    	pod 'MJRefresh', '~> 3.1.12'
    	pod 'RNCryptor'
	end

	target 'IAWExtensionTool' do
 	 	platform :ios, ‘9.0’
  		xcodeproj 'IAWExtensionTool/IAWExtensionTool.xcodeproj'
  		pod 'SnapKit', '~> 3.0.2'
  		pod 'SVProgressHUD', :git => 'https://github.com/SVProgressHUD/		SVProgressHUD.git'
  		pod 'RNCryptor'
  		pod 'AlamofireObjectMapper', '~> 4.0'
  		pod 'Kingfisher'
	end

上面的文件内容，你可以不用这么多，你只要关心workspace名称，和你当前库名称一致，以及添加一个xcodeproj库，这个库，就是为了，给你写工具测试的地方。

然后切换到模板库的文件夹下执行pod install:
安装你的代码库需要的依赖库

安装完成后你的模板库下会生成IAWExtensionTool.xcworkspace，说明你的模板基本修改完毕了
### 写代码发布吧
 双击IAWExtensionTool.xcworkspace打开，XCode就以workspace方式管理的你的代码库，以及你对应的demo库，如下图：
 ![最终模板](/images/928F58BE-FDB5-4FBF-955B-014338076DAB.png) 
 
这样就方便写库与测试在一起搞定，不用来回切换XCode.

###### 修改.podspec后缀的文件
![podspec](/images/42F8F7B7-8040-401D-92E4-2F967123C785.png) 

红色框里都是需要注意的，记住上面的版本和你目前github库里的releases版本要一致 。最下面的代码路径，以及资源路径，你如果需要也可以像我那样创建，如果不需要，你只要写上你目前正确的路径即可（注意code里面创建的文件夹，只是虚拟的归类管理，在本地文件内是没有的，请在项目文件夹内手动创建文件夹，然后在xcode里执行add Files To ** 加到里面去）

###### 然后提交代码到github
github 上创建IAWExtensionTool库，与本地文件夹进行同步。

	cd existing_folder
	git init
	git remote add origin https://github.com/IAskWind/IAWExtensionTool.git
	git add .
	git commit
	git push -u origin master
###### 注册CocoaPods
执行
	
	pod trunk register iaskwind@foxmail.com 'IAskWind' --description='IAskWind‘s mac'
	
然后你的邮箱会收到的标题为[CocoaPods] Confirm your session.的邮件，点击链接之后跳转页面，会看到：
You can go back to your terminal now. 说明你注册成功了。

###### 更新代码
我上面的版本是0.1.1 我现在更新我的代码库使其也变成0.1.1这样，我就可以发布cocospod上面了
修改代码，添加一些工具类，在demo里面测试这个方法。

	git add .
	git commit -m "添加**工具类"
	git status
	git pull
	git push
	git tag
	git tag '0.1.1'
	git push --tags
	
这样远程github上，就有0.1.1的releases库了

###### 本地验证 检查合法性

	pod lib lint --allow-warnings
	
###### 发布到cocospod库
 	
	pod trunk push IAWExtensionTool.podspec --allow-warnings

###### 更新本地cocospod索引库

	pod setup	
###### 查询库
	pod search DWExtensionTool
###### 更新库
	pod update	
### 备注 在Swift3.0
	在库文件下创建 .swift-version 文件，里面内容为 3.0 ，以保证swift库生效
### 其他命令
	查看个人信息：pod trunk me
	废除某个库：pod trunk deprecate **
	废除某个版本库：pod trunk delete ** 1.0.0
### 开源让世界更美好   
### 感谢
