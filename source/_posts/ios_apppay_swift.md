title: App支付iOS_Swift版本(客户端) 
date: 2016-11-29 10:24:14
categories: 支付
tags: [swift,ios,pay,支付宝]
description: swift、pay

---

Swift集成支付宝，一些细节记录

<!--more-->
### 支付宝官方文档集成
[App支付iOS集成流程详解](https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7629140.0.0.5cfNaI&treeId=193&articleId=105295&docType=1)

步骤1：启动IDE（如Xcode），把iOS包中的压缩文件中以下文件拷贝到项目文件夹下，并导入到项目工程中。

	AlipaySDK.bundle
	AlipaySDK.framework
在Build Phases选项卡的Link Binary With Libraries中，增加以下依赖：

![官方图](https://img.alicdn.com/top/i1/LB1PlBHKpXXXXXoXXXXXXXXXXXX)

其中，需要注意的是：

如果是Xcode 7.0之后的版本，需要添加libc++.tbd、libz.tbd；
如果是Xcode 7.0之前的版本，需要添加libc++.dylib、libz.dylib（如下图）。
![官方图](https://img.alicdn.com/top/i1/LB1ublXKpXXXXXBaXXXXXXXXXXX)
步骤2：在需要调用AlipaySDK的文件中，增加头文件引用。

	#import <AlipaySDK/AlipaySDK.h>

### AppDelegate.Swift处理
在AppDelegate.Swift添加支付宝回调处理

   	//app走application(ios9)里的这个回调，网页直接走viewcontroller里面的回调
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool{
        //跳转支付宝钱包进行支付，处理支付结果
        AlipaySDK.defaultService().processOrder(withPaymentResult: url, standbyCallback: { resultDict in
            dealResultAliPay(resultDic: resultDict)
        })
        return true
    
    }
    
    //app走application(ios8及其以下)里的这个回调，网页直接走viewcontroller里面的回调
    func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool{
        //跳转支付宝钱包进行支付，处理支付结果
        AlipaySDK.defaultService().processOrder(withPaymentResult: url, standbyCallback: {
            resultDict in
            dealResultAliPay(resultDic: resultDict)
        })
    
        return true
    }
关于dealResultAliPay你自己可以写成公公共方法，方便后续调用。

	func dealResultAliPay(resultDic:[AnyHashable : Any]?){
        if let resultStatus = resultDic?["resultStatus"]! {
            if let statusCode = resultStatus as? String{
                var msg = "支付失败,请重新支付！"
                switch statusCode {
                case "9000":
                    msg = "支付成功！"
                    //显示成功信息，更新具体数据，返回订单记录等等处理
                    return
                case "8000":
                    msg = "订单正在处理中，请留意订单记录列表!"
                case "4000":
                    msg = "支付失败,请重新购买套餐!"
                case "5000":
                    msg = "请勿重新请求!"
                case "6001":
                    
                    break
                //用户取消订单
                case "6002":
                    msg = "网络连接错误"
                default:
                    print(statusCode)
                }
               showMsg(msg)
            }
        }
    }


### 具体订单处理(订单是在服务端封装好的)

   	let id = packageInfoModels[(selectedIndex! as NSIndexPath).row].id
    //根据id获取后台封装好的订单套餐，发送给支付宝服务器
        WBNetworkTool.shareNetworkTool.goAlipay(id!){
            (responseMsgModel) in
            let orderString = responseMsgModel.data
            print(orderString)
            let scheme:String = "alisdkdemo" //回调的标识符
            //调用网页(没装支付宝app)会走这个回调，如果装了支付宝app，走AppDelegate里面的回调
            AlipaySDK.defaultService().payOrder(orderString, fromScheme: scheme, callback: {
                    resultDic in
                    //调用上面的支付处理
                    dealResultAliPay(resultDic: resultDic)
                })
        }

### 处理scheme，回调标识符
   点击项目名称，点击“Info”选项卡，在“URL Types”选项中，点击“+”，在“URL Schemes”中输入“alisdkdemo(名字可以随意，一般唯一表示当前项目)” 跟AlipaySDK.defaultService().payOrder里的一致。这样才能保证回调正确。
   
	注意：这里的URL Schemes中输入的alisdkdemo，为测试demo，实际商户的app中要填写独立的scheme，建议跟商户的app有一定的标示度，要做到和其他的商户app不重复，否则可能会导致支付宝返回的结果无法正确跳回商户app。
![Schemes](/images/8DCDF14A-1727-4502-9B31-650D9BDE336A.png)
	
### 开源让世界更美好   
### 感谢
[蚂蚁金服开放平台](https://doc.open.alipay.com/)
