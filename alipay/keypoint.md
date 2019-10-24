

支付开发指导：https://docs.open.alipay.com/270/10589

# 从官方文档提取关键点

## sdk 只需初始化一次

> 以 kotlin 代码为例,  只需要初始化一次, 后续调用不同的api都用同个实例

```
val alipayClient = DefaultAlipayClient(URL,APP_ID,APP_PRIVATE_KEY,FORMAT,CHARSET,ALIPAY_PUBLIC_KEY,SIGN_TYPE);
```



## pageExecute and execute

pageExecute 返回的会是表单, execute 返回是一串参数

## 业务回调地址

支付宝传 return_url,  也就是发起前端支付的时候，可以把期望支付后跳转的地址传给后端发起支付，在用户完成支付后，就可以跳转到期望的地址了

# 调试

在可以说明一下调试的细节，以电脑端网站支付为例。在调用支付宝电脑网址支付接口后，会返回了以下 html（安全性考虑，我对针对部分关键信息进行了混淆）

```
<form name=\"punchout_form\" method=\"post\" action=\"https://openapi.alipay.com/gateway.do?charset=UTF-8&method=alipay.trade.page.pay&sign=pGjynJNMsoxxxxxxxxxxxx2FavP3cypq10T%2FAE5lxI3%2FK0e8XF19NK0yh%2Bb%2FjQpv9foFk54530UI3na%2Fp4Qz54FUCC0%2BbldMx3Ei2D259vVg2WVCuNm0gFe43qP%2FGwWreC0CL1l3UG30tBzFngWYkNSany%2BQt15%2FYw09KYTm3EnnVKmbRlZZYrPmFQleyD2Dpr5ZUk3u1MSdeT1FHZ14IPMTyP7Yw3rMLDHqP7fPLvBbk%2BEwVbdK9oxVcCmTQTNOKYg%3D%3D&return_url=https%3A%2F%2Fbaidu.com&notify_url=https%3A%2F%2Fdev-api-ezbuy.xxxxxxxxxxfication%2Falipay&version=1.0&app_id=20weerwer68&sign_type=RSA2&timestamp=2019-10-23+10%3A46%3A39&alipay_sdk=alipay-sdk-java-3.7.110.ALL&format=JSON\">\n<input type=\"hidden\" name=\"biz_content\" value=\"{&quot;out_trade_no&quot;:&quot;56b86313f53f11e9b5d791bce55fb321&quot;,&quot;product_code&quot;:&quot;FAST_INSTANT_TRADE_PAY&quot;,&quot;subject&quot;:&quot;编程猫商城&quot;,&quot;total_amount&quot;:&quot;0.01&quot;}\">\n<input type=\"submit\" value=\"立即支付\" style=\"display:none\" >\n</form>\n<script>document.forms[0].submit();</script>
```

在前端还没介入联调时，后端自测的时候，可以将以上代码在 http://www.jsons.cn/htmldebug/ 执行，执行后，就会跳到支付宝的支付页面。

