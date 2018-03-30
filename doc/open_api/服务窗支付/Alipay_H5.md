

#    



# 注意事项

- 此接口只能在支付宝APP内使用
- 所有涉及到金额的单位都是分，最小的单位是1分，不能有小数出现
- notify_url是平台服务器从后台直接发起请求到商户服务器，商户处理时不能检查用户的cookie或session；商户更新DB等发货流程需要在notify_url完       成后，以确保掉单时，平台补单能成功补上
- notify_url有可能重复通知，商户需要做去重处理，避免多次发货
- notify_url收到的通知，商户处理成功或检查订单已经处理，需要返回处理成功的标志纯字符串success，字符串success不区分大小写；如果我们没         有收到返回的success，我方服务器继续向你们发送通知，在三小时后将不再通知；假设所有订单都没有返回 success，将增加我们服务器的通知负         载，最坏的情况，可能就是导致正常通知商户的通知发生延迟；另外我们会催促你们完善，如果长期不改善，研发或运维技术将对贵司开通的支付接       口采取控制措施。
- 其它注意事项

**参数大小写问题**
请留意文档中要求的字符大小写问题， 如 “md5 运算后， 字符串的字符要转换为大写” 。

**参数格式问题**
所有传入参数，均为字符串类型，请注意文档中各处的具体要求。

**时间戳问题**
请使用 Linux 时间戳，注意为字符串格式。

**同一商户订单号支付问题**
商户订单支付失败需要生成新单号重新发起支付，要对原订单号调用关单，避免重复支付；系统下单后，用户支付超时，系统退出     不再受理，避免用户继续，请调用关单接口。
注意：订单生成后不能马上调用关单接口，最短调用时间间隔为5分钟。



# 测试账户

请联系运营人员获取
	

# 接口规则
 
## 协议规定

规则|说明
---|---
传输方式|为保证交易安全性，建议采用HTTPS传输
提交方式|	采用POST方法提交
数据格式|	提交和返回数据都为JSON格式
字符编码|	统一采用UTF-8字符编码
签名算法|	MD5，后续会兼容SHA1、SHA256、HMAC等。
签名要求|	请求和接收数据均需要校验签名，详细方法请参考安全规范-[签名算法](/qianming)
判断逻辑|	先判断协议字段返回，再判断业务返回，最后判断交易状态

## 安全规范

安全规范-[签名算法](/qianming)
## 数据格式

**提交数据**
采用HTTPS标准的POST协议，为了保证接收方接收数据正确,传输数据必须签名。

```
mid=100000510983456&noise=RRE1235W&orderNo=M201611101010100002&subject=iphone7&body=iphone7&amount=5230.00&type=wechat&notifyUrl=http://192.168.1.123:8090/leopard/notify&callbackUrl=http://192.168.1.123:8090/leopard/success&openid=qw123352ert2&sign=567A9D86BD90C09567A9D86BD90C098C
```

**返回数据**

```json
{
  "code":"SUCCESS",
  "msg":"ok",
  ...
  "sign":"567A9D86BD90C09567A9D86BD90C098C "
}
```

返回有code参数，SUCCESS表示调用成功；非SUCCESS表示调用失败。


# API列表
 

## 下单请求

地址:[环境](/tech)/pay/jspay/alipay


参数|说明|类型|长度|必须|备注
---|---|---|---|---|---
mid|商户号|string|15|Y|商户号
noise|随机字符串|string|32|Y|随机字符串，不长于32位
orderNo|商户订单号|string|32|Y|商户系统中的订单号
subject|订单标题|string|128|Y|可放置商品名称
body|订单描述|string|128|Y|商品的简要描述
amount|订单金额|number|18,2|Y|##.##  (圆.角分)
notifyUrl|通知URL|string|256|Y|接收支付通知的url
buyerId|买家支付宝用户ID|string|128|N|当本参数不为空时,优先使用本参数
buyerLogonId|买家支付宝账号|string|128|N|当buyerId不存在时,使用本参数
type|支付类型|string|32|Y|alipay
sign|签名|string|128|Y|见 [数字签名规则](/qianming)

请求示例:
```json
{
    amount=0.03,
    orderNo=1502700XXXXXXXX,
    subject=subject,
    sign=50FE3DE0D933E6F691FXXXXXXXXXXXX,
    mid=8120170XXXXXXXXX,
    notifyUrl=http://localhosXXXXXXXXX/notify,
    body=test,
    buyerLogonId=47XXXXXXXX.com,
	type=alipay,
    noise=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
}
```
响应参数：


参数|说明|类型|长度|必须|备注
---|---|---|---|---|---
code|返回码|string|32|Y|SUCCESS/其他;如非SUCCESS见[错误码](/error_code),交易是否成功需要查看 resultCode来判断
msg|返回消息|string|64|Y|错误信息描述
code为SUCCESS时返回|||||
resultCode|返回状态码|string|32|Y|如非SUCCESS 参见 [返回状态码](/error_code)
errCode|错误代码|string|32|N|
errCodeDes|错误代码描述|string|128|N|
noise|随机字符串|string|32|Y|随机字符串，不长于32位
mid|商户号|string|15|Y|商户号
sign|签名|string|128|Y|[数字签名规则](/qianming)
resultCode为SUCCESS时返回|||||
orderNo|商户订单号|string|32|Y|
amount|订单金额|number|18,2|Y|
payInfo|原生链接|string|1024|Y|原生手机网站支付链接,JSON字符串，自行唤起支付宝钱包支付
payUrl|非原生链接|string|1024|N|非原生手机网站支付链接,直接使用此链接请求支付宝支付
响应示例:

```
{
    "msg": "ok",
    "amount": 0.03,
    "code": "SUCCESS",
    "orderNo": "1502699XXXXXXX",
    "noise": "65nlxXXXXXXXXX4LEMUxPWzcit5o8si",
    "resultCode": "SUCCESS",
    "sign": "4XXXXXXXXXXX8BFE18BE891C45225",
    "mid": "812017061XXXXXXXX",
    "payUrl": "https://pay.swiftpass.cn/pay/prepay?token_id=2fec6e80a4b1a7729XXXXXXXXecb9f6&trade_type=XXXXXXXXXXXX",
    "payInfo": "{\"tradeNO\":\"20170814210010XXXXXX36099\",\"status\":\"0\"}"
}
```


## 支付通知
 见[支付结果通知](/支付通知/notify)

## 支付查询
见 [订单查询](/订单查询/queryOrder)

# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)



