#### 1.M1卡搜卡

```java
//参数1：搜卡超时时间单位秒，参数2：卡片监听
UMPay.getInstance().m1CardSearch(10, new MyListener());
public class MyListener implements UMM1CardCallBack {
	@Override
	public void onReBind(int code, String msg) {
	 //2017.10.17添加，支付插件升级会造成aidl断开绑定，就会回调此方法，需要接入方按照demo重新绑定即可
	}
    @Override
    public void onSuccess(M1Response m1Response) {
        switch (m1Response.code) {
            case M1CardCode.SEARCHSUCCESS:
               //搜卡成功
                break;
            case M1CardCode.AUTHSUCCES:
                //授权成功
                break;
            case M1CardCode.WRITESUCCES:
                //写入成功
                break;
            case M1CardCode.READSUCCES:
                //读成功
                break;
        }
    }
    @Override
    public void onError(M1Response m1Response) {
		//失败信息 包括所有的错误信息
    }
}
```

**【响应】**

`M1Response`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| serialNum  | String  | C  | 卡序列号  |
| readInfo  | String  | C  | 块数据  |
| operaBlk  | String  | C | 操作的块号 包括搜卡，授权，读卡，写卡  |



#### 2.M1卡授权

```java
UMPay.getInstance().m1CardAuth("A", 13, "FFFFFFFFFFFF", serialNum);
```

参数1 授权的秘钥形式   “A” 或 “B”
参数2 授权的块号  0-63之间
参数3 授权的秘钥16进制字符串
参数4 卡的序列号 搜卡成功的时候返回

#### 3.M1卡读卡

```java
//参数1 块号   0-63
UMPay.getInstance().m1CardRead(13);
```

#### 3.M1卡写卡

```java
UMPay.getInstance().m1Cardwrite(13,"FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF ");
```

**关闭界面前需要解除搜卡**
```java
UMPay.getInstance().m1CardStop();
```