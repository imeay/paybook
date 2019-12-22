# 内网穿透

为了便于本地测试一些第三方服务，如微信支付，支付宝支付， 学会内网穿透工具就变得很重要了！！！

# 客户端下载地址

[http://www.ngrok.cc/download.html](http://www.ngrok.cc/download.html)

# 使用教程
按照步骤，选择 ngrok ，可体验免费版后再考虑使用收费版本， 得到 隧道id。

一般会要求配置以下几点:
* 访问域名
* 绑定端口（最后和需要内网穿透的服务一样，可随时更改）

在最后一般可被分配到类似 `****.idcfengye.com` 的域名，这也是我们实现内网穿透最重要的域名

## 改变文件权限
下载下来的 ./sunny 文件默认是没有执行权限的，根据需要可分配权限，为了方便，我一般都是给 777 权限

```
chmod 777 ./sunny
```

## 启动

eg： `./sunny clientid $client_id`

> client_id 就是上面申请得到的隧道id

```
./sunny clientid d2002d324234234dcb709fd 
```

# 官方文档
[sunny-ngrok 之开通隧道](http://www.ngrok.cc/_book/general/open.html)

[sunny-ngrok 之客户端使用](http://www.ngrok.cc/_book/start/ngrok_linux.htm])



# 结合微信或者支付宝支付

## 设置支付宝或者微信的回调域名

如支付宝发起支付时回调地址可设置 http://xxxxx.free.idcfengye.com/alipay/callback

微信发起支付时回调地址则设置 http://xxxxx.free.idcfengye.com/wxpay/callback

支付完成后，第三方支付会访问发起支付时设置的回调地址，并携带了支付成功后的参数，业务则回调第三方支付的回调参数做业务处理即可。

