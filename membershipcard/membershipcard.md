**获取会员卡号参考[MemberShipCardActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/MemberShipCardActivity.java)类**


```java
UMPay.getInstance().readMemberShipCard(FastJsonUtils.toJson(request),new UMMemberShipCardCallback() {
    @Override
    public void onReBind(int code, String msg) {
        cancelDialog();
        reBind(code, msg);
    }
    @Override
    public void onReadSuccess(MemberShipCardResponse response) {
        info.setText(FastJsonUtils.toJson(response));
    }
    @Override
    public void onReadFail(MemberShipCardResponse response) {
        cancelDialog();
        Log.e("onReadFail", FastJsonUtils.toJson(response));
        info.setText(FastJsonUtils.toJson(response));
    }
});
```


**【请求】**


`MemberShipCardRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| trackType  | int  | M  | 磁道1 磁道2  磁道3  分别传1，2，3  |


**【响应】**

`MemberShipCardResponse`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| cardNo  | String  | C  | 会员卡号  |