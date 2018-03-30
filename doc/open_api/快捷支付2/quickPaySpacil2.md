
#     




# 注意事项
```
1.所有的接口参数均使用UTF-8进行编码。
2.接口返回的参数中带有签名参数，商户应首先对返回数据进行验签，以确认返回的数据未被篡改。
3.商户报备时需提供所绑定银行卡的正确预留信息(手机号，身份证)，否则会影响正常下单。
4.调用接口时，必须指定付款方信息，只允许同名进出。
5.支付请求后无返回，支付结果会以通知形式告知(如果支付成功后通知。用户取消支付后不会通知)。
```

# 文档说明
>为商户提供快捷支付接口

# 测试账户

```bash
联系我方工作人员获取
```



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

## 业务流程
1:调用报备接口<br>
2:使用报备接口返回的商户号与商户密钥进行下单<br>
3:将返回的JSON中的html字段用浏览器加载后在跳转的银联页面完成支付<br>
4:我方平台异步通知您在下单申请时上送的notifyUrl中的通知地址
# API列表
## 商户报备

详情参考-[[商户报备](/商户报备/register)]
## 下单请求

### 下单请求
- 接口说明：后台API直联模式，异步回应；用户在商户客户端下单成功后返回银联95516页面，用户完成支付后会有异步通知，通知交易结果

地址:[环境](/tech)/quickPay/quickPayB2c

> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|				商户号|		 String|	15|		Y|	商户号|
|notifyUrl|			通知URL|		 String|	500|	Y|	针对该交易的交易状态同步通知接受URL(如不填写,则收不到通知)|
|orderNo|			外部交易号|	 String|	32|		Y|	合作伙伴交易号（确保在合作伙伴系统中唯一）|
|subject|			商品名称|	 String	|	128|		Y|	商品的标题|
|body|				商品描述|	 String	|	128|		Y|	商品的具体描述|
|amount|			交易金额|	 number|	13,2|	Y|	交易的总金额，单位为元|
|bankCardNo|	付款方银行卡号|String	|	30|		Y|	付款方银行卡号|
|cardId|	付款方身份证号|String|	18|		Y|	付款方身份证号|
|phoneNo|	付款方手机号码|String|	11|		Y|	付款方手机号码|
|bankCode|	付款方银行编码|String|	10|		Y|	付款方银行编码|
|remark|			备注	|		String|		200|	N|	备注|
|noise|				随机字符串|	String|		32|		Y|	随机字符串,不长于32位|
|sign|				签名	|		String|		200|	Y|数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名|

### 接口返回
参数|说明|类型|长度|必须|备注
---|----|----|----|----|---
code|返回码|string|32|Y|SUCCESS/其他;如非SUCCESS见[错误码](/error_code),交易是否成功需要查看 resultCode来判断
msg|返回消息|string|64|Y|错误信息描述
code为SUCCESS时返回|||||
resultCode|返回状态码|string|32|Y|如非SUCCESS 参见 [返回状态码](/error_code)
errCode|错误代码|string|32|N|
errCodeDes|错误代码描述|string|128|N|
noise|随机字符串|string|32|Y|随机字符串，不长于32位
mid|商户号|string|15|Y|商户号
orderNo|商户订单号|string|15|Y|商户上传的订单号
sign|签名|string|128|Y|[数字签名规则](/qianming)
resultCode为SUCCESS时返回|||||
flowNo||string|32|Y|平台订单号
html||string|20|Y|银联返回的95516支付页面，将字符串提取后转为HTML页面后会跳转到银联支付页面由用户完成支付

返回示例:
```json
{
    "code": "SUCCESS",
    "flowNo": "ZF20170923153753574035",
    "html": "<html><head><meta http-equiv=\"Content-Type\" content=\"text/html; charset=UTF-8\"/></head><body><form id = \"pay_form\" action=\"https://gateway.95516.com/gateway/api/frontTransReq.do\" method=\"post\" accept-charset=\"UTF-8\"><input type=\"hidden\" name=\"channelType\" id=\"channelType\" value=\"07\"/><input type=\"hidden\" name=\"acqInsCode\" id=\"acqInsCode\" value=\"48532900\"/><input type=\"hidden\" name=\"txnSubType\" id=\"txnSubType\" value=\"01\"/><input type=\"hidden\" name=\"version\" id=\"version\" value=\"5.0.0\"/><input type=\"hidden\" name=\"txnAmt\" id=\"txnAmt\" value=\"10000\"/><input type=\"hidden\" name=\"signMethod\" id=\"signMethod\" value=\"01\"/><input type=\"hidden\" name=\"backUrl\" id=\"backUrl\" value=\"https://payment.Notify.htm\"/><input type=\"hidden\" name=\"merAbbr\" id=\"merAbbr\" value=\"在线支付\"/><input type=\"hidden\" name=\"securityType\" id=\"securityType\" value=\"01\"/><input type=\"hidden\" name=\"encoding\" id=\"encoding\" value=\"UTF-8\"/><input type=\"hidden\" name=\"merCatCode\" id=\"merCatCode\" value=\"4900\"/><input type=\"hidden\" name=\"encryptCertId\" id=\"encryptCertId\" value=\"69*******98\"/><input type=\"hidden\" name=\"orderId\" id=\"orderId\" value=\"545028484\"/><input type=\"hidden\" name=\"signature\" id=\"signature\" value=\"INTyEbnluUXLeBKU6fpGwSRDjqOtnQX/P7nuwcEYRi2gy2VYdgXBQgVrgw/eGVXT7iMS8TzSEKG+u0xw4fqTyK0EjVyVnAgiSlFk56R3alxZw1xt3TUqSgX6yogXG0FoCkDrLL2HxHzzmGS9HJlIf65fR1VOhQQuO1Wr8o/tUjRikrEZptMcZu7itoTsE6YjPpZPKMFXA3l3wQgqCWWO3ibrC7TfJUaH09rL8Kvf0BzQmZnScmYuRNXqmkPYsIM3E6Ij2oQIGDJTX3XKOzr1/kNx/IZT7VGGsb5AoirSC9v6+/ZbOPvfugAoO54vrxm1WSZVHSb/Y92pc12aQUwf3w==\"/><input type=\"hidden\" name=\"txnType\" id=\"txnType\" value=\"01\"/><input type=\"hidden\" name=\"frontUrl\" id=\"frontUrl\" value=\"https://payment.handpay.cn/hpayTransGatewayWeb/b2cNotify/bankNotify.htm\"/><input type=\"hidden\" name=\"currencyCode\" id=\"currencyCode\" value=\"156\"/><input type=\"hidden\" name=\"merId\" id=\"merId\" value=\"853********5606\"/><input type=\"hidden\" name=\"reserved\" id=\"reserved\" value=\"{cardNumberLock=1}\"/><input type=\"hidden\" name=\"accNo\" id=\"accNo\" value=\"622**********071\"/><input type=\"hidden\" name=\"certId\" id=\"certId\" value=\"69*******98\"/><input type=\"hidden\" name=\"merName\" id=\"merName\" value=\"在线支付\"/><input type=\"hidden\" name=\"bizType\" id=\"bizType\" value=\"000201\"/><input type=\"hidden\" name=\"orderTimeout\" id=\"orderTimeout\" value=\"7200000\"/><input type=\"hidden\" name=\"accessType\" id=\"accessType\" value=\"1\"/><input type=\"hidden\" name=\"txnTime\" id=\"txnTime\" value=\"20170923153756\"/></form></body><script type=\"text/javascript\">document.all.pay_form.submit();</script></html>",
    "mid": "822**********77",
    "msg": "下单成功",
    "noise": "VVNjSCOZLigLhVRJkqTw9SbX7pCWuPOn",
    "orderNo": "27773856447864248773358098914931",
    "resultCode": "SUCCESS",
    "sign": "B160816E8FBB9D74EDB3603A1D14C0A5"
}

```

## 支付结果通知

见[支付结果通知](/支付通知/notify)

# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)