>  不管接多少种支付，如何合理的处理好支付业务，才是重中之重！



# 考虑支付状态的不可逆性

* 正常的流转
  * 待支付 -> 支付关闭
  * 待支付 -> 支付完成
* 不正常的流程
  * 关闭 -> 支付完成
  * 支付完成 -> 关闭

也就是在【支付关闭】后，就不存在状态流转为【已完成】， 反之也是一样



# 业务解耦

在一个订单系统里面，支付的发起，肯定离不开订单。 

思考一个问题, 订单如何获取支付状态...

...

...

...

## 解耦方式一 ：主动轮询

发起支付后，订单服务【通过订单号或者付款单号】请求支付服务，轮询获取支付状态



## 解耦方式二： 支付服务发送MQ， 订单服务订阅消费

* 支付服务发送MQ，考虑发送失败的可能性，同时应该方案，如错误记录，补发
* 订单服务订阅并消费，应考虑消费失败的可能性，这个时候应重试消费



个人更倾向于解耦方式二，即通过MQ的方式