# 后端处理部分

##  后端向 stripe 服务器发起请求
> 本文 Postman 请求，来走支付流程

### 请求头 Authorization 构造
 `Authorization` : `Basic base64("API密钥，请在 Stripe 后台查看")`

### 发起请求
```
curl --location --request POST 'https://api.stripe.com/v1/checkout/sessions' \
--header 'Authorization: Basic base64的结果' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'success_url=https://example.com/success' \
--data-urlencode 'cancel_url=https://example.com/cancel' \
--data-urlencode 'line_items[0][amount]=1000' \
--data-urlencode 'line_items[0][quantity]=1' \
--data-urlencode 'mode=payment' \
--data-urlencode 'line_items[0][name]=商品名' \
--data-urlencode 'line_items[0][currency]=cny'
```
###  响应的结果
```
{
    "id": "cs_test_a1ZwXXv5oZc2VcWAe6TyNu25Mk6GA4sglL6zzM5ogikPvcsqMWwGJLXmkk",
    "object": "checkout.session",
    "after_expiration": null,
    "allow_promotion_codes": null,
    "amount_subtotal": 1000,
    "amount_total": 1000,
    "automatic_tax": {
        "enabled": false,
        "status": null
    },
    "billing_address_collection": null,
    "cancel_url": "https://example.com/cancel",
    "client_reference_id": null,
    "consent": null,
    "consent_collection": null,
    "currency": "cny",
    "customer": null,
    "customer_creation": "always",
    "customer_details": null,
    "customer_email": null,
    "display_items": [
        {
            "object": "checkout.session.custom_display_item",
            "amount": 1000,
            "currency": "cny",
            "custom": {
                "description": null,
                "images": null,
                "name": "商品名"
            },
            "quantity": 1,
            "type": "custom"
        }
    ],
    "expires_at": 1643551744,
    "livemode": false,
    "locale": null,
    "metadata": {},
    "mode": "payment",
    "payment_intent": "pi_3KNHbw2eZvKYlo2C1XYaGU9E",
    "payment_link": null,
    "payment_method_options": {},
    "payment_method_types": [
        "card"
    ],
    "payment_status": "unpaid",
    "phone_number_collection": {
        "enabled": false
    },
    "recovered_from": null,
    "setup_intent": null,
    "shipping": null,
    "shipping_address_collection": null,
    "shipping_options": [],
    "shipping_rate": null,
    "status": "open",
    "submit_type": null,
    "subscription": null,
    "success_url": "https://example.com/success",
    "total_details": {
        "amount_discount": 0,
        "amount_shipping": 0,
        "amount_tax": 0
    },
    "url": "https://checkout.stripe.com/pay/cs_test_a1ZwXXv5oZc2VcWAe6TyNu25Mk6GA4sglL6zzM5ogikPvcsqMWwGJLXmkk#fidkdWxOYHwnPyd1blpxYHZxWlFcampIVGRwc2FAQXQwMUtsUXVtTDJvfScpJ2N3amhWYHdzYHcnP3F3cGApJ2lkfGpwcVF8dWAnPyd2bGtiaWBabHFgaCcpJ2BrZGdpYFVpZGZgbWppYWB3dic%2FcXdwYHgl" // 前端支付调整的 url
}
```
## 异常情况
商品售价不能太低，至少需50美分，低于50美分的情况下，会提示如下错误
```
{
    "error": {
        "code": "amount_too_small",
        "doc_url": "https://stripe.com/docs/error-codes/amount-too-small",
        "message": "The Checkout Session's total amount must convert to at least 50 cents. ¥3.00 converts to approximately $0.47.",
        "type": "invalid_request_error"
    }
}
```

# 前端部分
## 前端渲染支付 url
前端可以拿接口返回的 url，直接打开，
![image.png](https://s3.bmp.ovh/imgs/2022/02/3a3d80e178eeda5e.png)

我们是用 test 的密钥，所以最好使用官方提供的模拟信用卡来测试，不要使用真实的信用卡来测试： [https://stripe.com/docs/testing](https://stripe.com/docs/testing)

## 支付成功或者失败
stripe 会根据实际的支付结果调整之前前端传的 `success_url`, `cancel_url`

# 异步通知部分
Webhook 可配置支付事件的回调，我们只要在后台配置好地址，在对应的地址处理回调即可。如下图所示:
![image.png](https://s3.bmp.ovh/imgs/2022/02/a2d9941304d7b794.png)
为了方便本地方便测试回调，我们也可以添加本地监听器

## 回调内容
```
[{"id":"evt_1KNrwvI5m43bf5dBjeGkCUHc","object":"event","api_version":"2020-08-27","created":1643605029,"data":{"object":{"id":"cs_test_a1seVuGqGFung8BjGjdU3idZ0qQSAVLqSQ2rBP3sClGvCW3FBoVo8O2UvX","object":"checkout.session","amount_subtotal":1000,"amount_total":1000,"automatic_tax":{"enabled":false},"cancel_url":"https://example.com/cancel","currency":"cny","customer":"cus_L3zwiaUziu3CC2","customer_creation":"always","customer_details":{"email":"mimeay@163.com","tax_exempt":"none","tax_ids":[]},"expires_at":1643691387,"livemode":false,"metadata":{},"mode":"payment","payment_intent":"pi_3KNrwFI5m43bf5dB0qwHcAMs","payment_method_options":{},"payment_method_types":["card"],"payment_status":"paid","phone_number_collection":{"enabled":false},"shipping_options":[],"status":"complete","success_url":"https://example.com/success","total_details":{"amount_discount":0,"amount_shipping":0,"amount_tax":0}}},"livemode":false,"pending_webhooks":2,"request":{},"type":"checkout.session.completed"}]
```
## 本地监听
1. 安装 `cli`
```
brew install stripe/stripe-cli/stripe
```
2. 登录
![image.png](https://s3.bmp.ovh/imgs/2022/02/fddef858a638f8c4.png)


3. 按enter 打开网页, 点击运行访问完成登录验证
![image.png](https://s3.bmp.ovh/imgs/2022/02/e04d9991a6844434.png)

4. 监听
```
stripe listen --forward-to localhost:9103/pay/notification/stripe
```

5. 重新下单，完成支付，本地就可以接收到支付回调事件啦
```
{"id":"evt_1KNtZqI5m43bf5dBoHExTWXE","object":"event","api_version":"2020-08-27","created":1643611286,"data":{"object":{"id":"cs_test_a1ocpFVybu8XP7AKf5Evviea9SbqVlCgvjqfKNOiG9dO9HHokduhjnGj7g","object":"checkout.session","after_expiration":null,"allow_promotion_codes":null,"amount_subtotal":1000,"amount_total":1000,"automatic_tax":{"enabled":false,"status":null},"billing_address_collection":null,"cancel_url":"https://example.com/cancel","client_reference_id":null,"consent":null,"consent_collection":null,"currency":"cny","customer":"cus_L41dtiSBXFhtHB","customer_creation":"always","customer_details":{"email":"xxxxx","phone":null,"tax_exempt":"none","tax_ids":[]},"customer_email":null,"expires_at":1643697650,"livemode":false,"locale":null,"metadata":{},"mode":"payment","payment_intent":"pi_3KNtZGI5m43bf5dB1EP64V9k","payment_link":null,"payment_method_options":{},"payment_method_types":["card"],"payment_status":"paid","phone_number_collection":{"enabled":false},"recovered_from":null,"setup_intent":null,"shipping":null,"shipping_address_collection":null,"shipping_options":[],"shipping_rate":null,"status":"complete","submit_type":null,"subscription":null,"success_url":"https://example.com/success","total_details":{"amount_discount":0,"amount_shipping":0,"amount_tax":0},"url":null}},"livemode":false,"pending_webhooks":3,"request":{"id":null,"idempotency_key":null},"type":"checkout.session.completed"}
```
# 参考文章
[Stripe 快速开始](https://stripe.com/docs/payments/quickstart)
[创建一个 session](https://stripe.com/docs/api/checkout/sessions/create)
[Stripe支付介绍和开发对接，图文加代码
](https://ityouzi.com/archives/stripe-pay-code.html)