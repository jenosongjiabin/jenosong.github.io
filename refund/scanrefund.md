**非当天的做的交易想还原交易需要调用这个接口，参照`demo`中[ScanRefundActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/ScanRefundActivity.java)类所需要的字段都是通过支付返回的字段**

> 注意：如果能确认交易和退款是在同一台设备上完成，且支付插件的数据未被清除，则数据可以不上送，支付插件会自动补全batchId、orderDateTime、amount。如果选择全要素上传，则需要上送


```java
RefundRequest request = new RefundRequest();
request.payType = payType;
request.uPayTrace = uPayTrace;//凭证号，交易成功返回
Request.orderDate = orderDate;//交易日期
request.rfAmount = rfAmount;//退款金额
// 以下字段可以选择上送
request.amount = amount;(amount为int类型) //原交易金额
refundRequest.uPayOrderId= uPayOrderId;
refundRequest.orderDateTime = orderDateTime;

UMFintech.getInstance().scanRefund(request, new UMScanRefundCallback () {
@Override
public void onReBind(int code, String msg) {
 //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
}
    @Override
    public void onRefundSuccess(RefundResponse response) {
       //退款成功，订单最终状态
    }

    @Override
    public void onRefundFail(RefundResponse response) {
        //退款失败，订单最终状态
    }

    @Override
    public void onRefundUnknown(RefundResponse response) {
        //退款未知，可以发起退款状态查询
		// UMFintech.getInstance().scanRefundQuery(request, callback);
		//具体逻辑参考 Demo 的RefundStateQueryActivity 类
    }

});
```

**【请求】**

`RefundRequest`

| 字段          | 类型   | 必须 | 描述                                                         |
| ------------- | ------ | ---- | ------------------------------------------------------------ |
| amount        | int    | M    | 退款金额/原交易金额 分，交易成功之后返回的amount             |
| payType       | String | M    | 支付类型（CT、IC、YSIC、YS），微信、支付宝、银联二维码、磁条卡、ic卡、闪付卡，交易成功之后返回的 |
| rfAmount      | String | C    | 退款金额，微信支付宝退款时使用。                             |
| uPayOrderId   | String | C    | 扫码付字段，u付订单号，交易成功的返回                        |
| orderDateTime | String | C    | 扫码付字段，交易日期时间，交易成功的返回  如：1220112334     |

**【响应】**

`RefundResponse`

| 字段           | 类型   | 必须 | 描述                                 |
| -------------- | ------ | ---- | ------------------------------------ |
| message        | String | M    | 响应信息                             |
| code           | int    | M    | 返回码                               |
| orderId        | String | O    | 订单号                               |
| amount         | String | O    | 退款金额  单位：分                   |
| tradeNo        | String | O    | 平台处理流水                         |
| platDate       | String | O    | 平台日期  格式：MMdd                 |
| platTime       | String | O    | 平台时间  格式：HHmmss               |
| uPospDate      | String | O    | POSP日期 ,退款状态查询不会返回此此段 |
| uPospTime      | String | O    | POSP时间 ,退款状态查询不会返回此此段 |
| paySeq         | String | O    | 参考号                               |
| unionPayMerId  | String | O    | 商户编号,商户在银联注册的商户号      |
| unionPayPosId  | String | O    | 终端编号                             |
| uMerId         | String | O    | U付商户号                            |
| uPosId         | String | O    | U付终端号                            |
| bankName       | String | O    | 发卡行                               |
| account        | String | O    | 卡号                                 |
| cardExpiryDate | String | O    | 卡有效期                             |
| uPayTrace      | String | O    | 凭证号                               |
| batchId        | String | O    | 批次号                               |
| stateCode      | String | O    | 后台返回的状态码                     |
| msgToUser      | String | O    | 错误用户提示                         |
| msgDesc        | String | O    | 解释口径                             |

