
#    




# 注意事项
!> 使用该接口前，请提前向我方运营进行公众号报备，否则无法使用


- 所有涉及到金额的单位**元.角分**
- notifyUrl是平台服务器从后台直接发起请求到商户服务器，商户处理时不能检查用户的cookie或session；
商户更新DB等发货流程需要在notifyUrl完成后，以确保掉单时，平台补单能成功补上
- notifyUrl有可能重复通知，商户需要做去重处理，避免多次发货
- notifyUrl收到的通知，商户处理成功或检查订单已经处理，需要返回处理成功的标志纯字符串success，字符串success不区分大小写；如果我们没
 有收到返回的success，我方服务器继续向你们发送通知，在三小时后将不再通知；
假设所有订单都没有返回success，将增加我们服务器的通知负载，最坏的情况，可能就是导致正常通知商户的通知发生延迟；另外我们会催促你们完善，如果长期不改善，研发或运维技术将对贵司开通的支付接口采取控制措施。
- 文档中请求接口时传的参数，必填为是的参数是必须要传的（如有缺少会报错），必填为否的参数可以传也可以不传
- 返回参数中必填为是的参数是一定会返回的，必填为否的参数则不一定返回，    
- 关于用户openid：在关注者与公众号产生消息交互后，公众号可获得关注者的openid（加密后的微信号，每个用户对每个公众号的openid是唯一的。
对于不同公众号，同一用户的openid不同）获取openid 可参考以下地址:https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140842&token=&lang=zh_CN

使用测试商户号不需要传用户openid；

!> 切换正式的商户号需获取openid切换正式的商户号需获取openid,在请求参数里增加subOpenid字段，并把获取的openid值传给subOpenid
。在切换成正式商户号传subOpenid参数前，需提供正式商户号和公众号（服务号）appid由服务商配置（一般是通过邮件发送配置表），如果没有配置的话，可能会报sub_appid and sub_openid not match、sub_mch_id与sub_appid不匹配等错误，导致无法正常调用接口
- 其它注意事项

**参数大小写问题**
请留意文档中要求的字符大小写问题， 如 “md5 运算后， 字符串的字符要转换为大写” 。

**参数格式问题**
所有传入参数，均为字符串类型，请注意文档中各处的具体要求。

** 时间戳问题**
请使用 Linux 时间戳，注意为字符串格式。精确到秒，不需要到毫秒，即 10 位数字。

** 同一商户订单号支付问题**
商户订单支付失败需要生成新单号重新发起支付，要对原订单号调用关单，避免重复支付；系统下单后，用户支付超时，系统退出     不再受理，避免用户继续，请调用关单接口。
注意：订单生成后不能马上调用关单接口，最短调用时间间隔为5分钟。

# 测试账户

请询问我方相关人员
	


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
```base
mid=100000510983456&noise=RRE1235W&orderNo=M201611101010100002&subject=iphone7&body=iphone7&amount=5230.00&
type=wechat&notifyUrl=https://192.168.1.123:8090/leopard/notify&callbackUrl=https://192.168.1.123:8090/leopard/success&
openid=qw123352ert2&sign=567A9D86BD90C09567A9D86BD90C098C

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
 

## 统一下单接口


地址:[环境](/tech)/pay/jspay/wechat


参数|说明|类型|长度|必须|备注
---|---|---|---|---|---
mid|商户号|string|15|Y|商户号
orderNo|商户订单号|string|32|Y|商户系统中的订单号
subject|订单标题|string|128|Y|可放置商品名称
body|订单描述|string|128|Y|商品的简要描述
amount|订单金额|number|18,2|Y|##.##  (圆.角分)
notifyUrl|通知URL|string|256|Y|接收支付通知的url
tradeType|交易类型|string|16|Y|取值如下：NONJSAPI,JSAPI，NATIVE，APP(微信小程序/微信公众号支付传JSAPI,非原生支付传NONJSAPI)
minipg|是否小程序支付|String|1|N|值为1，表示小程序支付
deviceInfo|设备号|String|32|N|终端设备号
subOpenid|用户openid|string|128|Y|微信用户关注商家公众号的openid
subAppid|公众账号或小程序ID|String|32|Y|当发起公众号支付时，值是微信公众平台基本配置中的AppID(应用ID)；当发起小程序支付时，值是对应小程序的AppID
ip|终端IP|string|16|Y|订单生成的机器 IP
callbackUrl|前台地址|string|256|N|交易完成后跳转的URL，需给绝对路径，255字符内格式如:https://wap.tenpay.com/callback.asp 注:该地址只作为前端页面的一个跳转，需使用notifyUrl通知结果作为支付最终结果。此参数只在非原生态形式下才有效。
goodsTag|商品标记|	String|32|N|商品标记，微信平台配置的商品标记，用于优惠券或者满减使用
timeStart|订单生成时间|string|14|N|订单生成时间，格式为yyyyMMddHHmmss，如2009年12月25日9点10分10秒表示为20091225091010。时区为GMT+8 beijing。该时间取自商户服务器
limitCreditPay|是否限制信用卡|String|32|N|限定用户使用微信支付时能否使用信用卡，值为1，禁用信用卡；值为0或者不传此参数则不禁用
noise|随机字符串|string|32|Y|随机字符串，不长于32位
sign|签名|string|128|Y|见 [数字签名规则](/qianming)


请求示例:
```
mid=100000510983456&noise=RRE1235W&orderNo=M201611101010100002&subject=iphone7&body=iphone7&amount=52.00
&notifyUrl=https://192.168.1.123:8090/leopard/notify&callbackUrl=https://192.168.1.123:8090/leopard/success&
opened=qw123352ert2&sign=567A9D86BD90C09567A9D86BD90C098C
```
响应参数：

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
sign|签名|string|128|Y|[数字签名规则](/qianming)
resultCode为SUCCESS时返回|||||
deviceInfo|设备号|string|32|N|终端设备号
token|动态口令|String|64|N|平台生成的预支付 ID
payUrl|支付地址|String|256|N|非原生支付url
payInfo|原生态js支付信息或小程序支付信息|String|256|N|tradeType为JSAPI时返回


payInfo信息如下
```json
{
appId:"appid",
timeStamp:1414561699,
package:"prepay_id=***",
signType:"MD5",
paySign:"C380BEC2BFD727A4B6845133519F3AD6",
...,
nonceStr:"5K8264ILTKCH16CQ2502SI8ZNMTM67VS"
}
```


## 非原生支付

业务功能

如调用时是用的原生态js支付，此接口可以忽略交互模式

请求：后台请求交互模式
返回结果+通知：后台请求交互模式+后台通知交互模式

请求参数列表:无

请求url：见统一下单接口中返回的payUrl

在服务号中点击这个链接就可调起支付（用户点击页面中的微信支付按钮时实际上就是点击的这个链接，
此种方式需配置固定的支付授权目录：https://pay.swiftpass.cn/pay/ 但不用像原生态jsapi支付那样获取那些参数后续的操作，
测试时可以将组装好的这个链接放到手机微信端文件传输助手点击调起支付界面)

!> 支付授权目录的申请见申请表格中的授权目录列

!> 注：https://pay.swiftpass.cn/pay/jspay 请求地址必须在支持微信支付的公众账号打开，方能调起微信支付，在微信提供的测试公众账号上无法调起支付。


代码示例
```javascript
function onReady(payUrl){
	window.open(payUrl);
}
```


## 微信内H5调起支付

在微信浏览器里面打开H5网页中执行JS调起支付。接口输入输出数据格式为JSON。

注意：WeixinJSBridge内置对象在其他浏览器中无效。
列表中参数名区分大小，大小写错误签名验证会失败。 
getBrandWCPayRequest参数以及返回值定义见网页端接口参数列表，返回列表值说明见网页内支付接口err_msg返回结果值说明。

> 网页端接口参数列表

字段名|变量名|必填|类型|说明
---|---|---|---|---
公众号id|	appId	|是	|String|	对应初始化请求中返回的pay_info中的appId信息
时间戳|	timeStamp	|是	|String	|对应初始化请求中返回的pay_info中的timeStamp信息
随机字符串	|nonceStr	|是|	String	|对应初始化请求中返回的pay_info中的nonceStr信息
订单详情扩展字符串|	package	|是	|String	|对应初始化请求中返回的pay_info中的package信息
签名方式	|signType	|是	|String|	对应初始化请求中返回的pay_info中的signType信息
签名	|paySign	|是|	String	|对应初始化请求中返回的pay_info中的paySign信息


> 网页内支付接口err_msg返回结果值说明

返回值|描述
---|---
get_brand_wcpay_request:ok |支付成功 
get_brand_wcpay_request:cancel| 支付过程中用户取消 
get_brand_wcpay_request:fail |支付失败 


!> 注：JS API的返回结果get_brand_wcpay_request:ok仅在用户成功完成支付时返回。由于前端交互复杂，get_brand_wcpay_request:cancel或者get_brand_wcpay_request:fail可以统一处理为用户遇到错误或者主动放弃，不必细化区分。示例代码如下：

```javascript

function onBridgeReady(){
    WeixinJSBridge.invoke(
        'getBrandWCPayRequest', {
            "appId":"wx2421b1c4370ec43b",     //公众号名称，由商户传入     
            "timeStamp":"1395712654",         //时间戳，自1970年以来的秒数     
            "nonceStr":"e61463f8efa94090b1f366cccfbbb444", //随机串     
            "package":"prepay_id=u802345jgfjsdfgsdg888",     
            "signType":"MD5",         //微信签名方式：     
            "paySign":"70EA570631E4BB79628FBCA90534C63FF7FADD89" //微信签名 
        },
        function(res){     
			// 使用以上方式判断前端返回,微信团队郑重提示：res.err_msg将在用户支付成功后返回ok，但并不保证它绝对可靠。
            if(res.err_msg == "get_brand_wcpay_request:ok" ) {}      
        }
    ); 
 }
if (typeof WeixinJSBridge == "undefined"){
    if( document.addEventListener ){
        document.addEventListener('WeixinJSBridgeReady', onBridgeReady, false);
    }else if (document.attachEvent){
        document.attachEvent('WeixinJSBridgeReady', onBridgeReady); 
        document.attachEvent('onWeixinJSBridgeReady', onBridgeReady);
    }
 }else{
    onBridgeReady();
 } 

```

## 小程序调起支付API (调试中)

小程序调起支付数据签名字段列表：


**调用wx.requestPayment(OBJECT)发起微信支付**

Object参数说明：

描述|变量名|必填|类型|说明
---|---|---|---|---
小程序id|	appId	|是	|String|	微信分配的小程序ID
时间戳|	timeStamp	|是	|String	|对应初始化请求中返回的pay_info中的timeStamp信息
随机字符串	|nonceStr	|是|	String	|对应初始化请求中返回的pay_info中的nonceStr信息
订单详情扩展字符串|	package	|是	|String	|对应初始化请求中返回的pay_info中的package信息
签名方式	|signType	|是	|String|	对应初始化请求中返回的pay_info中的signType信息
签名	|paySign	|是|	String	|对应初始化请求中返回的pay_info中的paySign信息
成功回调|success|	否|Function|接口调用成功的回调函数|
失败回调|fail	|否|Function|接口调用失败的回调函数|
结束回调|complete	|否	|Function|接口调用结束的回调函数（调用成功、失败都会执行）|

回调结果：

回调类型	|errMsg|	说明
---|---|---
success	|requestPayment:ok	|调用支付成功
fail	|requestPayment:fail cancel	|用户取消支付
fail	|requestPayment:fail (detail message)	|调用支付失败，其中 detail message 为后台返回的详细失败原因

**代码示例**
```javascript
示例代码：
wx.requestPayment(
{
'timeStamp': '',
'nonceStr': '',
'package': '',
'signType': 'MD5',
'paySign': '',
'success':function(res){},
'fail':function(res){},
'complete':function(res){}
})
```


## 支付结果通知
 见[支付结果通知](/支付通知/notify)

## 订单查询
见 [订单查询](/订单查询/queryOrder)


# SDK&&DEMO下载
见 [SDK与DEMO](/SDK与DEMO/SDK与DEMO)



