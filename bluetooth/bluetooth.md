**蓝牙连接参考[BlueToothActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/BlueToothActivity.java)类**

```java
UMPay.getInstance().connectBluetooth(request, new UMConnectBluetoothCallBack() {
    @Override
    public void onConnectSuccess(ConnectBluetoothResponse response) {
        Log.e("连接成功","连接成功");
    }

    @Override
    public void onConnectError(ConnectBluetoothResponse response) {
        Log.e("连接失败","连接失败");
    }

    @Override
    public void onReBind(int code, String msg) {
        Log.e("重新绑定","重新绑定");
    }
});
```

**【请求】**

`ConnectBluetoothRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| address  | String  | M  | 蓝牙地址  |


**【响应】**

`ConnectBluetoothResponse`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |

