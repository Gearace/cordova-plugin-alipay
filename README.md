# Cordova 支付宝支付插件

fork后自用，修改为callback，不需要安装https://github.com/EddyVerbruggen/Custom-URL-scheme插件

还在开发中，待加入订单信息

## 支持的系统

* iOS
* Android

## 手动安装

1. 使用git命令将插件下载到本地，并标记为$CORDOVA_PLUGIN_DIR

``` 
	git clone https://github.com/charleyw/cordova-plugin-alipay.git && cd cordova-plugin-alipay && export CORDOVA_PLUGIN_DIR=$(pwd)
```

1. 修改$CORDOVA_PLUGIN_DIR/plugin.xml，将

``` 
	<preference name="private_key" value="$PRIVATE_KEY" />
```

改成

``` 
	<preference name="PRIVATE_KEY" value="你生成的private key"/>

**注意**：总共有两处
```

1. 安装

``` 
	cordova plugin add $CORDOVA_PLUGIN_DIR --variable PARTNER_ID=[你的商户PID可以在账户中查询] --variable SELLER_ACCOUNT=[你的商户支付宝帐号]
```

## 使用方法

``` 
window.alipay.pay({
	tradeNo: tradeNo,
	subject: "测试标题",
	body: "我是测试内容",
	price: 0.02,
	fromUrlScheme: "demoScheme://afterPaymentSuccess",
	notifyUrl: "http://your.server.notify.url"
});
```

### 参数说明

* tradeNo 这个是支付宝需要的，应该是一个唯一的ID号
* subject 这个字段会显示在支付宝付款的页面
* body 订单详情，没找到会显示哪里
* price 价格，支持两位小数
* fromUrlScheme 支付完成跳转的URL Scheme（[iOS](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html)，[Android](http://developer.android.com/training/basics/intents/filters.html)），可以使用这个[Cordova插件](https://github.com/EddyVerbruggen/Custom-URL-scheme)定义你的App的Scheme

调用`pay`方法会打开支付宝支付页面进行支付（如果有安装支付宝钱包的话会打开支付宝钱包），支付完成之后会跳回到程序，会跳到`fromUrlScheme`定义的程序页面，

如上面的例子会返回你的程序的`/afterPaymentSuccess`路径所定义的页面，并且支付结果会附加到这个url后面。你可以在程序中调试来确定该怎么处理。

## 为什么不能自动安装

正常情况下，应该是可以使用下面的命令安装的

``` 
cordova plugin add $CORDOVA_PLUGIN_DIR --variable PARTNER_ID=[你的商户PID可以在账户中查询] --variable SELLER_ACCOUNT=[你的商户支付宝帐号] --variable PRIVATE_KEY=[你生成的private key]
```

但是因为private key里面有时会有等号（=），而当前版本的cordova（v5.1.1）有一个bug，当参数中有等号（=）的时候就不能正常解析变量值了。这个bug已经在cordova的master上已经修好了，但是还没有发布。

## Liscense

© 2015 Wang Chao. This code is distributed under the MIT license.

cordova plugin add  ~/code/科多瓦插件/cordova-plugin-alipay --variable PARTNER_ID="2088111981444250" --variable SELLER_ACCOUNT="itshangcheng@163.com" --variable PRIVATE_KEY="MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBAL5TAE9+bEUGBCHjZYJEEH3vYinZDSm1SbkQe8wfgpsHbIrbeaTpBfeFA3dCbb77nk9iyEAA5o3vB1rJrse42jdiogAg+YCzcXLDOpS7nHowkpqp+A3M1EcC9PKNwsG0v6AAc8zrwPd3Nl9zGf8t2TRww4LnLAqAMxJl8CSYHuJxAgMBAAECgYBo7hHpweWnWF3G4TwByczd4bDZKZWcPRrcMT5Pl7/GAR3SoJY8WUy03ly+z5z6AneRhQCqaNSzw+jmIPN/oWaM1tV+wYsFEeWyc612LIJWjHy2CfmhKyAJD1xPC/+y0z1cWoGFwvcM/3VBU2F1DAyMgximVEMTddTssLLsYaAzwQJBAOijv6JVu/yVsUuk8EVWbmVq8r9OUZshpLbvGcjpq81fpOUSDKWAHij26o8vvmPaHmo1d3JJY1G2zOLsT8mcPrUCQQDRb31KvFI2SULU95o+1f6J3RL31/4xofCprbgP0UzdFgPWN7hdLM/2vqSyPiNVIxpV/DG8+UVQt9NQ9+RrHa5NAkANeKX5LYPEPZrVqYhsS3P7FXVXFJ7vH8Sc/z17/+P98YLn7OKklsWoU5wDjJ02xQOr3Mq86HkC21YD8fEw2IZdAkEApdzDvzJRcYinkv3cfDMBeLFKWloGh8wWSmq3wF8jnlvXAgnyyme481KcIEUxujUooDwwL9bB3GEYy6DmlyZUaQJBAKaWWnCIUMZDR6IbWJ4I8hNU+3ZbD2WtfZVMyHwZ4PXaNvnIN/zXBmvRT4xcNyOpid1ut2rXuNs+3JmS4m8GL0k="