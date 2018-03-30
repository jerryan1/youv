

# 注意事项
```
1.所有的接口参数均使用UTF-8进行编码。
2.接口参数中带有签名参数，商户应对数据进行验签，以确认返回的数据未被篡改。
```

# 文档说明
>提供查询银行编码的接口。

# 测试账户
>测试账户信息

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
> 安全规范-[签名算法](/qianming)

## 数据格式
> 查询参数:


# API列表

## 平台支持的银行查询


地址:[环境](/tech)/query/banks

   
```
协议：https
方式: POST
```

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|商户号|string|15|Y|商户号|
|noise|随机字符串|string|32|Y|随机字符串，不长于32位|
|sign|签名|string|128|Y|-|


单笔状态查询参数示例:
```html

mid=100000510983456&noise=12eqweqw432423432423&sign=4A53EDE1F11C7C81DA4970503814E70C

```

> 响应参数:

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
    <td>SUCCESS/FAIL</td>
  </tr>
  <tr>
    <td>msg</td>
    <td>返回消息</td>
    <td>string</td>
    <td>64</td>
    <td>Y</td>
    <td></td>
  </tr>
  <tr>
    <td colspan="6">code位SUCCESS时返回</td>
  </tr>
  <tr>
    <td>resultCode</td>
    <td>返回状态码</td>
    <td>string</td>
    <td>32</td>
    <td>Y</td>
    <td>请参看:参数对照-通知返回码</td>
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
  <tr>
    <td colspan="6">resultCode为SUCCESS时返回</td>
  </tr>
  <tr>
    <td>banks</td>
    <td>银行列表</td>
    <td>list</td>
    <td>-</td>
    <td>N</td>
    <td>内容为JSON</td>
  </tr>
  <tr>
    <td colspan="6">以下为银行列表的内容</td>
  </tr>
  <tr>
    <td>code</td>
    <td>编号</td>
    <td>string</td>
    <td>10</td>
    <td>Y</td>
    <td></td>
  </tr>
  <tr>
    <td>name</td>
    <td>名称</td>
    <td>string</td>
    <td>30</td>
    <td>Y</td>
    <td></td>
  </tr>
</table>


示例:

```json
{
  "code":"SUCCESS",
  "msg":"ok",
  "resultCode":"SUCCESS",
  "noise":"6W5H246V",
  "mid":"100000510983456",
  "sign":"567A9D86BD90C09567A9D86BD90C098C"
  "banks":[
    {"code":"104", "name":"中国银行"},
    {"code":"105", "name":"中国建设银行"}
  ]
}
```

## 商户可用的银行查询


地址:[环境](/tech)/query/merbanks
   
```
协议：https
方式: POST
```

| 参数 | 说明 | 类型 | 长度 | 必须 | 备注 |
|:-----:|:--------:|:------:|:-------:|:------:|:---------------|
|mid|商户号|string|15|Y|商户号|
|bankType|卡类型|string|1|N|0：借记卡、1：贷记卡、2：都支持|
|noise|随机字符串|string|32|Y|随机字符串，不长于32位|
|sign|签名|string|128|Y|-|


单笔状态查询参数示例:
```html

mid=100000510983456&noise=12eqweqw432423432423&sign=4A53EDE1F11C7C81DA4970503814E70C

```

> 响应参数:

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
    <td>SUCCESS/FAIL</td>
  </tr>
  <tr>
    <td>msg</td>
    <td>返回消息</td>
    <td>string</td>
    <td>64</td>
    <td>Y</td>
    <td></td>
  </tr>
  <tr>
    <td colspan="6">code位SUCCESS时返回</td>
  </tr>
  <tr>
    <td>resultCode</td>
    <td>返回状态码</td>
    <td>string</td>
    <td>32</td>
    <td>Y</td>
    <td>请参看:参数对照-通知返回码</td>
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
  <tr>
    <td colspan="6">resultCode为SUCCESS时返回</td>
  </tr>
  <tr>
    <td>banks</td>
    <td>银行列表</td>
    <td>list</td>
    <td>-</td>
    <td>N</td>
    <td>内容为JSON</td>
  </tr>
  <tr>
    <td colspan="6">以下为银行列表的内容</td>
  </tr>
  <tr>
    <td>code</td>
    <td>编号</td>
    <td>string</td>
    <td>10</td>
    <td>Y</td>
    <td></td>
  </tr>
  <tr>
    <td>name</td>
    <td>名称</td>
    <td>string</td>
    <td>30</td>
    <td>Y</td>
    <td></td>
  </tr>
  <tr>
    <td>bankType</td>
    <td>卡类型</td>
    <td>string</td>
    <td>1</td>
    <td>Y</td>
    <td></td>
  </tr>
</table>


示例:

```json
{
  "code":"SUCCESS",
  "msg":"ok",
  "resultCode":"SUCCESS",
  "noise":"6W5H246V",
  "mid":"100000510983456",
  "sign":"567A9D86BD90C09567A9D86BD90C098C"
  "banks":[
    {"code":"104", "name":"中国银行"},
    {"code":"105", "name":"中国建设银行"}
  ]
}
```
