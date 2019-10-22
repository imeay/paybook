如 [微信支付文档](https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=4_3)描述:

1. 第一步，设所有发送或者接收到的数据为集合M，将集合M内非空参数值的参数按照参数名ASCII码从小到大排序（字典序），使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串stringA。
2. 在stringA最后拼接上key得到stringSignTemp字符串，并对stringSignTemp进行MD5运算，再将得到的字符串所有字符转换为大写，得到sign值signValue



# 易错点

* 注意字段拼写

  * 如 appid ，非 app_id

* 签名类型

  * sign_type 默认md5,  支持的签名类型包括 HMAC-SHA256 和 MD5

* 金额

  * 微信要求传的是分，如果服务定的是单位是元，请求微信接口时，切记先转化为分

* 回调地址

  * 要求外网可访问

* 特殊字段

  * openid:  JASAPI 要求必传
  * scene_info: H5 要求必传

  

# 签名工具

 开发过程，如遇到签名错误等情况，可利用微信的签名工具进行排查， 验证地址: [https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=20_1](https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=20_1)