#zxing简介
zxing是google开发的条码、二维码编解码库
#zxing核心库使用
##解码
类com.google.zxing.MultiFormatReader为解码器
类com.google.zxing.BinaryBitmap为二进制位图，作为图像输入
类com.google.zxing.Result为解码结果
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
类com.google.zxing.MultiFormatWriter为编码器
类com.google.zxing.common.BitMatrix为图像矩阵，保存编码器的编码结果
示例代码(详细可参见文件com/zxing/decoding/DecodeHandler.java):
```java
Hashtable<EncodeHintType, String> hints = new Hashtable<EncodeHintType, String>();  
hints.put(EncodeHintType.CHARACTER_SET, "utf-8"); // 设置字符串编码
BitMatrix matrix = new MultiFormatWriter().encode(str,
	BarcodeFormat.QR_CODE, widthAndHeight, widthAndHeight); // 生成二维码
```
