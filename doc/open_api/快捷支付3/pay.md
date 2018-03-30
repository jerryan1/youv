
### 确认支付
> 接口说明：后台API直联模式；商户自定义页面引导用户输入短信验证码，并通过后台直联方式直接发送至在线支付平台；

```
测试环境地址:https://devapi.tfcpay.com/v2/quickPay3/pay
生产环境地址:https://api.tfcpay.com/v2/quickPay3/pay
协议：https
方式: POST
```

> 请求参数:

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|				商户号|		 string|	15|		Y|	商户号|
|orderNo|			商户订单号|	 string|	32|		Y|	合作伙伴交易号（确保在合作伙伴系统中唯一）|
|phoneNo|	付款方手机号码|string|	11|		Y|	付款方手机号码|
|smsCode|	短信验证码|string|	10|		Y||
|noise|				随机字符串|	string|		32|		Y|	随机字符串,不长于32位|
|sign|				签名	|		string|		200|	Y|数据的加密校验字符串，目前只支持使用MD5签名算法对待签名数据进行签名|

请求示例:
```
mid=100000510983456&orderNo=M201611101010100002&phoneNo=15011111111
&smsCode=983456&noise=A2343WER157&sign=567A9D86BD90C09567A9D86BD90C098C
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

返回示例:
```json
{
    "code": "SUCCESS",
    "msg": "ok",
    "resultCode": "SUCCESS",
    "noise": "VVNjSCOZLigLhVRJkqTw9SbX7pCWuPOn",
    "mid": "100000510983456",
    "flowNo": "ZF20170923153753574035",
    "orderNo": "27773856447864248773358098914931",
    "sign": "B160816E8FBB9D74EDB3603A1D14C0A5"
}
```
