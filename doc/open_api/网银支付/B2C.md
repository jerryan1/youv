#    




# 注意事项
```
1.所有的接口参数均使用UTF-8进行编码。
2.接口返回的参数中带有签名参数，商户应首先对返回数据进行验签，以确认返回的数据未被篡改。
3.网银支付请求后无返回，支付结果会以通知形式告知(如果支付成功后通知。用户取消支付后不会通知)
```

# 文档说明
>为商户提供网银支付接口。

# 测试账户

```bash
商户号:812017050323777
密钥:ddbax6n4cg8qj958ytt6

测试商户号最大限额1.00元/笔
```

# 接口规则
## 协议规定

规则|说明
---|---
传输方式|为保证交易安全性，建议采用HTTPS传输
提交方式|	采用POST方法提交
数据格式|	见示例
字符编码|	统一采用UTF-8字符编码
签名算法|	MD5，后续会兼容SHA1、SHA256、HMAC等。
签名要求|	请求和接收数据均需要校验签名，详细方法请参考安全规范-[签名算法](/qianming)
判断逻辑|	先判断协议字段返回，再判断业务返回，最后判断交易状态

## 安全规范

安全规范-[签名算法](/qianming)


# 支付方式
## 业务流程
```
网银(B2C)支付接口说明：
1.商户发起网银支付请求后，用户会打开网银支付界面。
2.用户支付成功后，平台会把支付结果异步返回给商户。
```
## 场景介绍

# API列表
## 网银支付请求

地址:[环境](/tech)/netpay

```
协议：https
方式: POST
```

 参数 | 说明 | 类型 | 最大长度 | 必须 | 备注 
:-|:-|:-|:-|:-:|:--
mid|商户号|string|15|Y|商户号
orderNo|商户订单号|string|32|Y|合作伙伴系统中的订单号
amount|订单金额|number|(13,2)|Y|-
bankCode|银行代码|string|8|N|业务类型为收银台,为空。其他必填<br/>请参看:参数对照-银行简码
notifyUrl|通知URL|string|200|Y|针对该交易的交易状态同步通知接收URL
returnUrl|返回url|string|200|N|用于支付完成后跳转到商户网站指定的地址(如不填写,则不跳转)
currencyType|货币类型|srting|3|Y|国际通用的3位货币符号,如CNY,USD.目前暂时只支持人民币CNY
subject|订单标题|string|50|Y|商品名称
body|商品描述|string|50|Y|商品的具体描述
cardType|支付卡类型|string|8|Y|卡类型，01（储蓄卡）、02（信用卡）
channel|	来源类型	|String	|3|	N	|01（PC端）、 02（手机端）
businessType|	业务类型|	String	|3	|N	|01（B2C-API）、02（B2C-收银台）、 03（B2B-API）、  04（B2B-收银台）、默认：01
remark|备注|string|128|N|-
noise|随机字符串|string|32|Y|随机字符串,不长于32位
sign|签名|string|128|Y|-


支付参数示例:
```html
mid=100000510983456&orderNo=M201611101010100002&subject=iphone7&amount=5230.00&bankCode=102&notifyUrl=https://192.168.1.123:8090/leopard/notify&
returnUrl=https://192.168.1.123:8090/success&
currencyType=CNY&
remark=remark&
body=body&
noise=123123edsr808434&
sign=4A53EDE1F11C7C81DA4970503814E70C
```
## 支付结果通知

见[支付结果通知](/支付通知/notify)


# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)

# 参数对照
-[银行简码](/bankcode)
