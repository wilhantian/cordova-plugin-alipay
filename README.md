## cordova-plugin-wilhantian-alipay ##
> 此插件修改自[offbye的支付宝插件](https://github.com/offbye/cordova-plugin-alipay)
> 感谢`offbye`对开源软件的贡献

Makes your Cordova application enable to use the [Alipay SDK](https://doc.open.alipay.com/docs/doc.htm?spm=a219a.7629140.0.0.hT44dE&treeId=54&articleId=104509&docType=1)
for mobile payment with Alipay App or Mobile Web. Requires cordova-android 4.0 or greater.

### ChangeLogs
 > 本cordova插件是基于支付宝App支付SDK的Demo实现

+ 1.0.4:
 - 简化插件调用方式. 原`cordova.plugins.AliPay.pay()`改为`AliPay.pay()` 

+ 1.0.2: 
 - 修正`IOS`平台下resource资源引入错误问题

+ 1.0.1(`offbye`版本): 
 - 升级支付宝SDK版本到20160825；
 - 修改了一些bug;
 - 支持Android和iOS Alipay SDK
###主要功能

 - 主要功能是：服务器把订单信息签名后，调用该插件调用支付宝sdk进行支付，支付完成后如支付成功，如果是9000状态，还要去服务端去验证是否真正支付

### Install 安装

The following directions are for cordova-cli (most people).  

* Open an existing cordova project, with cordova-android 4.0.0+, and using the latest CLI. TBS X5  variables can be configured as an option when installing the plugin
* Add this plugin

  ```sh
  cordova plugin add https://github.com/wilhantian/cordova-plugin-alipay.git --variable PARTNER_ID=[你的商户PID可以在账户中查询]
  ```
  （对于android，可以不传PARTNER_ID）

   offline：下载后再进行安装 `cordova plugin add YOUR_DIR`

### 支持平台
- Android 
- IOS

### API

* js调用插件方法

```javascript

    //第一步：订单在服务端签名生成订单信息，具体请参考官网进行签名处理
    var payInfo  = "xxxx";

    //第二步：调用支付插件        	
    plugins.AliPay.pay(payInfo,function success(e){
        var status = e.resultStatus;
        //TODO
    },function error(e){});

    //e.resultStatus  状态代码  e.result  本次操作返回的结果数据 e.memo 提示信息
    //e.resultStatus  9000  订单支付成功 ;8000 正在处理中  调用function success
    //e.resultStatus  4000  订单支付失败 ;6001  用户中途取消 ;6002 网络连接出错  调用function error
    //当e.resultStatus为9000时，请去服务端验证支付结果

    //第三步：验证支付正确性（服务器接受支付宝回调 + 客户端主动轮询）
```
注意: 当e.resultStatus为9000时，请去服务端验证支付结果. [参考资料](https://doc.open.alipay.com/doc2/detail.htm?spm=0.0.0.0.xdvAU6&treeId=59&articleId=103665&docType=1)

> ！！！注意！！！2016年11月后支付宝更新了支付SDK和web控制台
>> 开发者需要到`账户中心-PID和公钥管理-mapi网关产品秘钥`中设置RSA公钥
>>> 直接在`开发者中心-应用环境`中设置的RSA公钥无法与此插件SDK版本对应!!!
