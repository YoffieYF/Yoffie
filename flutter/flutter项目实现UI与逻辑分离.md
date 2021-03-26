# Flutter项目实现UI与逻辑分离

实际开发中随着开发的深入，功能会一点点的变多，如果UI代码与逻辑操作代码都写在一起，势必会造成一个文件的代码越来越多。这个时候就需要将一些逻辑代码抽离出来。

## 如何抽离

使用provider进行抽离。每个UI都会有一个provider去管理数据处理与一些用户的交换逻辑。  
例如在实现一个"ETH钱包项目的欢迎页面"的时候。  
功能有：1.创建新用户的入口， 2.根据钱包私钥导入用户，3根据keystore导入用户  
实现方法：可以定义两个文件，一个是welcome_page.dart, 另一个是welcome_page_store.dart。然后利用provider将这两个文件关联起来。  （ps：需要引入 provider: 4.3.2 ）
例如下面的代码:  
welcome_page.dart

```dart
import 'package:dimchat_app/business/tips/password_dialog.dart';
import 'package:dimchat_app/business/welcome/welcome_page_store.dart';
import 'package:dimchat_app/commom/utils/colours.dart';
import 'package:dimchat_app/commom/utils/dimens.dart';
import 'package:dimchat_app/commom/utils/image_utils.dart';
import 'package:dimchat_app/commom/utils/macros.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

class WelcomePage extends StatefulWidget {
  @override
  _WelcomePageState createState() => _WelcomePageState();
}

class _WelcomePageState extends State<WelcomePage> {
  WelcomePageStore _welcomePageStore;

  @override
  void initState() {
    _welcomePageStore = WelcomePageStore(context: context);
    super.initState();
  }


  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider.value(
        value: _welcomePageStore,
        child: Consumer<WelcomePageStore>(builder: (BuildContext context,
            WelcomePageStore welcomePageStore, Widget child) {
          return Scaffold(
            backgroundColor: Colors.white,
            body: Column(
              children: [
                Container(
                  width: Macros.screenWidthPX(),
                  child: Stack(
                    children: [
                      Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          loadAssetImage("welcome/bg_top_left"),
                          Align(
                            alignment: Alignment.center,
                            child:  loadAssetImage('welcome/mid_icon'),
                          ),
                          Container(
                            margin: EdgeInsets.only(left: Dimens.gap_dp85),
                            child: loadAssetImage("welcome/bg_center"),
                          )
                        ],
                      ),
                      Positioned(right: 0, child: loadAssetImage("welcome/bg_right"),)
                    ],
                  ),
                ),
                GestureDetector(
                  onTap: () {
                    _welcomePageStore.createAccount();
                  },
                  child: Container(
                    height: Dimens.gap_dp50,
                    width: Dimens.gap_dp300,
                    padding: EdgeInsets.only(left: Dimens.gap_dp24, right: Dimens.gap_dp22),
                    margin: EdgeInsets.only(bottom: Dimens.gap_dp7, top: Dimens.gap_dp79),
                    decoration: BoxDecoration(
                      color: Colours.bg_btn_f4f4f4,
                      borderRadius: BorderRadius.all(Radius.circular(Dimens.gap_dp50/2))
                    ),
                    child: Stack(
                      children: [
                        Column(
                          mainAxisAlignment: MainAxisAlignment.center,
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text("创建身份", style: TextStyle(fontSize:Dimens.font_sp15, color: Colours.text_blue_2f97f3),),
                            Text("第一次使用钱包", style: TextStyle(fontSize:Dimens.font_sp10, color: Colours.text_gray_999999),)
                          ],
                        ),
                        Align(alignment: Alignment.centerRight, child: loadAssetImage("welcome/enter"))
                      ],
                    ),
                  ),
                ),
                GestureDetector(
                  onTap: () {
                    _welcomePageStore.importUser();
                  },
                  child: Container(
                    height: Dimens.gap_dp50,
                    width: Dimens.gap_dp300,
                    padding: EdgeInsets.only(left: Dimens.gap_dp24, right: Dimens.gap_dp22),
                    decoration: BoxDecoration(
                        color: Colours.bg_btn_f4f4f4,
                        borderRadius: BorderRadius.all(Radius.circular(Dimens.gap_dp50/2))
                    ),
                    child: Stack(
                      children: [
                        Column(
                          mainAxisAlignment: MainAxisAlignment.center,
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text("恢复身份", style: TextStyle(fontSize:Dimens.font_sp15, color: Colours.text_blue_2f97f3),),
                            Text("已拥有钱包", style: TextStyle(fontSize:Dimens.font_sp10, color: Colours.text_gray_999999),)
                          ],
                        ),
                        Align(alignment: Alignment.centerRight, child: loadAssetImage("welcome/enter"))
                      ],
                    ),
                  ),
                ),
              ],
            ),
          );
        }));
  }
}
```

welcome_page_store.dart  

```dart
import 'dart:convert';

import 'package:dimchat_app/business/welcome/import_user_page.dart';
import 'package:dimchat_app/commom/platform/dimchat_plugin/dimchat_platform.dart';
import 'package:dimchat_app/commom/utils/string_utils.dart';
import 'package:dimchat_app/commom/utils/toast.dart';
import 'package:dimchat_app/commom/utils/utils.dart';
import 'package:dimchat_app/model/dim_user_entity.dart';
import 'package:dimchat_app/model/version_entity.dart';
import 'package:flutter/cupertino.dart';
import 'package:dimchat_app/business/welcome/create_account_page.dart';
import 'package:permission_handler/permission_handler.dart';
import 'package:shared_preferences/shared_preferences.dart';

import '../my_tabbar_page.dart';

class WelcomePageStore extends ChangeNotifier {
  BuildContext _context;

  String text;

  WelcomePageStore({BuildContext context}) {
    this._context = context;
  }

  getText() async {
    String ss = await DimchatPlatform.getChannel();
    VersionEntity versionEntity = VersionEntity().fromJson(json.decode(ss));
    text = versionEntity.version;
    notifyListeners();
  }

  createAccount() async{
    Navigator.push(_context, CupertinoPageRoute(
      builder:(context) {
        return CreateAccountPage();
      }
    ));
  }

  importUser() {
    Navigator.push(_context, CupertinoPageRoute(
        builder:(context) {
          return ImportUserPage();
        }
    ));
  }

  recoveryAccount(String psw) async {
    bool hasPermission = await Utils.checkPermission([Permission.storage]);
    if (!hasPermission) {
      hasPermission = await Permission.storage.request().isGranted;
    }
    if (!hasPermission) return;
    if (StringUtils.isNotEmpty(psw)) {
      DimUserEntity dimUserEntity = await DimchatPlatform.pswLogin(psw);
      if (null != dimUserEntity) {
        SharedPreferences sp = await SharedPreferences.getInstance();
        sp.setString("psw", psw);
        Navigator.pushAndRemoveUntil(_context, CupertinoPageRoute(
            settings: RouteSettings(name: "/MyTabbarPage"),
            builder:(context) {
              return MyTabbarPage();
            }
        ), (route) => route == null, );
      } else {
        Toast.show("恢复失败");
      }
    }
  }

}

```

解释一下代码  
1.welcome_page_store.dart 文件： WelcomePageStore extends ChangeNotifier 需要继承ChangeNotifier  

2.welcome_page.dart 文件：需要定义WelcomePageStore _welcomePageStore; 就好比句柄，welcome_page.dart的类手握着welcome_page_store.dart中的类。

3.welcome_page.dart的build方法里面需要使用ChangeNotifierProvider.value的方式去将两个文件关联起来。 

```dart
 ChangeNotifierProvider.value(
        value: _welcomePageStore,
        child: Consumer<WelcomePageStore>(builder: (BuildContext context, WelcomePageStore welcomePageStore, Widget child) {

            }));
```

这样就可以把UI的代码放在welcome_page.dart 文件，一些页面跳转，导入数据的文本处理与一些用户数据的生成都可以放在welcome_page_store.dart 文件。 这样基本就做到了UI与逻辑分离。