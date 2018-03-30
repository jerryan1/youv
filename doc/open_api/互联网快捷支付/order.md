
### 下单请求
> 接口说明：后台API直联模式；商户调用该接口之前需要先进行绑卡操作，下单时需要使用绑卡返回的绑卡协议号进行交易，还需要通过“确认支付”接口上传短信验证码来最终完成支付。

地址:[环境](/tech)/quick/order/create


> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|               商户号|         string|    15|     Y|  商户号|
|contractId|    绑卡协议号|         string|    32|     Y|  绑卡协议号|
|orderNo|           商户订单号|   string|    32|     Y|  合作伙伴交易号（确保在合作伙伴系统中唯一）|
|amount|            交易金额|    number|    13,2|   Y|  交易的总金额，单位为元|
|currency|    交易币种|         string|    32|     Y|  国际通用的3位货币符号,如CNY,USD.目前暂时只支持人民币CNY|
|subject|           商品名称|    string |   50|     Y|  商品的标题|
|body|              商品描述|    string |   50|     Y|  商品的具体描述|
|notifyUrl|         通知URL|       string|    200|    Y|  针对该交易的交易状态异步通知接受URL|
|bankCode|          付款方银行编码|string| 10|     Y|  银行卡编号(详见- [银行简码](/bankcode))|
|cardType|          卡类标识|    string |   2|     Y|  01（借记卡），02（信用卡）|
|noise|             随机字符串|  string|     32|     Y|  随机字符串,不长于32位|
|sign|              签名  |       string|     128|    Y|数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名|


请求示例:
```
amount=0.5&bankCode=310&body=爆米花&cardType=01&contractId=201801030002057692&currency=CNY&mid=812017112437568&noise=e04629cd11ec46928de8776a9c4a19f5&notifyUrl=https://www.baidu.com&orderNo=7662110242900617&subject=手机&sign=B994307A774F6E55DC3C8FA2DCEC8C66
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
        <td>payOrderId</td>
        <td>平台订单号</td>
        <td>string</td>
        <td>32</td>
        <td>N</td>
        <td></td>
    </tr>
    <tr>
        <td>orderId</td>
        <td>商户订单号</td>
        <td>string</td>
        <td>32</td>
        <td>N</td>
        <td>商户上传的订单号</td>
    </tr>
</table>

响应示例:
```json
{
    "code": "SUCCESS",
    "errCode": "CHANNEL_ERROR",
    "errCodeDes": "机构ABC业务34没有支持的路由",
    "mid": "812017112437568",
    "noise": "vbqHIZxtO4BmNynUf8RfkeAgndZTaX5K",
    "resultCode": "ERROR",
    "sign": "5920799E6E089B64B4FC0638D15676B2"
}
```

