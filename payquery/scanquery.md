**对扫码付(用户被扫)回调`onPayUnknown`的订单调用此方法来查询订单的最终状态，参照`demo`中的`ScanQueryActivity`类**


```java
ScanPayRequest request = new ScanPayRequest();
request.orderId = orderId.getText().toString().trim();
request.payType = payType;
request.amount = Integer.parseInt(amount.getText().toString().trim());
request.paySeq = paySeq.getText().toString().trim();
//订单时间 格式MMdd （由于数据库做的分表，所以需要传这个字段，如果不传就查当月数据）
request.orderDate = orderDate.getText().toString().trim();
progressDialog("正在查询");
UMFintech.getInstance().scanQuery(request, new MyQueryCallBack());
```



**`MyQueryCallBack`回调**

```java
class MyQueryCallBack implements UMScanQueryCallback {
    @Override
    public void onPaySuccess(ScanPayResponse response) {
        //支付成功
    }

    @Override
    public void onPayFail(ScanPayResponse response) {
        cancelDialog();
        //支付失败
    }

    @Override
    public void onPaying(ScanPayResponse response) {
        cancelDialog();
        //支付中 还不能确定订单最终状态，可以通过错误提示是否继续调用查询接口

    }

    @Override
    public void onQueryError(ScanPayResponse response) {
        cancelDialog();
        //查询失败 还不能确定订单最终状态，可以通过错误提示是否继续调用查询接口
    }

    @Override
    public void onReBind(int code, String msg) {
        reBind(code, msg);
    }
}
```



**【请求】**

`ScanPayRequest`类：

| 字段  | 类型  | 必须  | 描述  |
| :------------ | :------------ | :------------ | :------------ |
| amount    | int    | M    | 金额（单位：分）                                             |
| payType   | String | M    | 支付类型编号  WX：微信，  AL：支付宝，  YL：银联二维码       |
| paySeq    | String | M    | 支付流水号(扫码付请求返回的uPayTrace,不能超过6个字)          |
| orderDate | String | M    | 交易日期（不能超过4个字，MMdd）                              |
| orderId   | String | O    | 商户订单号。建议商户平台自己维护，  生成规则：  商户平台定义：小于64位的非空字符  插件定义：yyMMddHHmmssSSS+{postusn} |



**【响应】**

`ScanPayResponse`类：

| 字段  | 类型  | 必须  | 描述  |
| :------------ | :------------ | :------------ | :------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | M  | 订单号  |
| payType  | String  | O  | 支付类型（WX、AL、YL）微信，支付宝，银联二维码  |
| platDate | String | O | 平台日期  格式：MMdd |
| uPayOrderId | String | O | U付订单号 |
| payAmount | String | O | 实际支付金额（单位：分） |
| platTime | String | O | 平台时间HHmmss |
| uPayTrace | String | O | 支付凭证号 |
| cardType | String | O | 卡类型，00借记卡 01贷记卡 |
| stateCode | String | O | 后台返回的状态码 |
| msgToUser | String | O | 错误用户提示 |
| msgDesc | String | O | 解释口径 |