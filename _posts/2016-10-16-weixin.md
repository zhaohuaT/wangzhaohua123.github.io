---
layout: post
title:  "php微信支付开发教程 "
date:   2016-02-15 22:14:54
categories: 微信
tags:  微信 PHP 支付
excerpt: 1、下载微信官方支付API https://mp.weixin.qq.com/paymch/readtemplate?t=mp/business/course3_tmpl&lang=zh_CN
---
1、下载微信官方支付API https://mp.weixin.qq.com/paymch/readtemplate?t=mp/business/course3_tmpl&lang=zh_CN

2、将WxPayPubHelper文件夹拷贝到wp下的ThinkPHP\Library\Vendor\WxPay 目录下

3、配置WxPay.pub.config.PHP

![](http://img.blog.csdn.net/20150924175001777)

4、配置微信公众平台

![](http://img.blog.csdn.net/20150924175325013)
5、编写支付控制器代码 
![](http://img.blog.csdn.net/20150924180008680)
6、编写支付界面代码 
![](http://img.blog.csdn.net/20150924180242721)
7、支付成功后验证订单 
![](http://img.blog.csdn.net/20150924180433112)
注意

1、在第五步，编写控制器代码时，必须Vendor（）配置WxPayPubHelper

2、注意 $jsApi = new \JsApi_pub();以及其他对象创建时的”\”一定要有。

3、刚进入方法时可以从后台获取到sport_id参数，通过code获取openid之后，sport_id参数会丢失。因为调用了配置中的支付页面地址，重新加载了此页面。所以重新在支付页面地址后面加上参数。

4、支付金额以“分”为单位，注意转换单位

5、上面描述和总金额必须闯入字符串类型。否则会报错。

6、支付成功之后，回调状态未必是正确的(官方说明)，所以验证订单号。此时可以继续处理业务逻辑。

