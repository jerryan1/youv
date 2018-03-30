
# 注意事项

- 当调用刷卡支付接口返回待查询状态，需要调用查询接口查询订单实际支付状态。
- 调用查询接口建议：查询6次每隔5秒（具体的查询次数和时间也可自定义，建议查询时间不低于30秒），6次查询完成接口仍未返回成功标识（即查询接口返回的payStatus不是1）则调用撤销接口进行撤销。


# 测试账户

测试账户信息
	


# 接口规则
 
## 协议规定

规则|说明
---|---
传输方式|为保证交易安全性，建议采用HTTPS传输
提交方式|	采用POST方法提交
数据格式|	返回数据为JSON格式
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
mid=801110150940001&amount=0.01&orderNo=05301565988620875337699247666273
&subject=商品标题&body=商品描述&type=wechat&deviceInfo=设备号&authCode=24rwrewrwe
&noise=123213123dasdaseqwedas&sign=F41E203EC14B15B82D702B1C890AB356
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

一般有返回有code参数，SUCCESS表示调用成功；非SUCCESS表示调用失败。


# API列表


接口说明：用户展示微信或支付宝的条码/二维码给商户系统扫描后直接完成支付的模式


## 刷卡支付



地址:[环境](/tech)/pay/brushcard




请求参数：
	<table>
		<tr>
		    <th>参数</th>
		    <th>说明</th>
		    <th>类型</th>
		    <th>长度</th>
		    <th>必须</th>      
		    <th>备注</th>
		</tr>
		<tr>
		    <td>mid</td>
		    <td>商户号</td>
		    <td>string</td>
		    <td>15</td>
		    <td>Y</td>
		    <td>商户号 </td>
		</tr>
		<tr>
		    <td>orderNo</td>
		    <td>商户订单号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>商户系统中的订单号 </td>
		</tr>
		<tr>
		    <td>subject</td>
		    <td>订单标题</td>
		    <td>string</td>
		    <td>128</td>
		    <td>Y</td>
		    <td>可放置商品名称 </td>
		</tr>
		<tr>
		    <td>body</td>
		    <td>订单描述</td>
		    <td>string</td>
		    <td>128</td>
		    <td>Y</td>
		    <td>商品的简要描述 </td>
		</tr>
		<tr>
		    <td>amount</td>
		    <td>订单金额</td>
		    <td>number</td>
		    <td>18,2</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<!-- <tr>
		    <td>type</td>
		    <td>支付种类</td>
		    <td>string</td>
		    <td>10</td>
		    <td>Y</td>
		    <td>wechat：微信<br/>alipay：支付宝<br/>QQwallet：QQ钱包 </td>
		</tr> -->
		<tr>
		    <td>deviceInfo</td>
		    <td>终端设备号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>N</td>
		    <td>终端设备号，商户自定义</td>
		</tr>
		<tr>
		    <td>authCode</td>
		    <td>授权码</td>
		    <td>string</td>
		    <td>128</td>
		    <td>Y</td>
		    <td>扫码支付授权码，设备读取用户展示的条码或者二维码信息</td>
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
		    <td>sign</td>
		    <td>签名</td>
		    <td>string</td>
		    <td>128</td>
		    <td>Y</td>
		    <td></td>
		</tr>
	</table>

请求示例:
```
mid=100000510983456&orderNo=M201611101010100002&subject=iphone7&body=iphone7&amount=50.00
&deviceInfo=A001&authCode=1234567890&noise=A2343WER157&sign=567A9D86BD90C09567A9D86BD90C098C
```

响应参数：
	<table>
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
		    <td>参见 返回状态码</td>
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
		    <td></td>
		</tr>
		<tr>
		    <td>errCode</td>
		    <td>错误代码</td>
		    <td>string</td>
		    <td>128</td>
		    <td>N</td>
		    <td>在code为非SUCCESS时存在</td>
		</tr>
		<tr>
		    <td>errCodeDes</td>
		    <td>错误描述</td>
		    <td>string</td>
		    <td>128</td>
		    <td>N</td>
		    <td>在code为非SUCCESS时存在</td>
		</tr>
		<!-- <tr>
		    <td>needQuery</td>
		    <td>查询判断</td>
		    <td>string</td>
		    <td>14</td>
		    <td>N</td>
		    <td>用来判断是否需要调用查询接口，值为Y时需要，值为N时不需要</td>
		</tr> -->
        <tr><td colspan="6">code和resultCode都为SUCCESS时返回</td></tr>
		<tr>
		    <td>orderNo</td>
		    <td>商户订单号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>flowNo</td>
		    <td>平台订单号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>tradeNo</td>
		    <td>第三方订单号</td>
		    <td>string</td>
		    <td>64</td>
		    <td>N</td>
		    <td></td>
		</tr>
		<tr>
		    <td>payStatus</td>
		    <td>支付状态</td>
		    <td>string</td>
		    <td>2</td>
		    <td>Y</td>
		    <td>1：支付成功<br/>2：支付失败<br/>5：待查询<br/>10：已撤销</td>
		</tr>
		<tr>
		    <td>succAmount</td>
		    <td>成功金额</td>
		    <td>number</td>
		    <td>18,2</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>payTime</td>
		    <td>订单支付时间</td>
		    <td>string</td>
		    <td>14</td>
		    <td>Y</td>
		    <td>yyyyMMddHHmmss</td>
		</tr>
	</table>

响应示例：<br/>
```
{
  "code":"SUCCESS",
  "msg":"ok",
  "noise":"WE12GRT58Q",
  "resultCode":"SUCCESS",
  "mid":"100000510983456",
  "orderNo":"M201611101010100002",
  "flowNo ":"4744539234543241111",
  "tradeNo ":"431231233122233",
  "payStatus ":"1",
  "orderAmount ":5230.00,
  "succAmount ":5230.00,
  "payTime":"20161110101015", 
  "sign":"567A9D86BD90C09567A9D86BD90C098C "
}
```

## 刷卡查询 


接口说明：根据商户订单号查询平台的具体订单信息，返回JSON格式数据


地址:[环境](/tech)/query/brushcard



请求参数:

<table>
		<tr>
		    <th>参数</th>
		    <th>说明</th>
		    <th>类型</th>
		    <th>长度</th>
		    <th>必须</th>      
		    <th>备注</th>
		</tr>
		<tr>
		    <td>mid</td>
		    <td>商户号</td>
		    <td>string</td>
		    <td>15</td>
		    <td>Y</td>
		    <td>商户号 </td>
		</tr>
		<tr>
		    <td>orderNo</td>
		    <td>商户订单号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>商户系统中的订单号 </td>
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
		    <td>sign</td>
		    <td>签名</td>
		    <td>string</td>
		    <td>128</td>
		    <td>Y</td>
		    <td></td>
		</tr>
	</table>

请求示例：

```
mid=100000510983456&orderNo=M201611101010100002&noise=A2343WER157
&sign=567A9D86BD90C09567A9D86BD90C098C
```

响应参数：
	<table>
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
		    <td>Y</td>
		    <td></td>
		</tr>
        <tr><td colspan="6">code为SUCCESS时返回</td></tr>
		<tr>
		    <td>resultCode</td>
		    <td>返回状态码</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>参见 返回状态码</td>
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
		    <td></td>
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
		    <td>flowNo</td>
		    <td>平台订单号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>tradeNo</td>
		    <td>第三方订单号</td>
		    <td>string</td>
		    <td>64</td>
		    <td>N</td>
		    <td></td>
		</tr>
		<tr>
		    <td>payStatus</td>
		    <td>支付状态</td>
		    <td>string</td>
		    <td>2</td>
		    <td>Y</td>
		    <td>1：支付成功<br/>2：支付失败<br/>5：待查询</td>
		</tr>
		<tr>
		    <td>orderAmount</td>
		    <td>订单金额</td>
		    <td>number</td>
		    <td>18,2</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>succAmount</td>
		    <td>成功金额</td>
		    <td>number</td>
		    <td>18,2</td>
		    <td>Y</td>
		    <td></td>
		</tr>
		<tr>
		    <td>payTime</td>
		    <td>订单支付时间</td>
		    <td>string</td>
		    <td>14</td>
		    <td>Y</td>
		    <td>yyyyMMddHHmmss</td>
		</tr>
	</table>

响应实例:

```
{
  "code":"SUCCESS",
  "msg":"ok",
  "noise":"WE12GRT58Q",
  "resultCode":"SUCCESS",
  "mid":"100000510983456",
  "orderNo":"M201611101010100002",
  "flowNo ":"4744539234543241111",
  "tradeNo ":"431231233122233",
  "payStatus ":"1",
  "orderAmount ":5230.00,
  "succAmount ":5230.00,
  "payTime":"20161110101015",
  "sign":"567A9D86BD90C09567A9D86BD90C098C "
}
```

## 刷卡撤销 

接口说明：当支付返回失败，或收银系统超时需要取消交易，可以调用该接口。接口逻辑 ： 支付失败的关单，支付成功的撤销支付。注意：5分钟的订单才可以撤销，其他正常支付的单无法调用	。
调用支付接口后请勿立即调用撤销订单接口，建议支付后至少15s后再调用撤销订单接口，返回JSON格式数据


地址:[环境](/tech)/pay/brushcard/revoke


请求参数:
    <table>
	  <tr>
		    <th>参数</th>
		    <th>说明</th>
		    <th>类型</th>
		    <th>长度</th>
		    <th>必须</th>      
		    <th>备注</th>
		</tr>
		<tr>
		    <td>mid</td>
		    <td>商户号</td>
		    <td>string</td>
		    <td>15</td>
		    <td>Y</td>
		    <td>商户号 </td>
		</tr>
		<tr>
		    <td>orderNo</td>
		    <td>商户订单号</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>商户系统中的订单号 </td>
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
		    <td>sign</td>
		    <td>签名</td>
		    <td>string</td>
		    <td>128</td>
		    <td>Y</td>
		    <td></td>
		</tr>
	</table>

请求示例：
```
mid=100000510983456&orderNo=M201611101010100002&noise=A2343WER157&sign=567A9D86BD90C09567A9D86BD90C098C
```

响应参数:
	<table>
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
		    <td>Y</td>
		    <td></td>
		</tr>
        <tr><td colspan="6">code为SUCCESS时返回</td></tr>
		<tr>
		    <td>resultCode</td>
		    <td>返回状态码</td>
		    <td>string</td>
		    <td>32</td>
		    <td>Y</td>
		    <td>参见 返回状态码</td>
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
		    <td></td>
		</tr>
        <tr><td colspan="6">resultCode为SUCCESS时返回</td></tr>
		<tr>
		    <td>status</td>
		    <td>状态</td>
		    <td>string</td>
		    <td>2</td>
		    <td>Y</td>
		    <td>0：成功<br/>1：失败</td>
		</tr>
	</table>

响应示例：
```
{
  "code":"SUCCESS",
  "msg":"ok",
  "noise":"WE12GRT58Q",
  "resultCode":"SUCCESS",
  "mid":"100000510983456",
  "status ":"0",
  "sign":"567A9D86BD90C09567A9D86BD90C098C "
}
```

# 返回状态码  
    
参见 [返回状态码(错误码)](/error_code)


# 数字签名  
	
 见安全规范-[签名算法](/qianming)