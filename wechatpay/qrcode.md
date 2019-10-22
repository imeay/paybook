

扫码支付应该在所有微信支付方式中应该是属于最简单，最容易接入的了。

理由是:

* 不用设置支付安全域名或者目录
* 统一下单的接口返回参数转化为二维码后，微信扫描即可在微信内触发起支付页



以下支付字段为统一下单时，扫码支付必须含有的参数

```
{
   trade_type: 'NATIVE', // 支付类型
   appid: string,
   mchd_id: string,
   nonce_str: string, // 随机生成即可
   sign_type: string, // HMAC-SHA256 或者 md5
   body: string, // 商品描述
   out_trade_no: string // 对外的商户订单号, 一般用 uuid 生成即可，但是长度要满足微信方的要求
   total_fee: int // 支付金额，单位为分
   spbill_create_ip: string // 用户发起支付的客户端ip
   notify_url: string // 回调地址，业务服务接收微信支付结果的地址
}
```



扫码支付返回的结果

```
code_url: string // 值结果如右: weixin://wxpay/bizpayurl/up?pr=NwY5Mz9&groupid=00
```

