> 建议： 成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题

扫码付支持微信、支付宝、银联二维码，可以参考`demo`中的[ScanActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/ScanActivity.java)类


```java
ScanPayRequest request = new ScanPayRequest();
request.amount = amount;//amount为int类型
request.mediaNo = result.getText();
request.goodsDescribe = "商品描述商品描述";
request.goodsInfo = "商品信息"
//订单id (推荐接入方后台生成，保证自己平台唯一,最大长度64位)，下方为测试所用
request.orderId = "0101" + dateTimeFormat.format(new Date());
request.isD0 = 1; //传1不启用D0结算 不传默认启用D0
UMFintech.getInstance().scanPay(request, umScanPayCallback);
```

`UmScanPayCallback`回调

```java
private UMScanPayCallback umScanPayCallback = new UMScanPayCallback() {
	@Override
    public void onReBind(int code, String msg) {
        //支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }
    @Override
    public void onPaySuccess(ScanPayResponse response) {
        //支付成功回调，最终订单状态
    }
    @Override
    public void onPayFail(ScanPayResponse response) {
         //支付失败回调，最终订单状态
    }
    @Override
    public void onPayUnknown(ScanPayResponse response) {
        //支付状态未知回调（由于网络和各种异常有可能会出现这种状态),不能确定订单最终状态，推荐接入平台记录状态为未知，后续可以再次调用扫码付状态查询方法，来确定最终状态
	    //UMFintech.getInstance().scanQuery();
      //具体参考 Demo的ScanQueryActivity类
    }
};

```

**【请求】**

`ScanPayRequest`类：

| 字段  | 类型  | 必须  | 描述  |
| :------------ | :------------ | :------------ | :------------ |
| amount  | int  | M  | 金额（单位：分）  |
| mediaNo  | String  | M  | 二维码编号（微信，支付宝，银联二维码）  |
| goodsInfo  | String  | M  | 商品信息(不能超过6个字)  |
| goodsDescribe  | String  | M  | 商品描述（不能超过42个字）  |
| orderId  | String  | M  | 商户订单号 (推荐接入方后台生成，保证自己平台唯一,最大长度64位)  |
| uPayTrace  | String  | O  | 交易凭证号，接入方上送(长度为6位) |
| reserve  | String  | O  | 保留字段  |
| isD0 | int | O | 是否D0清算 0：是 1：否 |




**【响应】**

`ScanPayResponse`类：

| 字段  | 类型  | 必须  | 描述  |
| :------------ | :------------ | :------------ | :------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| orderId  | String  | C  | 订单号  |
| payType  | String  | C  | 支付类型（WX、AL、YL）微信，支付宝，银联二维码  |
| payDate  | String  | C  | 支付日期 格式：yyyyMMdd  |
| payAmount  | String  | C  | 实际支付金额（单位：分）  |
| platTime  | String  | C  | 平台时间HHmmss  |
| uPayTrace  | String  | O  | 支付凭证号  |
| cardType  | String  | O  | 卡类型(00:借记卡，01：贷记卡)  |
| stateCode  | String  | O  | 交易失败时,状态码  |
| msgToUser  | String  | O  | 交易失败时,错误提示  |
| msgDesc  | String  | O  | 交易失败时,解析口径  |


