
#    




# 注意事项
```
1.所有的接口参数均使用UTF-8进行编码。
2.考虑到业务发展，对账单字段存在新增的可能 建议开发者在进行接口开发时予以兼容考虑
```

# 文档说明
>为商户提供对账文件下载接口。商户系统通过HTTP方式后台访问账单下载链接，将账单csv文件下载到本地后自行处理。

# 测试账户
>请询问我方相关人员

# 接口规则
## 协议规定

规则|说明
:---|:---
传输方式|为保证交易安全性，建议采用HTTPS传输
提交方式|	采用POST方法提交
字符编码|	统一采用UTF-8字符编码
签名算法|	MD5，后续会兼容SHA1、SHA256、HMAC等。
签名要求|	请求需要校验签名，详细方法请参考安全规范-[签名算法](/qianming)

## 安全规范

安全规范-[签名算法](/qianming)


# API列表
## 下载对账单

地址:[环境](/tech)/merchant/billdownload


```
协议：https
方式: POST
```

参数|说明|类型|长度|必须|备注
---|---|---|---|---|---
mid|商户号|string|15|Y|商户号
billType|账单类型|string|15|Y|DZ:支付流水对账单  DF:代付流水对账单
billDate|账单日期|string|15|Y|格式:yyyyMMdd 三个月之内的账单
noise|随机字符串|string|32|Y|随机字符串，不长于32位
sign|签名|string|128|Y|见 [数字签名规则](/qianming)

请求参数示例:
```json
mid=822017060723976&billType=DZ&billDate=20170820&noise=1503369047198&sign=40FF17558C0C5DF29E51B0F74DC995E8
```

响应:

!> 成功后会以流的形式返回文件,失败则会返回错误原因

失败响应示例:
```json
{
  "code": "SUCCESS", 
  "resultCode": "FAIL", 
  "errCode": "INVALID_DOWNLOAD_TIME", 
  "errCodeDes": "文件下载时间有误"
}
```



# 对账文件说明

### 交易对账文件
***<a href="/open_api/document/DZ2017-11-01.csv" target="_blank">交易对账文件模板下载</a>***
								
列名|说明
---|---
商户订单号|商户订单号
支付种类|01银行卡 02预付费卡 03微信 04支付宝 05百度钱包 06京东支付 07QQ钱包 20账户支付 21网银支付 22	快捷支付
支付方式|01扫码 02刷卡 03公众号 04APP 11H5支付 07借记卡 10贷记卡 05（B2C）API 12（B2C）收银台 13（B2B）API 14（B2B）收银  06快捷支付 08快捷支付(费率)API 09快捷支付(封顶)SDK 15快捷（B2C）收银台 
商户号|商户号
商户名称|商户名称
支付状态|1支付成功 2支付失败
交易金额(元)|交易金额(元)
成功金额(元)|成功金额(元)
商户手续费(元)|商户手续费(元)
支付完成时间|yyyyMMddHHmmss


													

### 代付流水对账单
***<a href="/open_api/document/DF2017-11-01.csv" target="_blank">代付对账文件模板下载</a>***	
							
列名|说明
---|---|
代付订单号|平台代付订单号
来源订单号|来源订单号
商户号|商户号
商户名称|商户名称
代付类型|98普通代付 99垫资代付
开户银行|开户银行
联行号|联行号
银行账户|银行账户
开户人姓名|开户人姓名
代付金额(元)|代付金额(元)
到账金额(元)|到账金额(元)
代付手续费(元)|代付手续费(元)
代付完成时间时间|yyyyMMddHHmmss
状态|3:代付成功 4:代付失败

# SDK&&DEMO下载
见 [SDK与DEM](/SDK与DEMO/SDK与DEMO)













	

	
	
