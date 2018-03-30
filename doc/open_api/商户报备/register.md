

# 注意事项
```
1.所有的接口参数均使用UTF-8进行编码。
2.接口返回的参数中带有签名参数，商户应首先对返回数据进行验签，以确认返回的数据未被篡改。
```

# 文档说明
>为商户提供商户注册接口。

# 测试账户

```bash
商户号:812017081524279
密钥:q485ghzz6nmr5duhctal

此商户下的支付类型仅用于测试,具体支付类型以正式商户号为准
```

# 接口规则
## 协议规定

规则|说明
:---|:---
传输方式|为保证交易安全性，建议采用HTTPS传输
提交方式|	采用POST方法提交
字符编码|	统一采用UTF-8字符编码
签名算法|	MD5，后续会兼容SHA1、SHA256、HMAC等。
签名要求|	请求和接收数据均需要校验签名，详细方法请参考安全规范-[签名算法](/qianming)
判断逻辑|	先判断协议字段返回，再判断业务返回，最后判断交易状态

## 安全规范

安全规范-[签名算法](/qianming)


# API列表
## 商户报备

地址:[环境](/tech)/merchant/register


```
协议：https
方式: POST
```


| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----|:--------|:------|:-------|:------|:---------------|
|mid|商户号|string|15|Y|商户号|
|merchantName|商户名称|string|20|Y||
|merchantAlias|商户简称|string|15|Y||
|weChatCategory|微信经营类目|string|20|N|<a href="/open_api/document/alipay.xlsx" target="_blank">附件下载</a>|
|alipayCategory|支付宝经营类目|string|20|N|<a href="/open_api/document/alipay.xlsx" target="_blank">附件下载</a>|
|qqCategory|QQ钱包经营类目|string|20|N|<a href="/open_api/document/category.xlsx" target="_blank">附件下载</a>|
|jdCategory|京东经营类目|string|20|N|<a href="/open_api/document/category.xlsx" target="_blank">附件下载</a>|
|idCard|身份证号码|string|18|Y||
|realName|真实姓名|string|20|Y||
|phoneNumber|手机号码|string|11|Y||
|bankCard|借记卡号|string|30|Y||
|bankLinked|借记卡联行号|string|30|Y|<a href="/open_api/document/banklink.xlsx" target="_blank">附件下载</a>|
|creditBankCard|信用卡号|string|30|N||
|creditBankLinked|信用卡联行号|string|30|N|<a href="/open_api/document/banklink.xlsx" target="_blank">附件下载</a>|
|cardAccountType|卡帐户类型|string|30|Y|01 个人 02 企业 目前只支持01个人|
|noise|随机字符串|string|32|Y|随机字符串，不长于32位|
|extendParams|扩展参数|string|500|Y|存放支付种类费率及代付费率,详见如下拓展参数实例及业务参数说明|
|sign|签名|string|128|Y|||


拓展参数示例:
```json

{
    "支付类型_1": [
        {"支付方式_1": "费率|固定金额"},
        {"支付方式_2": "费率|固定金额"},
        {"支付方式_3": "费率|固定金额"}
        
    ],
    "支付类型_2": [
        {"支付方式_1": "费率|固定金额"},
        {"支付方式_2": "费率|固定金额"}
    ],
    "代付": [
        {"D0代付费率": "费率|固定金额"},
        {"T1代付费率": "费率|固定金额"}
    ]
}
例如上传
支付种类为微信,支付方式为扫码和公众号的费率和固定金额、支付种类为网银B2C,支付方式为网银B2C的费率以及代付费率和固定金额
固定金额选填,填写用|分割,不填默认值为0
{
    "21": [ {"05": "0.76"}],                       //说明: 网银B2C （B2C）API费率是0.76,固定金额是0
    "03": [{"01": "0.65|3"},{"03": "0.73|2"}],     //说明: 微信 扫码费率是0.65,固定金额是3;公众号费率是0.73,固定金额是2
    "00": [{"D0": "0.1|1"},{"T1": "0.2|1"}]        //说明: 代付 D0代付费率是0.1,固定金额是1;T1代付费率是0.2,固定金额是1
}

```

请求参数示例:
```json
alipayCategory=2016062900190320&bankCard=1234569871258453526&bankLinked=102405041035&cardAccountType=01&extendParams={"03":[{"01":"0.38"}],"04":[{"01":"0.38"}]}&idCard=610321199203020082&merchantAlias=张三&merchantName=张三&mid=812017081524279&noise=5b923105cd0644a9b195663d13d673a4&phoneNumber=18628271614&realName=张三&weChatCategory=297&sign=793F7E495D2095EB1FD9A8DD16D60700
```

响应参数：

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----|:--------|:------|:-------|:------|:---------------|
|code|返回码|string|32|Y|SUCCESS/FAIL|
|msg|返回消息|string|64|Y||
|code为SUCCESS时返回|
|resultCode|返回状态码|string|32|Y|参见 返回状态码|
|errCode|错误代码|string|32|N||
|errCodeDes|错误代码描述|string|128|N||
|noise|随机字符串|string|32|Y|随机字符串，不长于32位|
|mid|商户号|string|15|Y||
|sign|签名|srting|128|Y||
|resultCode为SUCCESS时返回|
|merchantId|商户号|string|15|Y||
|encryptKey|商户秘钥|string|20|Y|||

响应参数示例:
```json
{
  "code":"SUCCESS",
  "msg":"ok",
  "noise":"WE12GRT58Q",
  "resultCode":"SUCCESS",
  "mid":"100000510983456",
  "merchantId ":"100000510983458",
  "encryptKey":"xsdfegfedfed",
  "sign":"567A9D86BD90C09567A9D86BD90C098C"
}
```

# 业务参数

## 支付类型
| 支付类型 | 支付类型名称 |
|:-----|:--------|
|01|银行卡|
|02|预付费卡|
|03|微信|
|04|支付宝|
|05|百度钱包|
|06|京东|
|07|QQ钱包|
|20|账户支付|
|21|网银B2C|
|22|快捷支付|
|00|代付|



## 支付方式
| 支付方式 | 支付方式名称 |
|:-----|:--------|
|01|扫码|
|02|刷卡|
|03|公众号|
|04|APP|
|05|（B2C）API|
|07|银行卡收单(借记卡)|
|08|快捷支付(费率)API|
|09|快捷支付(封顶)SDK|
|10|银行卡收单(贷记卡)|
|11|H5支付|
|12|（B2C）收银台|
|13|（B2B）API|
|14|（B2B）收银台|
|15|快捷（B2C）收银台|
|D0|D0代付费率|
|T1|T1代付费率|
|M0|D0代付固定金额|
|M1|T1代付固定金额|













	

	
	
