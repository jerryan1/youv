

# 注意事项
> 一、wap支付不支持微信内置浏览器调用.如果要微信内置浏览器里实现微信支付，必须要用公众号支付接口，微信对各个不同的使用场景有不同的接口限制规则

> 二、支付确认提示窗体，便于用户操作体验.

> 三、因微信限制，苹果手机支付完成按右上角返回不能跳到相应的callback_url页面，必须按苹果自带的左上角返回应用才能跳转，Android手机是可以正常返回的

> 四、一定要处理好后台异步通知，支付结果是以异步通知为判断依据

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
amount=0.02&body=订单描述&callbackUrl=https://www.baidu.com&deviceInfo=AND_WAP&goodsTag=goodsTag&ip=127.0.0.1&mchAppId=https://pay.weixin.qq.com&mchAppName=微信支付官网&mid=812017071924074&notifyUrl=/shopDemo/shopDemo/notify&orderNo=83410852683676152939591617532343&subject=订单标题&timeExpire=20191227091010&sign=9D6F1EB64A0A05A18279B192D84AC37C
```

**返回数据**

```json
{
  "code": "SUCCESS", 
  "mid": "812017071924074", 
  "noise": "mziK72ZKbZcoxAgvz1FTnmrKDYZY6ZZh", 
  "payInfo": "https://statecheck.swiftpass.cn/pay/wappay?token_id=16a286070d2ab6a85b4e120ccadb07474&service=pay.weixin.wappayv3", 
  "resultCode": "SUCCESS", 
  "sign": "DCA3180F537F2DDED44D7B8D0D3E760D"
}
```

返回有code参数，SUCCESS表示调用成功；非SUCCESS表示调用失败。


# API列表
 

## 下单请求

地址:[环境](/tech)/pay/wap/wechat


参数|说明|类型|长度|必须|备注
---|---|---|---|---|---
mid|商户号|string|15|Y|商户号
orderNo|商户订单号|string|32|Y|商户系统中的订单号
subject|订单标题|string|128|Y|可放置商品名称
goodsTag|商品标记|string|32|N|商品标记
body|订单描述|string|128|Y|商品的简要描述
amount|订单金额|number|18,2|Y|##.##  (圆.角分)
notifyUrl|通知URL|string|256|Y|接收支付通知的url
ip|终端IP|string|16|Y|订单生成的机器 IP
timeExpire|订单超时时间|string|14|N|订单失效时间， 格式为 yyyyMMddHHmmss，<br/>如2009 年 12 月 27 日 9 点 10 分 10 秒表示为20091227091010。<br/> 时区为 GMT+8 beijing。 该时间取自商户服务器
deviceInfo|应用类型|string|16|Y|iOS_SDK,AND_SDK,iOS_WAP,AND_WAP<br/> 暂时只支持iOS_WAP/AND_WAP
callbackUrl|前台地址|string|256|N|交易完成后跳转的 URL，需给绝对路径，255字符内格式 <br/>如:https://wap.tenpay.com/callback.asp <br/>注:该地址只作为前端页面的一个跳转，须使用 notify_url 通知结果作为支付最终结果。
mchAppId|应用标识|string|256|Y|iOS_SDK ： IOS 应 用 唯 一 标 识 (如：com.tencent.wzryIOS) <br/> AND_SDK: 应用在一台设备上的唯一标识,manifest 文 件 里 面 声 明 (如：com.tencent.tmgp.sgame) <br/> iOS_WAP/AND_WAP:WAP首页URL 地址,必须保证公网能正常访问(如https://m.jd.com)   
mchAppName|应用名|string|256|Y|iOS_SDK： 应用在 AppStore 中的唯一应用名（如：王者荣耀）<br/>AND_SDK: 应用在安桌分发市场中的应用名（如：王者荣耀）iOS_WAP / AND_WAP:WAP 网站名(如： 京东官网)         
noise|随机字符串|string|32|Y|随机字符串，不长于32位
sign|签名|string|128|Y|见 [数字签名规则](/qianming)


请求示例:
```
amount=0.02&body=订单描述&callbackUrl=https://www.baidu.com&deviceInfo=AND_WAP&goodsTag=goodsTag&ip=127.0.0.1&mchAppId=https://pay.weixin.qq.com&mchAppName=微信支付官网&mid=812017071924074&notifyUrl=/shopDemo/shopDemo/notify&orderNo=83410852683676152939591617532343&subject=订单标题&timeExpire=20191227091010&sign=9D6F1EB64A0A05A18279B192D84AC37C
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
		    <td>errCode</td>
		    <td>错误代码</td>
		    <td>string</td>
		    <td>128</td>
		    <td>N</td>
		    <td>在code为非SUCCESS时存在</td>
		</tr>
		<tr>
		    <td>errCodeDes</td>
		    <td>错误代码描述</td>
		    <td>string</td>
		    <td>128</td>
		    <td>N</td>
		    <td>在code为非SUCCESS时存在</td>
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
		    <td>payInfo</td>
		    <td>支付链接</td>
		    <td>string</td>
		    <td>1024</td>
		    <td>Y</td>
		    <td>唤起手机微信支付url地址</td>
		</tr>
	</table>

响应示例：<br/>
```
{
  "code": "SUCCESS", 
  "mid": "812017071924074", 
  "noise": "8ZL3OBMs72XVl3nTTiabMfaQ88KasFHH", 
  "payInfo": "https://statecheck.swiftpass.cn/pay/wappay?token_id=1b860b72bac0bc469c0c368a349677c89&service=pay.weixin.wappayv3", 
  "resultCode": "SUCCESS", 
  "sign": "0F38F2719567D79C62DAD377DD06FBD5"
}
```


## 支付通知
 见[支付结果通知](/支付通知/notify)

## 支付查询
见 [订单查询](/订单查询/queryOrder)


# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)



