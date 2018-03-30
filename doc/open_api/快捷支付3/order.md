
### 下单请求
> 接口说明：后台API直联模式；商户需通过自定义界面获得用户银行卡要素信息，并通过后台直联方式直接发送至在线支付平台；本订单请求接口将获得同步回应的同时，还将向用户的预留手机号发送验证码，还需要通过“确认支付”接口上传短信验证码来最终完成支付。

```
测试环境地址:https://devapi.tfcpay.com/v2/quickPay3/order
生产环境地址:https://api.tfcpay.com/v2/quickPay3/order
协议：https
方式: POST
```

> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|               商户号|         string|    15|     Y|  商户号|
|notifyUrl|         通知URL|       string|    200|    Y|  针对该交易的交易状态异步通知接受URL|
|orderNo|           商户订单号|   string|    32|     Y|  合作伙伴交易号（确保在合作伙伴系统中唯一）|
|subject|           商品名称|    string |   50|     Y|  商品的标题|
|body|              商品描述|    string |   50|     Y|  商品的具体描述|
|amount|            交易金额|    number|    13,2|   Y|  交易的总金额，单位为元|
|bankCode|          付款方银行编码|string| 10|     Y|  银行卡编号(详见- [银行简码](/bankcode))|
|bankCardNo|        付款方银行卡号|string  |   20|     Y|  付款方银行卡号|
|realName|          付款方姓名|   string |   20|     Y|  付款方持卡人姓名，用UTF-8转码|
|phoneNo|           付款方手机号码|string| 11|     Y|  卡在银行预留的手机号码|
|cardType|          卡类标识|    string |   2|     Y|  01（借记卡），02（信用卡）|
|cvv2|               信用卡安全码|       string|     3|      N| 信用卡背后三位数的安全码，信用卡时必填|
|expireDate|       卡有效期|string| 4|      N|  信用卡背后四位有效期（示例：0521），信用卡时必填|
|noise|             随机字符串|  string|     32|     Y|  随机字符串,不长于32位|
|sign|              签名  |       string|     128|    Y|数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名|


请求示例:
```
mid=100000510983456&notifyUrl=https://192.168.1.123:8090/leopard/notify
&orderNo=M201611101010100002&subject=iphone7&body=iphone7&amount=5230.00
&bankCode=102&bankCardNo=6226111111111111111&realName=张某某&phoneNo=15011111111
&cardType=02&cvv2=432&expireDate=2011&noise=A2343WER157&sign=567A9D86BD90C09567A9D86BD90C098C
```


> 响应参数:

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
        <td>SUCCESS/其他;如非SUCCESS见[错误码](/error_code),交易是否成功需要查看 resultCode来判断</td>
    </tr>
    <tr>
        <td>msg</td>
        <td>返回消息</td>
        <td>string</td>
        <td>64</td>
        <td>Y</td>
        <td>错误信息描述</td>
    </tr>
    <tr>
        <td colspan="6">code为SUCCESS时返回</td>
    </tr>
    <tr>
        <td>resultCode</td>
        <td>返回状态码</td>
        <td>string</td>
        <td>32</td>
        <td>Y</td>
        <td>如非SUCCESS 参见 [返回状态码](/error_code)</td>
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
        <td>128</td>
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
    <tr>
        <td colspan="6">resultCode为SUCCESS时返回</td>
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
        <td>orderNo</td>
        <td>商户订单号</td>
        <td>string</td>
        <td>32</td>
        <td>Y</td>
        <td>商户上传的订单号</td>
    </tr>
</table>

响应示例:
```json
{
    "code": "SUCCESS",
    "msg": "下单成功",
    "resultCode": "SUCCESS",
    "noise": "VVNjSCOZLigLhVRJkqTw9SbX7pCWuPOn",
    "mid": "100000510983456",
    "flowNo": "ZF20170923153753574035",
    "orderNo": "27773856447864248773358098914931",
    "sign": "B160816E8FBB9D74EDB3603A1D14C0A5"
}
```

