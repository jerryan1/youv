
#     


# 注意事项
```
1.所有的接口参数均使用UTF-8进行编码。
2.接口返回的参数中带有签名参数，商户应首先对返回数据进行验签，以确认返回的数据未被篡改。
3.调用接口时，必须指定收付款方信息，系统会做鉴权，只允许同名进出
```

# 文档说明
>为商户提供快捷支付接口

# 测试账户

```bash
联系我方工作人员获取
```

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

安全规范-[签名算法](/qianming)

# API列表
## 快捷支付（API直联，同名进出）
### 业务流程
> 1. 每张付款方银行卡交易前请先调用开通快捷支付接口，验证此卡是否开通快捷支付功能，如果未开通则会返回银联95516快捷开通页面,用户自行开通即可
> 2. 开通快捷支付功能后，调用快捷预下单接口，进行下单操作(请核实收、付款方个人信息正确性、真实性；以及收付款方银行卡开户行是否复合要求)，下单成功后用户会收到短信验证码
> 3. 使用短信验证码后进行订单支付
> 4. 支付成功我方平台异步通知下单申请时上送的notifyUrl中的地址

### 开通快捷支付
 - 接口说明:在进行快捷支付前要先对付款银行卡开通快捷支付，同一张银行卡只需要开通一次，同步回应。

地址:[环境](/tech)/quickPay/openDirectPay

> 请求参数:

|     参数     |     说明     |   类型   |  长度  |  必须  | 备注                                  |
| :--------: | :--------: | :----: | :--: | :--: | :---------------------------------- |
|    mid     |    商户号     | String |  15  |  Y   | 商户号                                 |
|  realName  |   付款方姓名    | String |  20  |  Y   | 付款方持卡人姓名，用UTF-8转码                   |
|   cardId   |  付款方身份证号码  | String |  18  |  Y   | 卡在银行预留身份证号码                         |
|  phoneNo   |  付款方手机号码   | String |  11  |  Y   | 卡在银行预留的手机号码                         |
|  bankCode  |  付款方银行编码   | String |  10  |  Y   | 银行卡编号(详见- [银行简码](/bankcode))        |
| bankCardNo |  付款方银行卡号   | String |  30  |  Y   | 付款方银行卡号                             |
|   flowNo   |   请求流水号    | String |  32  |  Y   | 请求流水号                               |
| returnUrl  | 开通成功商户前台页面地址 | String | 256  |  Y   | 开通成功后返回商户前台页面地址,请不要加入&符号                          |
|   noise    |   随机字符串    | String |  32  |  Y   | 随机字符串,不长于32位                        |
|    sign    |     签名     | String | 200  |  Y   | 数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名 |

### 接口返回

| 参数                    | 说明         | 类型     | 长度   | 必须   | 备注                                       |
| --------------------- | ---------- | ------ | ---- | ---- | ---------------------------------------- |
| code                  | 返回码        | string | 32   | Y    | SUCCESS/其他;如非SUCCESS见[错误码](/error_code),交易是否成功需要查看 resultCode来判断 |
| msg                   | 返回消息       | string | 64   | N    | 信息描述                                     |
| code为SUCCESS时返回       |            |        |      |      |                                          |
| resultCode            | 返回状态码      | string | 32   | Y    | 如非SUCCESS 参见 [返回状态码](/error_code)        |
| errCode               | 错误代码       | string | 32   | N    |                                          |
| errCodeDes            | 错误代码描述     | string | 128  | N    |                                          |
| noise                 | 随机字符串      | string | 32   | Y    | 随机字符串，不长于32位                             |
| mid                   | 商户号        | string | 15   | Y    | 商户号                                      |
| flowNo                | 请求流水号      | string | 15   | Y    | 请求流水号                                    |
| sign                  | 签名         | string | 128  | Y    | [数字签名规则](/qianming)                      |
| resultCode为SUCCESS时返回 |            |        |      |      |                                          |
| isOpen                | 是否开通       | string | 1    | Y    | 1.开通 2未开通                                |
| openHtml              | 开通用的html页面 | string | 1024 | N    | openHtml为2时返回,此页面为银联返回的95516快捷开通页面，将字符串提取后转为HTML页面后会跳转到银联支付页面由用户完成开通功能 |


返回示例:
```json
{
    "code":"SUCCESS",
    "isOpen":"2",
    "noise":"OKIKA9hHQecxdqSAFaHBm2CzlUdM0rLS",
    "resultCode":"SUCCESS",
    "sign":"7485F691945283AF9A29C43570DE1052",
    "mid":"822017101127057",
    "flowNo":"1509119570004",
    "openHtml":"此处是开通快捷的html页面字符串"
}
```

###  快捷预下单
- 接口说明：后台API直联模式；商户需通过自定义界面获得用户银行卡要素信息，并通过后台直联方式直接发送至在线支付平台；本订单请求接口将获得同步回应，但回应结果仅用于确认交易请求被受理，还需要通过“快捷确认支付”接口上传短信验证码来最终完成支付并明确交易结果。

地址:[环境](/tech)/quickPay/directPay


> 请求参数:

|        参数        |    说明    |   类型   |   长度   |  必须  | 备注                                  |
| :--------------: | :------: | :----: | :----: | :--: | :---------------------------------- |
|       mid        |   商户号    | String |   15   |  Y   | 商户号                                 |
|    notifyUrl     |  通知URL   | String |  200   |  Y   | 针对该交易的交易状态同步通知接受URL(如不填写,则收不到通知)    |
|     orderNo      |  外部交易号   | String |   32   |  Y   | 合作伙伴交易号（确保在合作伙伴系统中唯一）               |
|     subject      |   商品名称   | String |   50   |  Y   | 商品的标题                               |
|       body       |   商品描述   | String |   50   |  Y   | 商品的具体描述                             |
|      amount      |   交易金额   | number |  13,2  |  Y   | 交易的总金额，单位为元                         |
|     cardType     |   卡类标识   | String |   10   |  Y   | 02（信用卡）                             |
|    bankCardNo    | 付款方银行卡号  | String |   30   |  Y   | 付款方银行卡号                             |
|     realName     |  付款方姓名   | String |   20   |  Y   | 付款方持卡人姓名，用UTF-8转码                   |
|      cardId      | 付款方身份证号码 | String |   18   |  Y   | 卡在银行预留身份证号码                         |
|     phoneNo      | 付款方手机号码  | String |   11   |  Y   | 卡在银行预留的手机号码                         |
|   expireMonth    | 卡有效期（月）  | String |   2    |  Y   | 信用卡有效期月份                            |
|    expireYear    | 卡有效期（年）  | String |   2    |  Y   | 信用卡有效期年份                            |
|       cvv        |   CVV码   | String |   3    |  Y   | 信用卡CVV码                             |
|     bankCode     | 付款方银行编码  | String |   10   |  Y   | 银行卡编号(详见- [银行简码](/bankcode))        |
|     feeRate      |    费率    | number | (13,2) |  Y   | 费率，不带百分号如果费率为0.4% 则传0.4             |
|     mustAmt      |   固定金额   | String | (13,2) |  N   | 固定金额，默认为0                           |
|    isPrivate     |   对公对私   | String |   2    |  Y   | 01对公  02对私                          |
|  receivedCardNo  | 收款方银行卡号  | String |   30   |  Y   | 收款方银行卡号                             |
| receivedPhoneNo  | 收款方手机号码  | String |   11   |  Y   | 收款方手机号码                             |
| receivedBankCode | 收款方银行编码  | String |   10   |  Y   | 收款方银行编码                             |
|      remark      |    备注    | String |  200   |  N   | 备注                                  |
|      noise       |  随机字符串   | String |   32   |  Y   | 随机字符串,不长于32位                        |
|       sign       |    签名    | String |  200   |  Y   | 数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名 |

### 接口返回

| 参数                    | 说明     | 类型     | 长度   | 必须   | 备注                                       |
| --------------------- | ------ | ------ | ---- | ---- | ---------------------------------------- |
| code                  | 返回码    | string | 32   | Y    | SUCCESS/其他;如非SUCCESS见[错误码](/error_code),交易是否成功需要查看 resultCode来判断 |
| msg                   | 返回消息   | string | 64   | Y    | 错误信息描述                                   |
| code为SUCCESS时返回       |        |        |      |      |                                          |
| resultCode            | 返回状态码  | string | 32   | Y    | 如非SUCCESS 参见 [返回状态码](/error_code)        |
| errCode               | 错误代码   | string | 32   | N    |                                          |
| errCodeDes            | 错误代码描述 | string | 128  | N    |                                          |
| noise                 | 随机字符串  | string | 32   | Y    | 随机字符串，不长于32位                             |
| mid                   | 商户号    | string | 15   | Y    | 商户号                                      |
| orderNo               | 商户订单号  | string | 15   | Y    | 商户上传的订单号                                 |
| sign                  | 签名     | string | 128  | Y    | [数字签名规则](/qianming)                      |
| resultCode为SUCCESS时返回 |        |        |      |      |                                          |
| flowNo                |        | string | 32   | Y    | 平台账单号                                 |


返回示例:
```json
{
    "code":"SUCCESS",
    "errCode":"ORDER_FAIL",
    "errCodeDes":"付款人银行卡信息校验不通过",
    "flowNo":"ZF20170808123519570849",
    "mid":"822017072724126",
    "noise":"FclolQ1dq9nYagPe2atzOXIfFflVxGF9",
    "orderNo":"20170808123508544",
    "resultCode":"ERROR",
    "sign":"1367F04122F0DA6CAE5F51B4AB040DA0"
}
```
## 快捷确认支付

- 接口说明：参考上述快捷预下单中订单请求接口说明，同步回应；商户在发起订单请求后，还需要提供界面让用户填写所收到的短信验证码；商户通过本“快捷确认支付”接口上传短信验证码来最终完成支付并明确交易结果;最终交易结果以异步通知为准。

地址:[环境](/tech)/quickPay/payment


> 请求参数:

|     参数     |  说明   |   类型   |  长度  |  必须  | 备注                                  |
| :--------: | :---: | :----: | :--: | :--: | :---------------------------------- |
|    mid     |  商户号  | String |  15  |  Y   | 合作商户的商户号，由系统分配                   |
|  orderNo   | 商户订单号 | String |  32  |  Y   | 合作伙伴交易号（确保在合作伙伴系统中唯一）               |
| verifyCode | 短信验证码 | String |  50  |  Y   | 短信验证码                               |
|   noise    | 随机字符串 | String | 200  |  Y   | 随机字符串                               |
|    sign    |  签名   | String | 200  |  Y   | 数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名 |

### 接口返回

| 参数              | 说明     | 类型     | 长度   | 必须   | 备注                                       |
| --------------- | ------ | ------ | ---- | ---- | ---------------------------------------- |
| code            | 返回码    | string | 32   | Y    | SUCCESS/其他;如非SUCCESS见[错误码](/error_code),交易是否成功需要查看 resultCode来判断 |
| msg             | 返回消息   | string | 64   | Y    | 错误信息描述                                   |
| code为SUCCESS时返回 |        |        |      |      |                                          |
| resultCode      | 返回状态码  | string | 32   | Y    | 如非SUCCESS 参见 [返回状态码](/error_code)        |
| errCode         | 错误代码   | string | 32   | N    |                                          |
| errCodeDes      | 错误代码描述 | string | 128  | N    |                                          |
| noise           | 随机字符串  | string | 32   | Y    | 随机字符串，不长于32位                             |
| mid             | 商户号    | string | 15   | Y    | 商户号                                      |
| orderNo         | 商户订单号  | string | 15   | Y    | 商户订单号                                    |
| flowNo          |        | string | 32   | Y    | 平台账单号                                    |
| transTime       |        | string | 20   | Y    | 交易时间                                     |
| sign            | 签名     | string | 128  | Y    | [数字签名规则](/qianming)                      |


返回示例:
```json
{
    "mid":"822017072724126",
    "resultCode":"SUCCESS",
    "code":"SUCCESS",
	"msg":"成功",
    "flowNo":"ZF20170808123519570849",
    "noise":"FclolQ1dq9nYagPe2atzOXIfFflVxGF9",
    "orderNo":"20170808123508544",
    "sign":"1367F04122F0DA6CAE5F51B4AB040DA0"
}

```

## 手机控件支付（订单获取）

### 业务流程
> 1. 调用SDK快捷预下单接口进行下单操作，下单成功后返回tokenId值
> 2. 使用tokenId唤起SDK的银联支付功能页面,完成订单支付
> 3. 支付成功我方平台异步通知下单申请时上送的notifyUrl中的地址

- 接口说明：后台API直联模式，异步回应；用户在商户APP上下单时，商户APP将交易请求信息通过后台直联方式直接发送至在线支付平台，获得受理订单号等信息后，唤起支付控件并最终完成支付；支付完成后，在线支付系统将回调商户系统接口通知最终支付结果。

地址:[环境](/tech)/quickPay/mobilePay


###  SDK快捷预下单
> 请求参数:

|        参数        |    说明     |   类型   |   长度   |  必须  | 备注                                  |
| :--------------: | :-------: | :----: | :----: | :--: | :---------------------------------- |
|       mid        |    商户号    | String |   15   |  Y   | 商户号                                 |
|    notifyUrl     |   通知URL   | String |  200   |  Y   | 针对该交易的交易状态同步通知接受URL(如不填写,则收不到通知)    |
|     orderNo      |   外部交易号   | String |   32   |  Y   | 合作伙伴交易号（确保在合作伙伴系统中唯一）               |
|     subject      |   商品名称    | String |   50   |  Y   | 商品的标题                               |
|       body       |   商品描述    | String |   50   |  Y   | 商品的具体描述                             |
|      amount      |   交易金额    | number |  13,2  |  Y   | 交易的总金额，单位为元                         |
|     cardType     |   卡类标识    | String |   10   |  Y   | 02（信用卡）                             |
|    bankCardNo    |  付款方银行卡号  | String |   30   |  Y   | 付款方银行卡号                             |
|     feeRate      |    费率     | number | (13,2) |  Y   | 费率，不带百分号如果费率为0.4% 则传0.4             |
|     mustAmt      |   固定金额    | String | (13,2) |  Y   | 固定金额                                |
|  receivedCardNo  |  收款方银行卡号  | String |   30   |  Y   | 收款方银行卡号                             |
|  receivedCardId  |  收款方身份证号  | String |   18   |  Y   | 收款方身份证号                             |
| receivedPhoneNo  |  收款方手机号码  | String |   11   |  Y   | 收款方手机号码                             |
| receivedBankCode |  收款方银行编码  | String |   10   |  Y   | 收款方银行编码                             |
| receivedRealName | 收款方银行卡开户名 | string |   20   |  Y   | 收款方银行卡开户名                           |
|      remark      |    备注     | String |  200   |  N   | 备注                                  |
|      noise       |   随机字符串   | String |   32   |  Y   | 随机字符串,不长于32位                        |
|       sign       |    签名     | String |  200   |  Y   | 数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名 |

### 接口返回
| 参数                    | 说明     | 类型     | 长度   | 必须   | 备注                                       |
| --------------------- | ------ | ------ | ---- | ---- | ---------------------------------------- |
| code                  | 返回码    | string | 32   | Y    | SUCCESS/其他;如非SUCCESS见[错误码](/error_code),交易是否成功需要查看 resultCode来判断 |
| msg                   | 返回消息   | string | 64   | Y    | 错误信息描述                                   |
| code为SUCCESS时返回       |        |        |      |      |                                          |
| resultCode            | 返回状态码  | string | 32   | Y    | 如非SUCCESS 参见 [返回状态码](/error_code)        |
| errCode               | 错误代码   | string | 32   | N    |                                          |
| errCodeDes            | 错误代码描述 | string | 128  | N    |                                          |
| noise                 | 随机字符串  | string | 32   | Y    | 随机字符串，不长于32位                             |
| mid                   | 商户号    | string | 15   | Y    | 商户号                                      |
| orderNo               | 商户订单号  | string | 15   | Y    | 商户上传的订单号                                 |
| sign                  | 签名     | string | 128  | Y    | [数字签名规则](/qianming)                      |
| resultCode为SUCCESS时返回 |        |        |      |      |                                          |
| flowNo                |        | string | 32   | Y    | 平台账单号                                 |
| tokenId               |        | string | 20   | Y    | 受理订单号,用于唤起SDK手机控件支付                      |


返回示例:
```json
{
"code": "SUCCESS", 
"resultCode":"FAST_ORDER_FAIL",
"mid": "812017050623837",
"msg": "发短信信息错误！ 错误原因：订单系统异常",
"noise": "jIZG32mK0i", 
"orderNo": "20170621114400126", 
"flowNo": "ZF20170621114414128001", 
"sign": "D6CF3FEA05A7EE0876EF5610F1449C86",
"tokenId": "739987160098909026501"
}

```

## 支付结果通知

见[支付结果通知](/支付通知/notify)

# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)