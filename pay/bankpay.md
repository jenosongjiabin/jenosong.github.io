## **功能说明**

> 建议： 成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题

银行卡支付支持`磁条卡`、`IC卡`、`闪付卡`、`applepay`、`云闪付等`,支持两种支付模式。

* 模式一：支付插件内部一次性完成读卡、用户输入密码、交易上送流程，在此过程中接入方在回调等待最终的交易结果回调即可。

可以参考`demo`中的[CardActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/CardActivity.java)类。

## **接口说明**

> 注意：<font color='red'>为保证流程完整，插件内部会锁定支付流程，当前流程未结束，用户不能发起新的支付请求，如果在模式二中结束上一次请求，需要使用cancelCardPay()结束读卡锁定。</font>

* ###  cardPay() 支付

```java
BankCardPayRequest payRequest = new BankCardPayRequest();
payRequest.amount = amount;//amount为int类型
//订单id (推荐接入方后台生成，保证自己平台唯一,最大长度64位)，下方为测试所用
payRequest.orderId = "0101" + dateTimeFormat.format(new Date());
payRequest.isD0 = 1; //传1不启用D0结算 不传默认启用D0
payRequest.isSplitAccounts = 1; //传1支持分账。不传默认0不支持分账
payRequest.disableDoubleFree = true; //关闭小额双免 默认打开
UMFintech.getInstance().cardPay(payRequest, new UMBankCardPayCallback() {
	@Override
    public void onReBind(int code, String msg) {
        //支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
    }
    @Override
    public void onPaySuccess(BankCardPayResponse response) {
        //支付成功回调，最终订单状态
    }
    @Override
    public void onPayFail(BankCardPayResponse response) {
       //支付失败回调
    }
    @Override
    public void onPayUnknown(BankCardPayResponse response) {
        //支付状态未知回调（由于网络和各种异常有可能会出现这种状态），不能确定订单最终状态，推荐接入平台记录状态为未知，后续可以再次调用银行卡支付状态查询方法，来确定最终状态
        // 需要特殊注意，此情况下一笔交易时会自动冲正，务必让商户在做一次收款或者余额查询。
    }

    @Override
    public void onProgressUpdate(BankCardPayResponse response) {
        //各种流程回调
        if(UMCardCode.START_DOWNLOAD_KEY ==  response.code){
            //开始下载密钥（做银行卡支付需要先下载密钥）
        }else if(UMCardCode.START_READ_CARD ==  response.code){
            //密钥下载成功，开始刷卡！
        }else if(UMCardCode.START_READ_CARD ==  response.code){
            //刷卡成功，请输入密码
        }else if(UMCardCode.SEND_PAY_REQUEST ==  response.code){
            //密码输入成功，发起支付
        }
    }
});
```

* ### stopSearchCard() 取消读卡

  **注意：关闭银行卡界面一定要取消读卡**

```java
UMFintech.getInstance().stopSearchCard(null);
```

**【请求】**

`BankCardPayRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 金额（单位：分） |
| orderId  | String  | M  | 商户订单号。建议商户平台自己维护<br/>生成规则：商户平台定义：小于64位的非空字符<br/>插件定义：yyMMddHHmmssSSS+{postusn} |
| reserve  | String  | O  | 保留字段  |
| isD0 | int | O | 是否D0清算 0：是 1：否 |
| disableDoubleFree | boolean | O | 关闭小额双免 true：关闭 ，false：打开 |
| isSplitAccounts | int | O | 是否支付分账：传1支持分账。不传默认0不支持分账 |



**【响应】**

`BankCardPayResponse`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码 |
| orderId  | String  | C  | 订单id，回传订单号 |
| amount  | String  | C  | 金额（单位：分）  |
| cardPayType  | String  | C  | 银行卡类型，包括磁条卡（swipe），IC卡（ic），applepay（YS）,银行卡闪付（YSIC） |
| payType  | String  | C  | 银行卡类型，包括磁条卡（swipe），IC卡（ic），闪付卡、云闪付、applepay（nfc） |
| tradeNo  | String  | C  | 平台处理流水  |
| orderDate  | String  | C  | 订单日期（退款的时候使用） 格式：yyyyMMdd  |
| platTime  | String  | C  | 平台时间HHmmss  |
| paySeq  | String  | C  | 参考号（打印小票必要字段）  |
| unionPayMerId  | String  | C  | 商户编号，商户在银联注册的商户号  |
| unionPayPosId  | String  |  C | 终端编号  |
| uMerId  | String  | C  | U付商户号  |
| uPosId  | String  | C  | U付终端号  |
| bankName  | String  | C  | 发卡行  |
| account  | String  | C  | 脱敏卡号  |
| uPayTrace  | String  | C  | 凭证号  |
| batchId  | String  | C  | 批次号  |
| authCode  | String  | C  | 授权码  |
| freeAmt  | String  | C  | 小额双免金额，0不是小额双免  |
| freeMessage  | String  | C  | 小额双免提示语，打印小票使用，例如：交易金额未超[1000.00]元，免密免签  |
| cardType  | String  | C  | 卡类型，00:借记卡 01:贷记卡 |
| tradeMerAbbr | String  | C  | 交易商户简称，有此数据时小票打印该字段 |
| stateCode | String  | O | 交易失败时返回，后台返回的状态码 |
| msgToUser | String  | O | 交易失败时返回，错误用户提示 |
| msgDesc | String  | O | 交易失败时返回，解释口径 |
| payAction | String  | O | 1-刷卡，2-插卡，3-挥卡 |
| transCommand | String  | O | 10-消费，11-消费撤销，16-退款 |
| cardMedia | String  | O | 1-刷卡，2-插卡，3-挥卡 |

**【注意】**

(<font color='red'>需要特殊注意，当触发onPayUnknown回调时，下一笔交易时可能会自动冲正，务必让商户在做一次收款或者余额查询</font>)

