
### 快捷绑卡解绑
> 接口说明：快捷绑卡解绑，解除商户已经绑定的卡，需要使用绑卡协议号进行解绑，解除绑定后该卡无法进行快捷交易，如交易则需再次绑定

地址:[环境](/tech)/quick/card/unbind


> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|               商户号|         string|    15|     Y|  商户号|
|contractNo|          绑卡协议号|string| 32|     Y|  绑卡协议号|
|noise|             随机字符串|  string|     32|     Y|  随机字符串,不长于32位|
|sign|              签名  |       string|     128|    Y|数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名|


请求示例:
```
contractNo=201801030002051261&mid=812017112437568&noise=f83fa771f5134e20b27004386a69fcb5&sign=AA976B1AB42FA5DFDC1C37E182B142EC
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
        <td>unbindStatus</td>
        <td>解绑状态</td>
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
    "errCode": "SUCCESS",
    "mid": "812017112437568",
    "msg": "成功",
    "noise": "E7NMTalBOAsZSDBqi1js5nNninwsn3L5",
    "sign": "DE76495CF479DECB60C535EE21641F2A",
    "unbindStatus": 3
}
```

