> 建议： 成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题

扫码付支持微信、支付宝、银联二维码，可以参考demo中的`ScanInitiativeActivity`类

#### 请求收款码展示给客户扫码

```java
ScanPayInitiativeRequest scanPayInitiativeRequest = new ScanPayInitiativeRequest();
scanPayInitiativeRequest.amount = Integer.parseInt(amount);
scanPayInitiativeRequest.tips = "定金"; //tips不传默认为 消费
scanPayInitiativeRequest.payType = payType;
scanPayInitiativeRequest.orderId = orderId;
scanPayInitiativeRequest.isD0 = 1; //传1不启用D0结算 不传默认启用D0
UMFintech.getInstance().scanPayInitiative(scanPayInitiativeRequest, umScanPayInitiativeCallback);
```

**`UMScanPayInitiativeCallback`回调**

```java
private UMScanPayInitiativeCallback umScanPayInitiativeCallback = new UMScanPayInitiativeCallback() {
        @Override
        public void onReBind(int code, String msg) {
            //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
        }

        @Override
        public void onGetQRCodeSuccess(ScanPayInitiativeResponse response) {
            //请求二维码成功
        }

        @Override
        public void onGetQRCodeFail(ScanPayInitiativeResponse response) {
            //请求二维码失败
        }

};

```

**【请求】**

**`ScanPayInitiativeRequest`**

| 字段    | 类型   | 必须 | 描述                                                         |
| ------- | ------ | ---- | ------------------------------------------------------------ |
| amount  | int    | M    | 金额（单位：分）                                             |
| tips    | String | O    | 支付说明，默认为消费                                         |
| payType | String | M    | 支付类型(WX、AL、YL分别对应：微信，支付宝，银联)             |
| orderId | String | O    | 商户订单号。建议商户平台自己维护，  生成规则：商户平台定义：<br/>小于64位的非空字符  插件定义：yyMMddHHmmssSSS+{postusn} |
| isD0    | int    | O    | 是否D0清算 0：是 1：否                                       |

**【响应】**

**`ScanPayInitiativeResponse`**

| 字段      | 类型   | 必须 | 描述                                           |
| --------- | ------ | ---- | ---------------------------------------------- |
| message   | String | M    | 响应信息                                       |
| code      | int    | M    | 返回码                                         |
| uPayTrace | String | O    | 支付凭证号                                     |
| qrcodeUrl | String | O    | 收款码，通过此信息生成二维码                   |
| payType   | String | O    | 支付类型（WX、AL、YL）微信，支付宝，银联二维码 |
| date      | String | O    | 平台日期  格式：MMdd                           |
| amount    | String | O    | 订单金额（单位：分）                           |
| orderId   | String | O    | 订单号                                         |
| time      | String | O    | 平台时间HHmmss                                 |
| stateCode | String | O    | 后台返回的状态码                               |
| msgToUser | String | O    | 错误用户提示                                   |
| msgDesc   | String | O    | 解释口径                                       |