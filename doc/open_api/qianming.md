# 数字签名  
!> 数字签名使用MD5签名


> 第一步
```
设所有发送或者接收到的数据为集合M，将集合M内非空参数值的参数按照参数名ASCII码从小到大排序（字典序），使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串stringA。 
```

特别注意以下重要规则： 
- 参数名ASCII码从小到大排序（字典序）； 
- 如果参数的值为空不参与签名； 
- 参数名区分大小写； 
- 验证调用返回或微信主动通知签名时，传送的sign参数不参与签名，将生成的签名与该sign值作校验。 
- 接口可能增加字段，验证签名时必须支持增加的扩展字段 

> 第二步
```
在stringA最后拼接上秘钥得到stringSignTemp字符串
```

**示例参数如下：**

```
mid=123456789098765
orderNo=SC112233445566778899
amount=65.50
currency=CNY
date=20160510
time=152030
notify=https://domain/notify
remark=杯具
```
得到排序后并拼接的字符串+秘钥：
```
amount=65.50&currency=CNY&date=20160510&mid=123456789098765&notify=https://domain/notify&orderNo=SC112233445566778899&remark=杯具&time=152030&ddbax6n4cg8qj958ytt6
```
使用UTF-8转码后生成签名：

将拼接好的字符串进行UTF-8的转码后，用MD5算法计算摘要并转换为16进制，同时转为大写

```2B086E3C763634A869829AE9198F7DAE```


