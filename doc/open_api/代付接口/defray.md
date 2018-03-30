
#    




# 注意事项
> 此处填写在使用过程中的注意事项

# 测试账户

测试账户信息
​	


# 接口规则

## 协议规定

| 规则   | 说明                                       |
| ---- | ---------------------------------------- |
| 传输方式 | 为保证交易安全性，建议采用HTTPS传输                     |
| 提交方式 | 采用POST方法提交                               |
| 数据格式 | 提交的数据为key=value&key=value格式返回数据都为JSON格式  |
| 字符编码 | 统一采用UTF-8字符编码                            |
| 签名算法 | MD5，后续会兼容SHA1、SHA256、HMAC等。              |
| 签名要求 | 请求和接收数据均需要校验签名，详细方法请参考安全规范-[签名算法](/qianming) |
| 判断逻辑 | 先判断协议字段返回，再判断业务返回，最后判断交易状态               |

## 安全规范

安全规范-[签名算法](/qianming)

# API列表


## 下单请求

地址:[环境](/tech)/defray


|       参数        |   说明   |   类型   |   长度   |  必须  | 备注                             |
| :-------------: | :----: | :----: | :----: | :--: | :----------------------------- |
|       mid       |  商户号   | string |   15   |  Y   | 商户号                            |
|     orderNo     | 商户订单号  | string |   32   |  Y   | 商户系统中的订单号                      |
|     amount      |  订单金额  | number | (18,2) |  Y   | 代付金额                           |
|   receiveName   | 收款人姓名  | string |   20   |  Y   |                                |
|  openProvince   |  开户省   | string |   20   |  Y   |                                |
|    openCity     |  开户市   | string |   20   |  Y   |                                |
|    bankCode     | 收款银行编码 | srting |   20   |  Y   | 见 [银行简码](/bankcode) |
| bankBranchName  |  支行名称  | string |   30   |  Y   |                                |
|     cardNo      |   卡号   | string |   20   |  Y   |                                |
|   bankLinked    |  联行号   | string |   20   |  N   | 对私账户不传,对公账户必传                  |
|      type       |  代付类型  | string |   2    |  Y   | 02 额度(D0) 01 普通(T+N)           |
| cardAccountType | 卡帐户类型  | string |   2    |  Y   | 01 个人 02 企业目前只支持01个人           |
|  extendParams   |  扩展字段  | string |  255   |  N   |                                |
|      noise      | 随机字符串  | string |   32   |  Y   | 随机字符串，不长于32位                   |
|sign|签名|string|128|Y|见 [数字签名规则](/qianming)


请求示例:
```
amount=99.3&receiveName=苏家森&cardNo=8011101509400060000&bankLinked=102623053001&sourceType=00&username=苏家森&
orderNo=20170315143302100384&orgCode=802000000000023&bankName=中国工商银行&bankBranchName=中国工商银行北海分行云南路支行&cardAccountType=01&
openProvince=广西&openProvince=广西&openCity=北海市&mid=801110150940006&noise=bwk16i57n1izkwxyy5d4&sign=567A9D86BD90C09567A9D86BD90C098C
```

响应参数：

|          参数           |  说明   |   类型   |  长度  |  必须  | 备注                                 |
| :-------------------: | :---: | :----: | :--: | :--: | :--------------------------------- |
|         code          |  返回码  | string |  32  |  Y   | SUCCESS/FAIL                        |
|          msg          | 返回消息  | string |  64  |  Y   |                                    |
|    code为SUCCESS时返回    |       |        |      |      |                                    |
|      resultCode       | 返回状态码 | string |  32  |  Y   | 参见 [业务结果码](/error_code) |
|         noise         | 随机字符串 | string |  32  |  Y   | 随机字符串，不长于32位                       |
|          mid          |  商户号  | string |  15  |  Y   |                                    |
|         sign          |  签名   | srting | 128  |  Y   |       |
|        errCode        |  错误代码  | string |  32  |  N   |     |        
|      errCodeDes       | 错误代码描述 | string | 128  |  N   |  |
| resultCode为SUCCESS时返回 |       |        |      |      |                                    |
|        orderNo        | 商户订单号 | string |  32  |  Y   | 商户订单号                              |
|       serialNo        | 平台订单号 | string |  32  |  Y   |                                    |
|      transState       | 交易状态  | string |  1   |  Y   | 1：受理中 3：代付成功 4：代付失败                |
|     transMessage      | 交易说明  | string |  10  |  Y   |                                    |

响应示例：<br/>
```
{
  "code":"SUCCESS",
  "msg":"ok",
  "noise":"WE12GRT58Q",
  "resultCode":"SUCCESS",
  "mid":"100000510983456",
  "orderNo":"M201611101010100002",
  "serialNo":"20161110101015000020",
  "transState":"3",
  "transMessage":"支付成功",
  "sign":"567A9D86BD90C09567A9D86BD90C098C "
}
```

## 代付查询

地址:[环境](/tech)/defray/query


|   参数    |  说明   |   类型   |  长度  |  必须  | 备注           |
| :-----: | :---: | :----: | :--: | :--: | :----------- |
|   mid   |  商户号  | string |  15  |  Y   | 商户号          |
| orderNo | 商户订单号 | string |  32  |  Y   | 商户系统中的订单号    |
|  noise  | 随机字符串 | string |  32  |  Y   | 随机字符串，不长于32位 |
|sign|签名|string|128|Y|见 [数字签名规则](/qianming)


请求示例:

```
mid=100000510983456&receiveName=苏家森&orderNo=M201611101010100002&noise=A2343WER157&sign=567A9D86BD90C09567A9D86BD90C098C
```

响应参数：

|          参数           |  说明   |   类型   |  长度  |  必须  | 备注                                 |
| :-------------------: | :---: | :----: | :--: | :--: | :--------------------------------- |
|         code          |  返回码  | string |  32  |  Y   | SUCCESS/FAIL号                       |
|          msg          | 返回消息  | string |  64  |  Y   |                                    |
|    code为SUCCESS时返回    |       |        |      |      |                                    |
|      resultCode       | 返回状态码 | string |  32  |  Y   | 参见 [业务结果码](/error_code) |
|         noise         | 随机字符串 | string |  32  |  Y   | 随机字符串，不长于32位                       |
|          mid          |  商户号  | string |  15  |  Y   |                                    |
|         sign          |  签名   | srting | 128  |  Y   |                                    |
| resultCode为SUCCESS时返回 |       |        |      |      |                                    |
|        orderNo        | 商户订单号 | string |  32  |  Y   | 商户订单号                              |
|       serialNo        | 平台订单号 | string |  32  |  Y   |                                    |
|      transState       | 交易状态  | string |  1   |  Y   | 1：受理中 3：代付成功 4：代付失败                |
|     transMessage      | 交易说明  | string |  10  |  Y   |                                    | 			|

响应示例：<br/>
```
{
  "code":"SUCCESS",
  "msg":"ok",
  "noise":"WE12GRT58Q",
  "resultCode":"SUCCESS",
  "mid":"100000510983456",
  "orderNo":"M201611101010100002",
  "serialNo":"20161110101015000020",
  "transState":"3",
  "transMessage":"支付成功",
  "sign":"567A9D86BD90C09567A9D86BD90C098C "
}
```

# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)



