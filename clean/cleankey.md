> 清空pos的密钥信息，会重新下载并装载密钥。

```java
ClearRequest request = new ClearRequest();
request.action = ClearRequest.Action.ACTION_CLEAR_POS_KEY;
UMFintech.getInstance().clearRequest(request, new UMBaseCallback() {

  @Override
  public void onReBind(int code, String msg) {
    reBind(code, msg);
  }
});

```

