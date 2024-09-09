**用来查询扫码付退款状态未知的情况下来查询退款是否成功，参考`demo`中的RefundStateQueryActivity**


```java
ScanRefundQueryRequest request = new ScanRefundQueryRequest();
request.uPayTrace = uPayTrace.getText().toString().trim();
request.amount = amount.getText().toString().trim();
request.tradeNo = tradeNo.getText().toString().trim();
request.orderDate = "0611";
request.payType = "WX";// "AL" "YL" "WX"
UMFintech.getInstance().scanRefundQuery(request, new UMScanRefundQueryCallback() {
    @Override
    public void onRefundSuccess(ScanRefundQueryResponse response) {
        //退款成功
    }

    @Override
    public void onRefundFail(ScanRefundQueryResponse response) {
        //退款失败
    }

    @Override
    public void onRefundError(ScanRefundQueryResponse response) {
        //查询失败，还不能确定订单最终状态，可以通过错误提示是否继续发起查询
    }

    @Override
    public void onRefundUnknown(ScanRefundQueryResponse response) {
        //查询失败，还不能确定订单最终状态，可以通过错误提示是否继续发起查询
    }

    @Override
    public void onReBind(int code, String msg) {
                
    }
});
```

**【请求】**

`ScanRefundQueryRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| uPayTrace | String  | M  | 凭证号 |
| amount | String  | M  | 原交易金额 |
| orderDate | String | M | 订单日期 （4位 格式：MMdd） |
| payType | String | M | 支付方式 （支付宝："AL" 银联： "YL"微信： "WX"） |
| tradeNo | String | M | 平台流水号 |


**【响应】**

`ScanRefundQueryResponse`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | O  | 订单id  |
| amount  | String  | O  | 退款金额（单位：分）  |
| tradeNo  | String  | O  | 平台处理流水  |
| platDate  | String  | O  | 平台日期 格式：yyyyMMdd  |
| platTime  | String  | O  | 平台时间HHmmss  |
| modDate | String  | O  | 2.0版本添加订单修改日期 |
| modTime | String  |  O | 2.0版本添加订单修改时间 |
| msgDesc | String  | O  | 解释口径 |
| msgToUser | String  | O  | 错误用户提示 |
| stateCode | String  |  O | 后台返回的状态码 |


