# 准备工作

没，直接调用 sdk 提供的方法就完事了



# 需要注意的点

* 支付宝退款默认只能退3个月内的单，如希望延长时间，需联系支付宝的运营

* 对外退款id (out_request_no) 需要唯一， UUID 生成即可

# 退款方法

直接见文档就可以了 https://opendocs.alipay.com/apis/api_1/alipay.trade.refund



# 退款回调？

* 支付宝是没有退款回调的，调用退款接口会直接响应退款结果，接入方需要根据退款结果处理相关退款单
* 可主动查询退款结果： https://opendocs.alipay.com/apis/api_1/alipay.trade.fastpay.refund.query

