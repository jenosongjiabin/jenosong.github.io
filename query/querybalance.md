银行卡余额查询支持磁条卡、IC卡、闪付卡、applepay、云闪付，可以参考demo中的`BalanceInquiryActivity`类

```java
BaseRequest request = new BaseRequest()
UMFintech.getInstance().balanceInquiry_V2(request,new UMBalanceInquiryCallback() {
  @Override
  public void onReBind(int code, String msg) {
    cancelDialog();
    reBind(code, msg);
  }

  @Override
  public void onSuccess(BalanceInquiryRespnse response) {
    cancelDialog();
    //余额查询成功
  }

  @Override
  public void onFail(BalanceInquiryRespnse response) {
    cancelDialog();
    //查询失败
  }

  @Override
  public void onProgressUpdate(BalanceInquiryRespnse response) {
    tv_info.append("\nonProgressUpdate：" + FastJsonUtils.toJson(response));
    if (UMCardCode.START_DOWNLOAD_KEY == response.code) {
      progressDialog("开始下载密钥！");
    } else if (UMCardCode.START_READ_CARD == response.code) {
      progressDialog("密钥下载成功，开始刷卡！");
    } else if (UMCardCode.PASSWORD_KEYBOARD_OPEN == response.code) {
      progressDialog("刷卡成功，请输入密码");
    } else if (UMCardCode.SEND_BALANCEINQUIRY_REQUEST == response.code) {
      progressDialog("密码输入成功，发起余额查询");
    }
  }
});
```



**【响应】**

`BalanceInquiryRespnse`

| 字段       | 类型   | 必须 | 描述                      |
| ---------- | ------ | ---- | ------------------------- |
| message    | String | M    | 响应信息                  |
| code       | int    | M    | 返回码                    |
| platDate   | String | O    | 平台日期                  |
| platTime   | String | O    | 平台时间HHmmss            |
| bankName   | String | O    | 发卡行                    |
| account    | String | O    | 脱敏卡号                  |
| uPayTrace  | String | O    | 凭证号                    |
| balance    | String | O    | 余额（12位）              |
| subPayType | String | O    | 子支付类型                |
| cardType   | String | O    | 卡类型，00借记卡 01贷记卡 |
| stateCode  | String | O    | 后台返回的状态码          |
| msgToUser  | String | O    | 错误用户提示              |
| msgDesc    | String | O    | 解释口径                  |

