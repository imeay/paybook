其实可以一开始就直接讲支付相关的，但在这里还是先说一下接口设计， 接下来涉及到具体支付方式的时候，也会基于接口设计举例。



# 接口设计

## 通用的支付接口

当跳转到支付页面时,  对于支付服务来说，主要就2个参数, 接口设计大概如下

假设`backend_domain` 为支付服务的后端地址

```
url: ${backend_domain}/pay/unified_pay
method: post
body: {
  "payment_type": Enum // eg: ALI_WEB, ALI_H5, WX_NATIVE ... and so on
  "order_id": String
}
response: {
   payment_type: Enum,
   extra: Any // 这里会根据不同的支付类型，返回不同的值，后面的文章会更细节说明
}
```

这样设计的一个好处在于，前端并不需要因为接入多种支付方式，而接不同的接口，设计成一个接口的话，他们要改的就主要是支付方式的枚举值

##通用的支付接口还是得做一些让步

眼尖或者已经接过微信支付的同事就会发现，是不是少了一些参数？ 是的，的确是少了一些。

如 微信 H5 支付是需要传 scense_info 参数的,   JASAPI 支付 则要求必传 openid,  那上面的接口还得再改改

```
url: ${backend_domain}/pay/unified_pay
method: post
body: {
  "payment_type": Enum, // eg: ALI_WEB, ALI_H5, WX_NATIVE ... and so on
  "order_id": String,
  "openid": String?, // optional
  "scense_info": String?, optional
}
response: {
   payment_type: Enum,
   extra: Any
}
```

这样子基本上就能满足大部分的支付需求啦。

## 还需要再调整？

是，如支付宝网页版支付，是会跳到支付宝自身的页面的，支付完成后如何跳转回原有业务就是一个问题，支付宝提供了一个 return_url 的参数，我们把这个也作为我们的一个请求参数

```
url: ${backend_domain}/pay/unified_pay
method: post
body: {
  "payment_type": Enum, // eg: ALI_WEB, ALI_H5, WX_NATIVE ... and so on
  "order_id": String,
  "openid": String?, // optional
  "scense_info": String?, optional
  "return_url": String?
}
response: {
   payment_type: Enum,
   extra: Any
}
```

到此，接口及参数基本上就满足支付宝支付，微信支付了。