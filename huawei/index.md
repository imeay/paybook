



# 官方建议的开发流程
![image.png](https://upload-images.jianshu.io/upload_images/2058787-86ff405c35779a50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 华为支付流程图
![image.png](https://upload-images.jianshu.io/upload_images/2058787-57c0b5400ba1e6b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620)

# 验证支付结果相关官方文档
* 客户端模式获取 access_token
[https://developer.huawei.com/consumer/cn/doc/development/HMS-Guides/30101#h2-3-2-](https://developer.huawei.com/consumer/cn/doc/development/HMS-Guides/30101#h2-3-2-)
* Order服务购买Token校验
https://developer.huawei.com/consumer/cn/doc/development/HMS-References/iap-api-order-service-purchase-token-verification-v4

# 接入
## 构造请求的鉴权
// xxxxx, yyyyy 请替换为实际的 client_id, client_secret
```
curl --location --request POST 'https://oauth-login.cloud.huawei.com/oauth2/v3/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id=xxxxx' \
--data-urlencode 'client_secret=yyyyy'
```
响应的结果
```
{
    "access_token": "zzzzzzzzzzzzzzzzzzzzzzzzzzz",
    "expires_in": 3600,
    "token_type": "Bearer"
}
```

## Order服务购买Token校验
* URL：[https://orders-drcn.iap.hicloud.com](https://orders-drcn.iap.hicloud.com)[/applications/purchases/tokens/verify](https://orders-at-dre.iap.dbankcloud.com/applications/purchases/tokens/verify)
* Method: POST
* Header: Authorization="Basic Base64(APPAT:accessToken)"

## curl 请求
```
curl --location --request POST 'https://orders-drcn.iap.hicloud.com/applications/purchases/tokens/verify' \
--header 'Authorization: Basic 实际的access_token' \
--header 'Content-Type: application/json' \
--data-raw '{
    "purchaseToken":"请替换为实际的purchaseToken", 
    "productId":"test_product_2"
}'
```
## 正常情况
![image.png](https://upload-images.jianshu.io/upload_images/2058787-60b13499ae16fef4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620)

* 支付结果
```
{
    "responseCode": "0",
    "purchaseTokenData": "{\"autoRenewing\":false,\"orderId\":\"orderId\",\"packageName\":\"package\",\"applicationId\":applicationId,\"kind\":0,\"productId\":\"test_product_2\",\"productName\":\"法拉利\",\"purchaseTime\":1619073179000,\"purchaseTimeMillis\":1619073179000,\"purchaseState\":0,\"developerPayload\":\"法拉利\",\"purchaseToken\":\"purchaseToken\",\"responseCode\":\"0\",\"consumptionState\":1,\"confirmed\":1,\"purchaseType\":0,\"currency\":\"CNY\",\"price\":1,\"country\":\"CN\",\"payOrderId\":\"sandboxxxxxxxxxx\",\"payType\":\"4\",\"sdkChannel\":\"1\"}",
    "dataSignature": "ssssdataSignature",
    "signatureAlgorithm": "SHA256WithRSA"
}
```
* 可以对支付结果的校验
参见：[https://developer.huawei.com/consumer/cn/doc/development/HMSCore-Guides-V5/verifying-signature-returned-result-0000001050033088-V5](https://developer.huawei.com/consumer/cn/doc/development/HMSCore-Guides-V5/verifying-signature-returned-result-0000001050033088-V5)

## token 过期的情况
![image.png](https://upload-images.jianshu.io/upload_images/2058787-a8076f3663801207.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620)

# 总结
华为支付结果校验逻辑还是比较简单的，官网文档也很清晰。