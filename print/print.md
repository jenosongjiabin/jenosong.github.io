**参考`demo`中的[PrintActivity](https://github.com/mr-yang/PayPluginDemo/blob/master/app/src/main/java/com/umpay/payplugindemo/PrintActivity.java)类**

#### 1.打印文字

```java
PrintUtils.setStringContent("打印测试--1", 1, 1)
```
**其中使用**

* 参数1：打印的内容
* 参数2：内容显示的位置（1 居左 2 居中  3居右）
* 参数3：打印内容大小（1,2,3）



完整内容如下

```java
JSONArray array=new JSONArray();
array.put(PrintUtils.setStringContent("打印测试--1", 1, 1));
JSONObject ob=new JSONObject();
ob.put(“spos”,array);

```

**注意：**`JSONObject`中的内容中的`key`必须是`"spos"`，下面的方法同样需要。



#### 2.打印二维码

```java
PrintUtils.setTwoDimension("www.baidu.com", 2, 6)
```

**其中使用**

* 参数1 ：打印的内容（二维码内容）
* 参数2 ：内容显示的位置（1 居左 2 居中  3居右）
* 参数3 ：打印内容大小（1-8）



完整内容如下

```java
JSONArray array=new JSONArray();
array.put(PrintUtils.setTwoDimension("www.baidu.com", 2, 6))
JSONObject ob=new JSONObject();
ob.put(“spos”,array);

```





#### 3.打印一维码


```java
PrintUtils.setOneDimension("123123123", 2, 3, 3)
```

**其中使用**

* 参数1 ：打印的内容（一维码内容）
* 参数2 ：内容显示的位置（1 居左 2 居中  3居右）
* 参数3 ：一维码高度（2或3）
* 参数4 ：一维码宽度 （2或3）



完整内容如下

```java
JSONArray array=new JSONArray();
array.put(PrintUtils.setOneDimension("123123123", 2, 3, 3))
JSONObject ob=new JSONObject();
ob.put(“spos”,array);

```





#### 4.打印空行

```java
PrintUtils.setfreeLine("3")
```
**其中使用**

* 参数1 ：打印就空行个数 例如传入“3”则打印3个空行



完整内容如下

```java
JSONArray array=new JSONArray();
array.put(PrintUtils.setfreeLine("3"))
JSONObject ob=new JSONObject();
ob.put(“spos”,array);
```



#### 5.打印分割线

```java
PrintUtils.setLine()
```

完整内容如下

```java
JSONArray array=new JSONArray();
array.put(PrintUtils.setLine())
JSONObject ob=new JSONObject();
ob.put(“spos”,array);
```

> 温馨提示: 打印的分割线使用的默认字体“simsun”分割线的长度和具体使用的字体 建议根据实际情况 使用打印文字的方式自己打印分割线





#### 6.打印图片

```java
PrintUtils.setbitmap(1)
```

* 参数：图片显示的位置（1 偏左，2居中，3偏右）

完整内容如下

```java
SONArray array=new JSONArray();
array.put(PrintUtils.setbitmap(1))
JSONObject ob=new JSONObject();
ob.put(“spos”,array);
```
> 注意：打印图片的时SD卡中必须有这张图片  而且在执行打印的方法中 第二个参数要传入所要打印的图片的地址例如（/storage/emulated/0/jpg1.bmp），6.0权限需要自己应用处理





#### 7.打印复杂的内容

```java
JSONObject jsonObject = new JSONObject();
JSONArray jsonArray = new JSONArray();
try {
	//打印文字
	jsonArray.put(PrintUtils.setStringContent("打印测试--1", 1, 1));
	jsonArray.put(PrintUtils.setStringContent("打印测试--2", 1, 2));
	jsonArray.put(PrintUtils.setStringContent("下面是分割线--", 1, 3));
	//打印图片
	jsonArray.put(PrintUtils.setbitmap(1))
	jsonArray.put(PrintUtils.setLine());
	//打印空行
	jsonArray.put(PrintUtils.setfreeLine("1"));
	//打印图片
	jsonArray.put(PrintUtils.setbitmap(2))
	jsonArray.put(PrintUtils.setStringContent("--------------------------------", 2, 1));
	jsonArray.put(PrintUtils.setStringContent("打印测试--4", 1, 2));
	jsonArray.put(PrintUtils.setTwoDimension("www.baidu.com",2, 6));
	jsonArray.put(PrintUtils.setOneDimension("WWW.BAIDU.COM",2, 2, 2));
	jsonObject.put("spos", jsonArray);
} catch (JSONException e) {
e.printStackTrace();
}
```



### 8.执行打印方法

> 打印图片的本地地址（在本地文件中）如：/storage/emulated/0/jpg1.bmp 多张图片用：“|”分隔 如/storage/emulated/0/jpg1.bmp| /storage/emulated/0/jpg1.bmp


```java
PrintInfo info = new PrintInfo();
info.imagePath = imagePATH;//打印的图片路径
info.fontType = fontType;    //字体
info.lineSpace = space;      //行间距
info.printGary = printGray;   //打印机灰度 （int 类型0-10）
UMFintech.getInstance().print(ob.toString(), info , new BasePrintCallback() {
	@Override
	public void onReBind(int code, String msg) {
	//绑定断开需要重新绑定

	}
	@Override
	public void onStart() {
	//打印开始的回调
	}
	@Override
	public void onFinish() {
	//打印结束的回调
	//如果需要打印多张小票 请在前一张打印完之后执行。
	}
	@Override
	public void onError(int i, String s) {
	//打印异常回调
	}
});
```

**其中使用**

* 参数1：上面组装的打印的JSONobject 的字符串
* 参数2： 打印机配置
* 参数3：aar 中提供的打印回调 （监听了 打印开始  打印结束  打印异常 ）



