**交易列表查询可以参考demo中的[OrderListActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/OrderListActivity.java)类**

```java
OrderListRequest listRequest=new OrderListRequest();
if(!("AL".equals(str_type)||"WX".equals(str_type)||"BK".equals(str_type)||"YL".equals(str_type))){
    str_type="";
}
listRequest.payType=str_type;
listRequest.pageIndex=0;
listRequest.pageSize=10;
orderDate=et_date.getText().toString();
listRequest.orderDate=orderDate;//订单日期
orderId=et_id.getText().toString();
listRequest.orderId=orderId;//订单号

UMPay.getInstance().getOrderList(listRequest, new ListCallBack() {
    @Override
    public void onSuccess(OrderListResponse response) {
              Log.e("查询 onSuccess", FastJsonUtils.toJson(response));
           }
    @Override
    public void onFail(OrderListResponse response) {
              Log.e("查询 onFail", FastJsonUtils.toJson(response));
           }
    @Override
    public void onError(OrderListResponse response) {
               Log.e("查询 onError", FastJsonUtils.toJson(response));
          }
    @Override
    public void onReBind(int code, String msg) {
     }
});

```

**【请求】**


`OrderListRequest`类

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| payType  | String  | O  | AL:支付宝支付<br/>WX:微信支付<br/>BK:银行卡支付<br/>YL:银联二维码支  |
| pageIndex  | int  | M  | 查询的页码 从1开始  |
| pageSize  | int  | M  | 返回条数  |
| orderDate  | String  | M  | 订单日期 如“20181120”  |
| orderState  | String  | M  | 订单状态<br/>1.支付中<br/>2.交易成功（包括支付成功和退款成功）判断响应的refundamt如果refundamt>0说明是退款交易<br/>3.支付失败5.撤销成功 |

**【响应】**

`OrderListResponse`类


| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| message  | String  | M  | 响应信息  |
| code  | int  | M  | 返回码  |
| totalPage  | int  | C  | 总共页数  |
| curPage  | int  | C  | 当前界面  |
| platDate  | String  | C  | 平台日期  |
| transList  | JsonArray  | C  | 订单数据  |

transList

| 字段  | 类型  | 必须  | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| tradeno  | String  | M  | 平台流水号  |
| proxyId  | String  | M  | 集成商ID  |
| submerid  | String  | M  | 商户ID  |
| orderid  | String  | M  | 订单号  |
| orderdate  | String  | M  | 订单日期  |
| goodsid  | String  | M  | 商品编号  |
| goodsinfo  | String  | M  | 商品信息  |
| amount  | String  | M  | 交易金额（分）  |
| refundamt  | String  | M  | 退款金额  |
| intime  | String  | M  | 订单入库时间（时间戳）  |
| modtime  | String  | M  | 订单最近修改时间（时间戳）  |
| paytype  | String  | M  | BK:银行卡交易<br/>WX:微信<br/>AL:支付宝<br/>YL:银联二维码|
| platTime  | String  | M  | 平台时间（HHmmss）  |
| platDate  | String  | M  | 平台下单日期（YYYYMMDD）  |
| posSN  | String  | M  | 交易机器SN  |
| subPayType  | String  | M  | 子支付类型银行卡交易返回<br/>CT磁条卡<br/>IC ic卡插卡<br/>YS 手机非接<br/>YSIC 银行卡非接|
| paySeq  | String  | M  | 参考号（银行卡交易返回）  |
| unionPayMerId  | String  | M  | 商户编号（商户在银联注册的商户号）银行卡交易返回  |
| unionPayPosId  | String  | M  | 机器终端号（银联） 银行卡交易返回  |
| uMerId  | String  | M  | U付商户号 （银行卡交易返回） |
| uPosId  | String  | M  | U付终端号（银行卡交易返回）  |
| bankName  | String  | M  | 发卡行名称（银行卡交易返回）  |
| account  | String  | M  | 银行开卡号（银行卡交易返回）  |
| cardExpiryDate  | String  | M  | 卡过期时间（银行卡交易返回）  |
| uPayTrace  | String  | M  | 凭证号（银行卡交易返回）  |
| batchId  | String  | M  | 批次号（银行卡交易返回）  |
| authCode  | String  | M  | 授权码（银行卡交易返回）  |
| isFree  | String  | M  | 1:双免交易 其他不是（银行卡交易返回）  |
| freeMessage  | String  | M  | 双免说明  |