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
