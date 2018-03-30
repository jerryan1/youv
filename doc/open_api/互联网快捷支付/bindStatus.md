
### 快捷绑卡状态查询
> 接口说明：快捷绑卡查询接口，根据商户的银行卡号查询该卡片的绑卡情况以及绑卡协议号

地址:[环境](/tech)/quick/card/bindStatus


> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|               商户号|         string|    15|     Y|  商户号|
|cardNo|          银行卡号|string| 19|     Y|  银行卡号|
|noise|             随机字符串|  string|     32|     Y|  随机字符串,不长于32位|
|sign|              签名  |       string|     128|    Y|数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名|


请求示例:
```
cardNo=6222220000007777776&mid=812017112437568&noise=7e2cfd8ff73243d8b9fbcf3254f01017&sign=45B42EB56D34357BED525190BF050A8E
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
        <td>如果卡片没有绑定则该字段返回内容为空</td>
    </tr>
    <tr>
        <td>status</td>
        <td>绑定状态</td>
        <td>string</td>
        <td>1</td>
        <td>N</td>
        <td>1 已开通 2 未开通 3 解绑</td>
    </tr>
</table>

响应示例:
```json
{
    "code": "SUCCESS",
    "contractId": "201801030002051261",
    "mid": "812017112437568",
    "msg": "成功",
    "noise": "8X8aslxn0v3BYYpEvVLPAziyksKzgzL0",
    "sign": "5EC599CB42483FCC4610309CA3C5CC11",
    "status": "1"
}
```

