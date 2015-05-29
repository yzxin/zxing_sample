#关于本sample
本sample是android系统使用zxing进行条形码、二维码编解码处理的范例，是google提供范例的精简版本，仅保留扫码及二维码生成。本精简版本来源于网络，只是替换zxing的核心库到最新版，并增加了一些注释。
#zxing简介
zxing是google开发的条码、二维码编解码库
#zxing编解码使用
##解码
* 类com.google.zxing.MultiFormatReader为解码器
* 类com.google.zxing.BinaryBitmap为二进制位图，作为图像输入
* 类com.google.zxing.Result为解码结果

示例代码(详细可参见文件com/zxing/decoding/DecodeHandler.java):
```java
multiFormatReader = new MultiFormatReader(); // 构造解码器
multiFormatReader.setHints(hints); // 设置解码hints，主要是设置条码、二维码的类别
Result rawResult = multiFormatReader.decodeWithState(bitmap); // 解码
if (rawResult!=null)
  Log.d(TAG, "Found barcode :" + rawResult.toString()); // 输出解码结果
multiFormatReader.reset(); // 重置解码器，可以重复使用
```
##编码
* 类com.google.zxing.MultiFormatWriter为编码器
* 类com.google.zxing.common.BitMatrix为图像矩阵，保存编码器的编码结果

示例代码(详细可参见文件com/zxing/encoding/EncodingHandler.java):
```java
Hashtable<EncodeHintType, String> hints = new Hashtable<EncodeHintType, String>();  
hints.put(EncodeHintType.CHARACTER_SET, "utf-8"); // 设置字符串编码
BitMatrix matrix = new MultiFormatWriter().encode(str,
	BarcodeFormat.QR_CODE, widthAndHeight, widthAndHeight); // 生成二维码
```
#android应用中使用
本sample实现了扫码及二维码生成功能，其它应用需要使用这些功能，可以将本sample的所有源文件（除主窗口相关外）复制到目标应用中。<br>
以下是主窗口相关源文件：
* com/qrcode/MainActivity.java
* res/layout/main.xml

另外：AndroidManifest.xml文件需要增加本sample中列举的那些权限。
##扫码功能
扫码功能是通过打开com.zxing.activity.CaptureActivity进行扫描，并由onActivityResult获取扫描结果<br>
示例如下（详见com/qrcode/MainActivity.java）：
```java
Intent openCameraIntent = new Intent(MainActivity.this,CaptureActivity.class);
startActivityForResult(openCameraIntent, 0); // 开始扫码
...
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
	super.onActivityResult(requestCode, resultCode, data);

	if (resultCode == RESULT_OK) {
		Bundle bundle = data.getExtras();
		String scanResult = bundle.getString("result"); // 获取扫码结果
		...
	}
}
```
##二维码生成功能
二维码生成功能比较简单，调用com.zxing.encoding.EncodingHandler的createQRCode方法实现。<br>
示例如下（详见com/qrcode/MainActivity.java）：
```java
Bitmap qrCodeBitmap = EncodingHandler.createQRCode(contentString, 350); // 根据字符串contentString生成二维码，二维码宽度为350像素，生成结果为Bitmap位图，可以用来显示
```
