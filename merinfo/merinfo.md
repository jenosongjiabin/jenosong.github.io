#### 1.获取商户信息【SDK版本需要00.00.01.39】

获取商户信息可以参考`demo`中的[MerInfoActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/MerInfoActivity.java)类。

```java
UMFintech.getInstance().getMerInfo(new UMMerInfoCallBack() {
	@Override
	public void onSuccess(BackShopInfo info) {
		//获取成功回调
		Log.e("TAG","onSuccess:"+FastJsonUtils.toJson(info));
		tv_content.setText(FastJsonUtils.toJson(info));
	}
	@Override
	public void onError(BackShopInfo info) {
		//获取失败回调
		Log.e("TAG","onError:"+FastJsonUtils.toJson(info));
		tv_content.setText(FastJsonUtils.toJson(info));
	}
	@Override
	public void onReBind(int code, String msg) {
		//插件升级会造成与插件断开绑定，需要重新绑定
		Log.e("TAG","onReBind  code:"+code+" msg:"+msg);
		reBind(code, msg);
	}
});
```

**【响应】**


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| subMerId  | String  | C  | 商户ID  |
| subMerName  | String  | C  | 商户名  |
| proxyId  | String  | C  | 集成商ID  |
| proxyName  | String  | C  | 集成商名  |
| posSN  | String  | C  | 机器序列号  |

<br/>
#### 2.获取厂商名称

可直接调用，不需要绑定，请参考`GetPosCompanyActivity`类。

```java
UMFintech.getInstance().getPosCompany();
```


<br/>
#### 3.获取POS SN

可直接调用，不需要绑定，请参考`GetSnActivity`类。

```java
UMFintech.getInstance().getPosSn();
```

