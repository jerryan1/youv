
### 确认支付
> 接口说明：后台API直联模式；商户自定义页面引导用户输入短信验证码，并通过后台直联方式直接发送至在线支付平台；

地址:[环境](/tech)/quick/order/pay


> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|				商户号|		 string|	15|		Y|	商户号|
|contractNo|			绑卡协议号|	 string|	32|		Y|
|orderNo|			商户订单号|	 string|	32|		Y|	合作伙伴交易号（确保在合作伙伴系统中唯一）|
|amount|	交易金额|number|	13,2|		Y|	交易的总金额，单位为元|
|checkCode|	短信验证码|string|	8|		Y||
|cardType|	卡类型|string|	2|		Y|卡类型，01（储蓄卡）、02（信用卡）|
|noise|				随机字符串|	string|		32|		Y|	随机字符串,不长于32位|
|sign|				签名	|		string|		200|	Y|数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名|

请求示例:
```
amount=0.5&cardType=01&checkCode=204711&contractNo=201712280001919988&mid=812017112437568&noise=be5a62a854294b3d95e6d4796eceb5d3&orderNo=1204029627436491&sign=B7AFD84F5D91AF852A8ADBEACF97EE64
```


> 接口返回

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
        <td>Y</td>
        <td>商户上传的订单号</td>
    </tr>
    <tr>
        <td>orderStatus</td>
        <td>订单状态</td>
        <td>string</td>
        <td>32</td>
        <td>N</td>
        <td>
            CHANNEL_ORDER_SUCCESS:渠道下单成功</br>
            PAYMENT_SUCCESS:支付未明,有可能支付失败,有可能失败</br>
            ORDER_HAD_PAY:订单已支付</br>
            PAYMENT_INIT:初始状态</br>
            PAYING:支付中</br>
            ORDER_FAIL:失败</br>
            UNKOWN:支付未明</br>
        </td>
    </tr>
</table>

返回示例:
```json
{
    "code": "SUCCESS",
    "mid": "812017112437568",
    "msg": "成功",
    "noise": "dKrwG9dpeP6I2uYS2e6zAFk95zprWEdF",
    "orderId": "3051032581709461",
    "orderStatus": "ORDER_HAD_PAY",
    "payOrderId": "ZF20171228122623518201",
    "resultCode": "SUCCESS",
    "sign": "861D0B07D136A96F6D93854EEF91763E"
}
```
