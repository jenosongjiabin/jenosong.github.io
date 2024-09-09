**非当天的做的交易想还原交易需要调用这个接口，参照`demo`中[BankRefundActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/BankRefundActivity.java)类所需要的字段都是通过支付返回的字段**

```java
RefundRequest request = new RefundRequest();
request.orderDate = orderDate;
request.amount = amount;//(amount为int类型)
request.paySeq = paySeq;
request.payType = payType;
request.orderId = orderId;
UMFintech.getInstance().bankRefund(request, new UMRefundCallback () {
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
        //退款未知，需要再起发起退款
    }

	@Override
    public void onProgressUpdate(RefundResponse response) {
        //各种流程回调
        if(UMRefundCode.START_DOWNLOAD_KEY ==  response.code){
            //开始下载密钥
        }else if(UMRefundCode.START_READ_CARD ==  response.code){
            //密钥下载成功，开始刷卡！
        }else if(UMRefundCode.START_READ_CARD ==  response.code){
            //刷卡成功，请输入密码
        }else if(UMRefundCode.SEND_PAY_REQUEST ==  response.code){
            //密码输入成功，发起退款
        }
    }
});
```



**【请求】**

`RefundRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 退款金额 分，交易成功之后返回的amount  |
| orderDate | String | M | 订单日期，交易成功之后返回的  格式：MMdd |
| payType | String | M | 支付类型（CT、IC、YSIC、YS），微信、支付宝、银联二维码、磁条卡、ic卡、闪付卡，交易成功之后返回的 |
| paySeq | String | M | 参考号，交易成功之后返回的 |
| rfAmount | String | C | 退款金额，微信支付宝退款时使用。 |
| uPayOrderId | String | C | 扫码付字段，u付订单号，交易成功的返回 |
| orderDateTime | String | C | 扫码付字段，交易日期时间，交易成功的返回 |
| orderId | String | O | 商户订单号。建议商户平台自己维护，  生成规则：  商户平台定义：小于64位的非空字符  插件定义：yyMMddHHmmssSSS+{postusn} |


**【响应】**

`RefundResponse`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | O  | 订单id  |
| amount  | String  | O | 金额（单位：分）  |
| tradeNo  | String  | O  | 平台处理流水  |
| platDate  | String  | O  | 平台日期 格式：yyyyMMdd  |
| platTime  | String  | O  | 平台时间HHmmss  |
| uPospDate  | String  | O  | POSP日期 ,退款状态查询不会返回此此段  |
| uPospTime  | String  |  O | POSP时间 ,退款状态查询不会返回此此段  |
| paySeq  | String  | O  | 参考号（打印小票必要字段）  |
| unionPayMerId  | String  | O  | 商户编号，商户在银联注册的商户号  |
| unionPayPosId  | String  |  O | 终端编号  |
| uMerId  | String  | O  | U付商户号  |
| uPosId  | String  | O  | U付终端号  |
| bankName  | String  | O  | 发卡行  |
| account  | String  | O  | 脱敏卡号  |
| uPayTrace  | String  | O  | 凭证号  |
| batchId  | String  | O  | 批次号  |
| stateCode | String | O | 后台返回的状态码 |
| msgToUser | String | O | 错误用户提示 |
| msgDesc | String | O | 解释口径 |