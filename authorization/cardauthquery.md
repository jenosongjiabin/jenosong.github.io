<font color='red'>该接口不可用</font>
**预授权订单状态查询请参考[CardAuthQueryActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/CardAuthQueryActivity.java)**

```java
CreditPreauthQueryRequest request = new CreditPreauthQueryRequest();
request.orderDate = orderDate;
request.orderId = orderId;
UMFintech.getInstance().creditPreauthQuery(request, new UMPreauthQueryCallBack() {
	@Override
	public void onQuerySuccess(CreditPreauthQueryResponse response) {
	//查询成功 但是不代表最终的订单状态 订单状态说明见后面说明
	}
	@Override
	public void onQueryFail(CreditPreauthQueryResponse response) {
	//查询失败
	}
	@Override
	public void onReBind(int code, String msg) {
	//2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
	}
});

```

**【请求】**

`CreditPreauthQueryRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| orderId  | String  | M  | 订单id  |
| orderDate  | String  | M  | 预授权的订单日期  |


`CreditPreauthQueryResponse`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| code  | int  | M  | 返回码  |
| message  | String  | M  | 响应信息  |
| orderId  | String  | C  | 订单id  |
| amount  | String  | C  | 金额（单位：分）  |
| cardPayType  | String  | C  | 银行卡类型，包括磁条卡（CT），IC卡（IC），云闪付、applepay（YS）,银行卡闪付（YSIC）  |
| payType  | String  | C  | 银行卡类型，包括磁条卡（CT），IC卡（IC），闪付卡、云闪付、applepay（YS）  |
| tradeNo  | String  | C  | 平台处理流水  |
| orderDate  | String  | C  | 订单日期（退款的时候使用） 格式：yyyyMMdd  |
| platTime  | String  | C  | 平台时间HHmmss  |
| paySeq  | String  | C  | 参考号（打印小票必要字段）  |
| unionPayMerId  | String  | C  | 商户编号，商户在银联注册的商户号  |
| unionPayPosId  | String  |  C | 终端编号  |
| uMerId  | String  | C  | U付商户号  |
| uPosId  | String  | C  | U付终端号  |
| bankName  | String  | C  | 发卡行  |
| account  | String  | C  | 明文卡号  |
| cardExpiryDate  | String  | C  | 卡有效期  |
| uPayTrace  | String  | C  | 凭证号  |
| batchId  | String  | C  | 批次号  |
| authCode  | String  | C  | 授权码  |
| transState  | String  | C  | INIT  : 支付中<br/>FROZEN_FAIL: 预授权失败<br/>FROZEN_SUCCESS:  预授权成功<br/>FROZEN_CANCLE :  预授权被冲正（可看做预授权失败）<br/>FROZEN_REFUND :  预授权撤销成功<br/>PAY_SUCCESS_CARD :  预授权完成成功<br/>PAY_SUCCESS_NO_CARD :  预授权完成通知成功<br/>PAY_FAIL :  预授权完成失败<br/>PAY_CANCLE :  预授权完成冲正成功<br/>PAY_REFUND :  预授权完成撤销成功
  |