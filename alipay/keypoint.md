





# 从官方文档提取关键点

## sdk 只需初始化一次

> 以 kotlin 代码为例,  只需要初始化一次, 后续调用不同的api都用同个实例

```
val alipayClient = DefaultAlipayClient(URL,APP_ID,APP_PRIVATE_KEY,FORMAT,CHARSET,ALIPAY_PUBLIC_KEY,SIGN_TYPE);
```



## pageExecute and execute

pageExecute 返回的会是表单, execute 返回是一串参数



## 支付完成后前端回跳地址

支付宝传 return_url,  也就是发起前端支付的时候，可以把期望支付后跳转的地址传给后端发起支付，在用户完成支付后，就可以跳转到期望的地址了



# 异步通知

支付宝还会根据原始支付API中传入的异步通知地址notify_url，通过POST请求的形式将支付结果作为参数通知到商户系统



# 官方建议

- **由于前台回跳的不可靠性，前台回跳只能作为商户支付结果页的入口，最终支付结果必须以异步通知或查询接口返回为准，不能依赖前台回跳。**
- **商户系统接收到异步通知以后，必须通过验签（验证通知中的sign参数）来确保支付通知是由支付宝发送的。详细验签规则参考**[**异步通知验签**](https://docs.open.alipay.com/203/105286#s6)**。**
- **接受到异步通知并验签通过后，一定要检查通知内容，包括通知中的app_id, out_trade_no, total_amount是否与请求中的一致，并根据trade_status进行后续业务处理。**



# 调试

在可以说明一下调试的细节，以电脑端网站支付为例。在调用支付宝电脑网址支付接口后，会返回了以下 html（安全性考虑，我对针对部分关键信息进行了混淆）

```
<form name=\"punchout_form\" method=\"post\" action=\"https://openapi.alipay.com/gateway.do?charset=UTF-8&method=alipay.trade.page.pay&sign=pGjynJNMsoxxxxxxxxxxxx2FavP3cypq10T%2FAE5lxI3%2FK0e8XF19NK0yh%2Bb%2FjQpv9foFk54530UI3na%2Fp4Qz54FUCC0%2BbldMx3Ei2D259vVg2WVCuNm0gFe43qP%2FGwWreC0CL1l3UG30tBzFngWYkNSany%2BQt15%2FYw09KYTm3EnnVKmbRlZZYrPmFQleyD2Dpr5ZUk3u1MSdeT1FHZ14IPMTyP7Yw3rMLDHqP7fPLvBbk%2BEwVbdK9oxVcCmTQTNOKYg%3D%3D&return_url=https%3A%2F%2Fbaidu.com&notify_url=https%3A%2F%2Fdev-api-ezbuy.xxxxxxxxxxfication%2Falipay&version=1.0&app_id=20weerwer68&sign_type=RSA2&timestamp=2019-10-23+10%3A46%3A39&alipay_sdk=alipay-sdk-java-3.7.110.ALL&format=JSON\">\n<input type=\"hidden\" name=\"biz_content\" value=\"{&quot;out_trade_no&quot;:&quot;56b86313f53f11e9b5d791bce55fb321&quot;,&quot;product_code&quot;:&quot;FAST_INSTANT_TRADE_PAY&quot;,&quot;subject&quot;:&quot;编程猫商城&quot;,&quot;total_amount&quot;:&quot;0.01&quot;}\">\n<input type=\"submit\" value=\"立即支付\" style=\"display:none\" >\n</form>\n<script>document.forms[0].submit();</script>
```

在前端还没介入联调时，后端自测的时候，可以将以上代码在 http://www.jsons.cn/htmldebug/ 执行，执行后，就会跳到支付宝的支付页面。



# 官方文档 

其实我觉得这个支付宝关于手机网站支付的开发文档写的蛮好的。建议通读一遍后再进行开发

[https://docs.open.alipay.com/203/105285/](https://docs.open.alipay.com/203/105285/)