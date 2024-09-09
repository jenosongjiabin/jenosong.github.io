#### 请求查询客户付款状态

```java
ScanPayStateQueryRequest request = new ScanPayStateQueryRequest();
request.amount = Integer.parseInt(scanPayInitiativeResponse.amount);
request.date = scanPayInitiativeResponse.date;
request.time = scanPayInitiativeResponse.time;
request.upayTrace = scanPayInitiativeResponse.uPayTrace;
request.payType = scanPayInitiativeRequest.payType;
request.orderId = scanPayInitiativeResponse.orderId;
//request.interval = 8; //8秒轮询一次 （根据用户需求自定义，不传默认为8）
//request.repeatCount = 5; //总共轮询5次（根据用户需求自定义，不传默认为5）
//注意：interval和repeatCount参数只是为了提供定制化需求，即通过这两个参数配置后台轮询次数和间隔，可以配置 repeatCount = 1（即该请求只轮询查询一次）， 如果回调走 onPayUnknown方法，接入方可以再次调用查询支付状态，以防止出现后台长时间轮询请求，而Activity已经处于销毁状态的情况
UMFintech.getInstance().scanPayStateQuery(request, umScanPayStateQueryCallback);
```

**`UMScanPayStateQueryCallback`回调**

```java
private UMScanPayStateQueryCallback umScanPayStateQueryCallback = new UMScanPayStateQueryCallback() {
    @Override
    public void onReBind(int code, String msg) {
	    //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }

    @Override
    public void onPaySuccess(ScanPayStateQueryResponse response) {
        //支付成功
    }

    @Override
    public void onPayFail(ScanPayStateQueryResponse response) {
        //支付失败
    }

    @Override
    public void onPayUnknown(ScanPayStateQueryResponse response) {
        //支付状态未知 (在用户配置的轮询次数和间隔之后还是没查到支付状态)
		//可继续发起查询请求
    }

};
```



**【请求】**

**`ScanPayStateQueryRequest`**

| 字段        | 类型   | 必须 | 描述                                             |
| ----------- | ------ | ---- | ------------------------------------------------ |
| amount      | int    | M    | 金额（单位：分）                                 |
| upayTrace   | String | M    | 支付凭证号                                       |
| payType     | String | M    | 支付类型(WX、AL、YL分别对应：微信，支付宝，银联) |
| date        | String | M    | 平台日期  格式：MMdd                             |
| time        | String | M    | 平台时间HHmmss                                   |
| repeatCount | int    | O    | 该请求需要轮询的总次数，默认5次                  |
| interval    | int    | O    | 轮询时间间隔，单位秒，默认8秒                    |
| orderId     | String | M    | 订单id                                           |



**【响应】**

**`ScanPayStateQueryResponse`**

| 字段        | 类型   | 必须 | 描述                                           |
| ----------- | ------ | ---- | ---------------------------------------------- |
| message     | String | M    | 响应信息                                       |
| code        | int    | M    | 返回码                                         |
| uPayTrace   | String | O    | 支付凭证号                                     |
| payType     | String | O    | 支付类型（WX、AL、YL）微信，支付宝，银联二维码 |
| date        | String | O    | 平台日期  格式：MMdd                           |
| payAmount   | String | O    | 支付金额（单位：分）                           |
| orderId     | String | O    | 订单号                                         |
| time        | String | O    | 平台时间HHmmss                                 |
| uPayOrderId | String | O    | U付订单号                                      |
| stateCode   | String | O    | 后台返回的状态码                               |
| msgToUser   | String | O    | 错误用户提示                                   |
| msgDesc     | String | O    | 解释口径                                       |

