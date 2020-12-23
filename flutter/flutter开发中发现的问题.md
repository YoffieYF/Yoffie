# flutter开发中发现的问题

## TextField不会垂直居中的问题 （已解决）（待解决）
UI需要实现如下效果，类似下图  
![image](https://yoffieyf.github.io/Yoffie/image/flutter_01.png)  
发现使用TextField文字不能垂直居中，改使用CupertinoTextField能解决此问题,如下：  
````
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