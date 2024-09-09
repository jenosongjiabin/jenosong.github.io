> 建议：成功、失败、未知的交易数据都存在接入方的后台，如出现交易失败的情况，便于我们快速排查问题
>
> 注意：<font color='red'>信用卡预授权完成撤销只针对有卡完成的撤销，预授权无卡完成不支持撤销</font>

预授权完成撤销请参考`demo`的[CardAuthDoneReverActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/CardAuthDoneReverActivity.java)类

```java
CardAuthDoneReverRequest request = new CardAuthDoneReverRequest();
request.amount = amount; //预授权完成时的金额(amount为int类型)
request.orderDate = orderDate;//预授权的日期
request.uPayTrace= uPayTrace;//预授完成成功的时候返回的凭证号
request.orderId = orderId;//商户订单号
UMFintech.getInstance().cardPreAuthDoneRever(request, new UMPreAuthDoneReverCallBack() {
  @Override
  public void onReverSuccess(CardAuthDoneReverResponse reverResponse) {
    //预授权完成撤销成功
  }

  @Override
  public void onReverFail(CardAuthDoneReverResponse reverResponse) {

    //预授权完成撤销失败
  }

  @Override
  public void onReverUnknown(CardAuthDoneReverResponse reverResponse) {

  }

  @Override
  public void onProgressUpdate(CardAuthDoneReverResponse response) {

    if (UMCardCode.START_DOWNLOAD_KEY == response.code) {
      //开始下载秘钥
    } else if (UMCardCode.START_READ_CARD == response.code) {
      //秘钥下载成功，开始刷卡
    } else if (UMCardCode.START_READ_CARD == response.code) {
      //刷卡成功，请输入密码

    } else if (CardPreAuthCode.SEND_AUTH_REQUEST == response.code) {
      //密码输入成功，发起预授权撤销
    }
  }

  @Override
  public void onReBind(int code, String msg) {
    //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
  }
});
```


**【请求】**

`CardAuthDoneReverRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| amount  | int  | M  | 金额（单位：分）  |
| orderId  | String  | M  | 订单id 建议商户平台自己维护，生成规则：商户平台定义：小于64位的非空字符插件定义：yyMMddHHmmssSSS+{postusn} |
| orderDate  | String  | M  | 预授权的订单日期  |
| uPayTrace | String  | M  | 预授权完成的凭证号 |


**【响应】**

`CardAuthDoneReverResponse`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| code  | int  | M  | 返回码  |
| message  | String  | M  | 响应信息  |
| orderId  | String  | O | 订单id  |
| amount  | String  | O | 金额（单位：分）  |
| cardPayType  | String  | O | 银行卡类型，包括磁条卡（CT），IC卡（IC），云闪付、applepay（YS）,银行卡闪付（YSIC）  |
| payType  | String  | O | 银行卡类型，包括磁条卡（CT），IC卡（IC），闪付卡、云闪付、applepay（YS）  |
| tradeNo  | String  | O | 平台处理流水  |
| orderDate  | String  | O | 订单日期（退款的时候使用） 格式：yyyyMMdd  |
| platTime  | String  | O | 平台时间HHmmss  |
| paySeq  | String  | O | 参考号（打印小票必要字段）  |
| unionPayMerId  | String  | O | 商户编号，商户在银联注册的商户号  |
| unionPayPosId  | String  | O | 终端编号  |
| uMerId  | String  | O | U付商户号  |
| uPosId  | String  | O | U付终端号  |
| bankName  | String  | O | 发卡行  |
| account  | String  | O | 明文卡号  |
| cardExpiryDate  | String  | O | 卡有效期  |
| uPayTrace  | String  | O | 凭证号  |
| batchId  | String  | O | 批次号  |
| authCode  | String  | O | 授权码  |
| stateCode | String | O | 后台返回的状态码 |
| msgToUser | String | O | 错误用户提示 |
| msgDesc | String | O | 解释口径 |