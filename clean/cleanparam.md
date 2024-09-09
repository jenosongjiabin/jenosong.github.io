> 注意：清除pos参数，清除后参数需要重新下载，数据库会清空，会影响冲正和撤销功能

```java
ClearRequest request = new ClearRequest();
request.action = ClearRequest.Action.ACTION_CLEAR_POS_PARAMS;
UMFintech.getInstance().clearRequest(request, new UMBaseCallback() {

  @Override
  public void onReBind(int code, String msg) {
    reBind(code, msg);
  }
});

```

