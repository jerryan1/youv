
> 返回状态码(resultCode)


返回状态码 | 描述|解决方案
---|---|----
VERIFY_FAILED | 验签失败|请检查签名算法和签名密钥
LACK_PARAM | 缺少参数|请检查所有必填参数均已上送
NO_MERCHANT | 商户不存在|请检查商户号
INVALID_MERCHANT | 无效商户|商户未通过审核或商户被锁定
NO_ORDER | 订单不存在|请检查订单号
ORDER_CLOSED | 订单已关闭|
WRONG_ORDER | 错误的订单|请检查订单状态
TRADE_LIMITED | 交易受限|被风控限制，请检查交易金额或咨询客服
ORDER_EXIST | 订单已存在|请检查订单号是否重复提交
REFUND_AMT_TOO_LARGE | 退款金额超额|请检查原订单支付金额和已申请退款金额
REFUND_EXIST | 退款单号已存在|请检查否重复退款
REFUND_FAILED | 退款失败|
NO_REFUND_ORDER | 退款的支付订单号不存在|检查订单号
NOT_PROVIDE_REFUND | 不支持退款|
IP_LIMITED | IP受限|
INVALID_PAYTYPE|无效的支付方式|联系运营开通该支付方式
MERCHANT_CHANNEL_ERROR|商户通道信息未关联|联系运营设置商户通道信息
CHANNEL_ERROR|渠道信息获取失败|检查交易的商户是否在报备时报备了本次交易的支付方式，或者本次交易的银行是否是该通道支持的银行
NOT_FOUND_CHANNEL_MID|未获取到通道商户号|联系我方运营
DEFRAY_AMT_TOO_LARGE|订单金额超额|账户余额不足,检查商户号和商户账户余额