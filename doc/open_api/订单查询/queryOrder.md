
#     




# 注意事项
```
1.所有的接口参数均使用UTF-8进行编码。
2.接口参数中带有签名参数，商户应对数据进行验签，以确认返回的数据未被篡改。
```

# 文档说明
>为商户提供单笔及批量订单及刷卡(商户主扫)订单状态查询接口。

# 测试账户
>测试账户信息

# 接口规则
## 协议规定

| 规则   | 说明                                       |
| ---- | ---------------------------------------- |
| 传输方式 | 为保证交易安全性，建议采用HTTPS传输                     |
| 提交方式 | 采用POST方法提交                               |
| 数据格式 | 提交和返回数据都为JSON格式                          |
| 字符编码 | 统一采用UTF-8字符编码                            |
| 签名算法 | MD5，后续会兼容SHA1、SHA256、HMAC等。              |
| 签名要求 | 请求和接收数据均需要校验签名，详细方法请参考安全规范-[签名算法](/qianming) |
| 判断逻辑 | 先判断协议字段返回，再判断业务返回，最后判断交易状态               |

## 安全规范
> 安全规范-[签名算法](/qianming)

## 数据格式
> 查询参数:


# API列表

## 订单单笔查询


地址:[环境](/tech)/query/single


```
协议：https
方式: POST
```

|  参数  |  说明  |   类型   |  长度  |  必须  | 备注   |
| :--: | :--: | :----: | :--: | :--: | :--- |
| mid  | 商户号  | string |  15  |  Y   | 商户号  |
|orderNo|商户订单号|string|256|Y|订单号
|noise|随机字符串|string|32|Y|随机字符串，不长于32位|
|sign|签名|string|128|Y|-|


单笔状态查询参数示例:
```html

mid=100000510983456&orderNo=19895074304585363210811455355824&noise=12eqweqw432423432423&sign=4A53EDE1F11C7C81DA4970503814E70C

```

> 响应参数:

|          参数           |   说明   |   类型   |  长度  |  必须  | 备注                                       |
| :-------------------: | :----: | :----: | :--: | :--: | :--------------------------------------- |
|         code          |  返回码   | string |  32  |  Y   | SUCCESS/FAIL                             |
|          msg          |  返回消息  | string |  64  |  Y   |                                          |
|    code位SUCCESS时返回    |        |        |      |      |                                          |
|      resultCode       | 返回状态码  | string |  32  |  Y   | 请参看:参数对照-通知返回码                           |
|          mid          |  商户号   | number |  15  |  Y   |                                          |
|         noise         | 随机字符串  | string |  32  |  Y   | 随机字符串，不长于32位                             |
|         sign          |   签名   | string | 128  |  Y   |                                          |
| resultCode为SUCCESS时返回 |        |        |      |      |                                          |
|        orderNo        | 商户订单号  | string |  32  |  Y   | 商户订单号                                    |
|        tradeNo        |  交易号   | string |  64  |  N   | 支付宝或微信的交易号                               |
|        flowNo         | 支付流水号  | string |  32  |  Y   | 平台流水号                                    |
|        amount         |  订单金额  | number | 18,2 |  Y   |                                          |
|         type          | 商户返回码  | string |  10  |  Y   | wechat：微信<br/>alipay：支付宝<br/>QQwallet：QQ钱包<br/>netpay：网银支付<br/>quickpay：快捷支付 |
|        status         |  订单状态  | string |  2   |  Y   | 0：初始<br/>1：成功<br/>2：失败<br/>3：关闭          |
|       orderTime       | 订单创建时间 | string |  14  |  Y   | 格式yyyyMMddHHmmss                         |
|        payTime        | 支付完成时间 | string |  14  |  N   | 格式yyyyMMddHHmmss，仅在支付成功或失败时存在值           |

示例:

```json
{
    "code":"SUCCESS",
  "msg":"ok",
  "resultCode":"SUCCESS",
  "noise":"6W5H246V",
  "mid":"100000510983456",
  "orderNo":" M201611101010100002",
  "flowNo":"20161101010100198763",
  "tradeNo":" 1217752501201407033233368018",
  "amount":5230.00,
  "type":"wechat",
  "status":"1",
  "orderTime":"20161110101010",
  "payTime":"20161110101323",
  "sign":"567A9D86BD90C09567A9D86BD90C098C"
}
```

## 订单批量查询


地址:[环境](/tech)/query/multi


|  参数  |  说明  |   类型   |  长度  |  必须  | 备注   |
| :--: | :--: | :----: | :--: | :--: | :--- |
| mid  | 商户号  | string |  15  |  Y   | 商户号  |
|orderNo|商户订单号|string|256|Y|多笔订单使用逗号","分隔
|noise|随机字符串|string|32|Y|随机字符串，不长于32位|
|sign|签名|string|128|Y|-|

批量状态查询参数示例:


```
mid=100000510983456&noise=788QWEET&orderNo=M201611101010100002,M201611101010100011&
sign=567A9D86BD90C09567A9D86BD90C098C
```

|          参数           |   说明   |   类型   |  长度  |  必须  | 备注                                       |
| :-------------------: | :----: | :----: | :--: | :--: | :--------------------------------------- |
|         code          |  返回码   | string |  32  |  Y   | SUCCESS/FAIL,非SUCCESS见[错误码](/error_code) |
|          msg          |  返回消息  | string |  64  |  Y   |                                          |
|    code位SUCCESS时返回    |        |        |      |      |                                          |
|      resultCode       | 返回状态码  | string |  32  |  Y   | 请参看:参数对照-通知返回码                           |
|          mid          |  商户号   | number |  15  |  Y   |                                          |
|         noise         | 随机字符串  | string |  32  |  Y   | 随机字符串，不长于32位                             |
|         sign          |   签名   | string | 128  |  Y   |                                          |
| resultCode为SUCCESS时返回 |        |        |      |      |                                          |
|        orders         |  订单列表  | array  |  -   |  Y   | 订单查询结果集,orders下的详情见order详情               |
|        order详情        |        |        |      |      |                                          |
|        orderNo        | 商户订单号  | string |  32  |  Y   |                                          |
|        flowNo         | 支付流水号  | string |  32  |  Y   | leopard系统中的流水号                           |
|        tradeNo        |  交易号   | string |  64  |  N   | 支付宝或微信的交易号                               |
|        amount         |  订单金额  | number | 18,2 |  Y   |                                          |
|         type          | 商户返回码  | string |  10  |  Y   | wechat：微信<br/>alipay：支付宝<br/>netpay：网银支付<br/>quickpay：快捷支付<br/>QQwallet:QQ钱包<br/>jd:京东支付|
|        status         | 支付完成时间 | string |  14  |  Y   | 0：初始<br/>1：成功<br/>2：失败<br/>3：关闭<br/>6:获取交易信息失败 |
|       orderTime       | 订单创建时间 | string |  14  |  Y   | 格式yyyyMMddHHmmss                         |
|        payTime        | 支付完成时间 | string |  14  |  N   | 格式yyyyMMddHHmmss，仅在支付成功或失败时存在值           |


示例:

```json

   {
  "code":"SUCCESS",
  "msg":"ok",
  "resultCode":"SUCCESS",
  "noise":"83NDI0Q93",
  "mid":"100000510983456",
  "orders":"[{
	"orderNo":"M201611101010100002",
	"flowNo":"20161101010100198763\",
	"tradeNo":" 1217752501201407033233368018",
	"amount":5230.00,
	"type":"wechat",
	"status":"1",
	"orderTime":"20161110101010",
	"payTime":"20161110101323"
  },
  {
	"orderNo":"M201611101016180003",
	"flowNo":"20161110101618198765",
	 "tradeNo":"1217752501201407033233368103",
	"amount":5230.00,
	"type":"wechat",
	"status":"1,
	"orderTime":"20161110101618",
	"payTime":"20161110101931"
  }]",
  "sign":"567A9D86BD90C09567A9D86BD90C098C"

}
```

 




# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)





​	


​	
