
### 快捷绑卡
> 接口说明：快捷绑卡接口需要对使用该接口的客户的四要素信息(姓名、身份证、手机号、银行卡号)进行绑定与认证

地址:[环境](/tech)/quick/card/bind

> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|               商户号|         string|    15|     Y|  商户号|
|userName|         	姓名|       string|    15|    Y|  持卡人姓名|
|phone|           手机号码|   string|    11|     Y|  用户在银行预留手机号|
|idType|           证件类型|    string |   2|     Y|  (00 身份证(目前只支持))|
|idNo|              商品描述|    string |   18|     Y|  持卡人身份证号|
|cardType|            卡类型|    string|    2|   Y|  (0.储蓄卡， 1.信用卡(暂不支持))|
|cardNo|          银行卡号|string| 19|     Y|  银行卡号|
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
cardNo=6226111111111111111&cardType=01&idNo=100000199111110000&idType=00&mid=822017111034824&noise=07e5114a22dc49bfa13d4a70b2aa9a94&phone=15011111111&userName=张某某&sign=1D7B99608DB53E73E4A65B300FB1BD76
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
        <td>contractId</td>
        <td>绑卡协议号</td>
        <td>string</td>
        <td>32</td>
        <td>N</td>
        <td></td>
    </tr>
</table>

响应示例:
```json
{
    "code": "SUCCESS",
    "contractId": "201801030002051261",
    "mid": "812017112437568",
    "msg": "成功",
    "noise": "otemg16GwpYoEyLrDndxcWJPVcsCzK1H",
    "resultCode": "SUCCESS",
    "sign": "F62760FB2C554B5A91FB56F11EDD7CE0"
}
```

