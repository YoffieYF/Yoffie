# flutter开发中发现的问题

## TextField不会垂直居中的问题 （已解决）（待解决）

UI需要实现如下效果，类似下图  
![image](https://yoffieyf.github.io/Yoffie/image/flutter_01.png)  
发现使用TextField文字不能垂直居中，改使用CupertinoTextField能解决此问题,如下：  

```Dart
CupertinoTextField(
      decoration: BoxDecoration(
        border: Border.all(width: Macros.scale(0), color: Colors.transparent),
      ),
      controller: textEditingController,
      maxLines: 1,
      minLines: 1,
      maxLength: 11,
      cursorHeight: Macros.scale(21),
      enabled: true,
      focusNode: focusNode,
      autofocus: autofocus,
      style: _getTextStyle(),
      cursorColor: Colours.blue_2B9BFE,
      textInputAction: TextInputAction.newline,
      placeholder: hintText,
      placeholderStyle: _getHintStyle(),
      keyboardType: keyboardType,
      textAlign: textAlign,
      padding: EdgeInsets.symmetric(vertical: Macros.scale(50 / 2 / 2)),
      inputFormatters: inputFormatters
          ? [FilteringTextInputFormatter.allow(RegExp("[0-9]"))]
          : null,
    );
```

但是有个新的问题，光标不能垂直居中对齐了（待解决）  

## https证书问题  

开发的时候发现dio的网络请求一直报错误，一直处于网络延时的状态，是因为iOS系统会去下载一个https证书，如果这个证书颁发机构是国外的机构那么会出现app变得卡顿，网络请求一直失败。  
解决方案：服务器端需要更换https证书的颁发机构，可以换成阿里的https证书。

## Android运行报错 (已解决)  

```dart
Error:
Attribute application@label value=(day_fantasy) from AndroidManifest.xml:34:9-36 is also present at [com.github.yyued:SVGAPlayer-Android:2.5.3] AndroidManifest.xml:13:9-41 value=(@string/app_name).
Suggestion: add 'tools:replace="android:label"' to <application> element at AndroidManifest.xml:32:5-53:19 to override.
```

解决方案： 如报错的提示 - Suggestion: add 'tools:replace="android:label"' to <application> element at AndroidManifest.xml:32:5-53:19 to override.  
1.修改文件： android/app/src/main/AndroidManifest.xml  
2.找到： <manifest 标签 并添加： xmlns:tools="http://schemas.android.com/tools"  
3.找到： <application 标签 并添加： tools:replace="android:label"

## Android运行报错 （已经解决)

```dart
Execution failed for task ':app:checkDebugDuplicateClasses'.
[        ] > 1 exception was raised by workers:
[        ]   java.lang.RuntimeException: Duplicate class org.intellij.lang.annotations.Flow found in modules annotations-13.0.jar (org.jetbrains:annotations:13.0) and annotations-java5-15.0.jar (org.jetbrains:annotations-java5:15.0)
[        ]   Duplicate class org.intellij.lang.annotations.Identifier found in modules annotations-13.0.jar (org.jetbrains:annotations:13.0) and annotations-java5-15.0.jar (org.jetbrains:annotations-java5:15.0)
```

解决方案：  
1.找到 android/app/build.gradle 文件
2.在文件最后添加：

```dart
configurations {
    cleanedAnnotations
    compile.exclude group: 'org.jetbrains', module: 'annotations'
}
```

## Android打包问题 (已解决)  

使用 flutter build apk --release 打出来的的包，会出现点击相册
crash。但是使用debug运行或者flutter run --release都不会出现问题。  
加入“真机获取crash并保存crash信息到手机“的代码，打开手机crash文件。  
报错信息如下：

```dart
SUPPORTED_64_BIT_ABIS=[Ljava.lang.String;@93ed4d3
versionCode=1
BOARD=unknown
BOOTLOADER=unknown
TYPE=user
ID=MRA58K
TIME=1528462997000
BRAND=Xiaomi
TAG=Build
SERIAL=IJTCLZLBKFU8V8EY
HARDWARE=mt6797
SUPPORTED_ABIS=[Ljava.lang.String;@2cb1810
CPU_ABI=arm64-v8a
RADIO=unknown
IS_DEBUGGABLE=false
DISPLAY_TYPE=unknown
MANUFACTURER=Xiaomi
SUPPORTED_32_BIT_ABIS=[Ljava.lang.String;@156cf0d
TAGS=release-keys
CPU_ABI2=
UNKNOWN=unknown
USER=builder
FINGERPRINT=Xiaomi/nikel/nikel:6.0/MRA58K/8.4.28:user/release-keys
HOST=c3-miui-ota-bd04.bj
PRODUCT=nikel
versionName=1.0.0
DISPLAY=MRA58K
MODEL=Redmi Note 4
DEVICE=nikel
java.lang.AssertionError: java.lang.NoSuchMethodException: fromValue [int]
	at b.f.a.h.b()
	at b.f.a.h.a()
	at b.f.a.a.a()
	at b.f.a.a.a()
	at com.opensource.svgaplayer.m.f$c.a()
	at com.opensource.svgaplayer.m.f$c.a()
	at com.opensource.svgaplayer.m.b$b.a()
	at com.opensource.svgaplayer.m.b$b.a()
	at com.opensource.svgaplayer.m.g$b.a()
	at com.opensource.svgaplayer.m.g$b.a()
	at com.opensource.svgaplayer.m.d$b.a()
	at com.opensource.svgaplayer.m.d$b.a()
	at b.f.a.e.a()
	at b.f.a.e.a()
	at com.opensource.svgaplayer.g$d.run()
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1113)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:588)
	at java.lang.Thread.run(Thread.java:818)
Caused by: java.lang.NoSuchMethodException: fromValue [int]
	at java.lang.Class.getMethod(Class.java:624)
	at java.lang.Class.getMethod(Class.java:603)
	... 18 more
java.lang.NoSuchMethodException: fromValue [int]
	at java.lang.Class.getMethod(Class.java:624)
	at java.lang.Class.getMethod(Class.java:603)
	at b.f.a.h.b()
	at b.f.a.h.a()
	at b.f.a.a.a()
	at b.f.a.a.a()
	at com.opensource.svgaplayer.m.f$c.a()
	at com.opensource.svgaplayer.m.f$c.a()
	at com.opensource.svgaplayer.m.b$b.a()
	at com.opensource.svgaplayer.m.b$b.a()
	at com.opensource.svgaplayer.m.g$b.a()
	at com.opensource.svgaplayer.m.g$b.a()
	at com.opensource.svgaplayer.m.d$b.a()
	at com.opensource.svgaplayer.m.d$b.a()
	at b.f.a.e.a()
	at b.f.a.e.a()
	at com.opensource.svgaplayer.g$d.run()
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1113)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:588)
	at java.lang.Thread.run(Thread.java:818)
```

初步分析是因为找不到svgaplayer的某个方法，其实这个方法是有的，而且在debug运行的时候是没问题，推测是因为安卓打包过程中会去压缩包体积，导致出现一些问题。 

解决方案：  
flutter build apk --release  --no-shrink 使用此命令不去压缩包体积，就没有问题了。为什么压缩会导致crash这个有待深入研究。