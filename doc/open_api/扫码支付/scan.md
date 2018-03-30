
#    




# 注意事项
> QQ钱包/微信/支付宝/京东支付  扫码支付接口参数一致，系统根据不同的type进行区分

# 测试账户

```bash
商户号:812017050323777
密钥:ddbax6n4cg8qj958ytt6

测试商户号最大限额1.00元/笔
```



# 接口规则

## 协议规定

| 规则   | 说明                                       |
| :--- | :--------------------------------------- |
| 传输方式 | 为保证交易安全性，建议采用HTTPS传输                     |
| 提交方式 | 采用POST方法提交                               |
| 数据格式 | 提交和返回数据都为JSON格式                          |
| 字符编码 | 统一采用UTF-8字符编码                            |
| 签名算法 | MD5，后续会兼容SHA1、SHA256、HMAC等。              |
| 签名要求 | 请求和接收数据均需要校验签名，详细方法请参考安全规范-[签名算法](/qianming) |
| 判断逻辑 | 先判断协议字段返回，再判断业务返回，最后判断交易状态               |

## 安全规范

安全规范-[签名算法](/qianming)
## 数据格式

**提交数据**
采用HTTPS标准的POST协议，为了保证接收方接收数据正确,传输数据必须签名。
```bash

mid=100000510983456&orderNo=M201611101010100002&subject=iphone7&body=iphone7&amount=5230.00&
type=wechat&deviceInfo=A001&authCode=1234567890&
noise=A2343WER157&sign=567A9D86BD90C09567A9D86BD90C098C
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

code参数，SUCCESS表示调用成功；非SUCCESS表示调用失败,非SUCCESS时见错误码描述。


# API列表


## 下单请求


地址: [环境](/tech)/pay/paycode



| 参数        | 说明    | 类型     | 长度   | 必须   | 备注                                       |
| :-------- | :---- | :----- | :--- | :--- | :--------------------------------------- |
| mid       | 商户号   | string | 15   | Y    | 商户号                                      |
| orderNo   | 商户订单号 | string | 32   | Y    | 商户系统中的订单号                                |
| subject   | 订单标题  | string | 127  | Y    | 可放置商品名称                                  |
| body      | 订单描述  | string | 127  | Y    | 商品的简要描述                                  |
| amount    | 订单金额  | number | 18   | Y    | ##.##  (圆.角分)                            |
| type      | 支付种类  | string | 10   | Y    | wechat:微信<br/>alipay:支付宝<br/>QQwallet:QQ钱包<br/>jd:京东支付 |
| notifyUrl | 通知URL | string | 256  | Y    | 接收支付通知的url                               |
| noise     | 随机字符串 | string | 32   | Y    | 随机字符串，不长于32位                             |
| sign      | 签名    | string | 128  | Y    | 见 [数字签名规则](/qianming)                    |


请求示例:
```
mid=100000510983456&orderNo=M201611101010100002&subject=iphone7&body=iphone7&amount=5230.00&
type=wechat&notifyUrl=https://192.168.1.123:8090/leopard/notify&noise=A2343WER157&sign=567A9D
86BD90C09567A9D86BD90C098C
```
响应参数：
	<table border="0" cellspacing="0"  cellpadding="0">
		<tr>
		    <th>参数</th>
		    <th>说明</th>
		    <th>类型</th>
		    <th>长度</th>
		    <th>必须</th>      
		    <th>备注</th>
		</tr>
		<tr>
		    <td>code</td>
		    <td>返回码</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>SUCCESS/FAIL </td>
		</tr>
		<tr>
		    <td>msg</td>
		    <td>返回消息</td>
		    <td>string</td>
		    <td>64</td>
		    <td>N</td>
		    <td></td>
		</tr>
	    <tr><td colspan="6">code为SUCCESS时返回</td></tr>
		<tr>
		    <td>resultCode</td>
		    <td>返回状态码</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>参见 [返回状态码](/error_code)</td>
		</tr>
		<tr>
		    <td>errCode</td>
		    <td>错误代码</td>
		    <td>string</td>
		    <td>32</td>
		    <td>N</td>
		    <td></td>
		</tr>
		<tr>
		    <td>errCodeDes</td>
		    <td>错误代码描述</td>
		    <td>string</td>
		    <td>32</td>
		    <td>N</td>
		    <td></td>
		</tr>
		<tr>
		    <td>noise</td>
		    <td>随机字符串</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>随机字符串，不长于32位</td>
		</tr>
		<tr>
		    <td>mid</td>
		    <td>商户号</td>
		    <td>string</td>
		    <td>15</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>sign</td>
		    <td>签名</td>
		    <td>string</td>
		    <td>128</td>
		    <td>Y</td>
		    <td>[数字签名规则](/qianming)</td>
		</tr>
	    <tr><td colspan="6">resultCode为SUCCESS时返回</td></tr>
		<tr>
		    <td>orderNo</td>
		    <td>商户订单号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>amount</td>
		    <td>订单金额</td>
		    <td>string</td>
		    <td>18,2</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>type</td>
		    <td>支付种类</td>
		    <td>string</td>
		    <td>10</td>
		    <td>Y</td>
		    <td>wechat:微信<br/>alipay:支付宝<br/>QQwallet:QQ钱包<br/>jd:京东支付</td>
		</tr>
		<tr>
		    <td>datetime</td>
		    <td>订单创建时间</td>
		    <td>string</td>
		    <td>14</td>
		    <td>Y</td>
		    <td>yyyyMMddHHmmss</td>
		</tr>
		<tr>
		    <td>qrCode</td>
		    <td>二维码链接</td>
		    <td>string</td>
		    <td>1024</td>
		    <td>Y</td>
		    <td>返回支付宝、微信或QQ钱包的二维码地址</td>
		</tr>
	</table>

响应示例：<br/>
```
{
  "code":"SUCCESS",
  "msg":"ok",
  "resultCode":"SUCCESS",
  "noise":"457QWCFYU5",
  "mid":"100000510983456",
  "orderNo":" M201611101010100002",
  "amount":5230.00,
  "type":"wechat",
  "datetime":"20161110101015",
  "qrCode":"weixin://wxpay/s/An4baqw ",
  "sign":"567A9D86BD90C09567A9D86BD90C098C "
}
```


## 支付通知
 见[支付结果通知](/支付通知/notify)

## 支付查询
见 [订单查询](/订单查询/queryOrder)

# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)



