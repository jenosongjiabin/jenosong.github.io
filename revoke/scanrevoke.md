**当天的做的交易想还原交易需要调用这个接口，参照`demo`中[ScanRevokeActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/ScanRevokeActivity.java)类所需要的字段都是通过支付返回的字段**

> 注意：如果能确认交易和撤销是在同一台设备上完成，且支付插件的数据未被清除，则以下数据可以不上送，支付插件会自动补全batchId、orderDateTime、amount


```java
RefundRequest request = new RefundRequest();
request.payType = payType;
request.uPayTrace= uPayTrace;
//以下字段可以选择上送
request.amount = amount;(amount为int类型)
request.batchId= batchId;
request.orderDateTime= orderDateTime;

UMFintech.getInstance().scanRevoke (request, new UMScanRefundCallback () {
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

});
```


**【请求】**

`RefundRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 退款金额 分，交易成功之后返回的amount  |
| batchId       | String | O    | 批次号                                                       |
| uPayTrace     | String | M    | 凭证号，交易成功之后返回.                                    |
| orderDateTime | String | O    | 原交易日期时间如：1220112034                                 |
| payType  | String  | M  | 支付类型（CT、IC、YS、YSIC、WX、ZFB、YL）磁条卡、ic卡、闪付卡、微信、支付宝、银联二维码交易成功之后返回的  |


**【响应】**

`RefundResponse`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | O  | 订单id  |
| platDate  | String | O    | 平台日期  格式：MMdd   |
| platTime  | String | O    | 平台时间  格式：HHmmss |
| tradeNo   | String | O    | 订单号                 |
| stateCode | String | O    | 后台返回的状态码       |
| msgToUser | String | O    | 错误用户提示           |
| msgDesc   | String | O    | 解释口径               |
