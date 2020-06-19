---
title: Flutter 实战从零开始 新闻客户端 - 08 路由管理 auto_route
date: 2020-04-17 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

# 本节目标

- 安装插件
- 路由定义
- 自动生成路由控制类
- 转场动画
- 登录检查中间件
- 带参数传递
- 获取返回值

## 正文

### 一些优秀的路由插件

- [fluro](https://pub.flutter-io.cn/packages/fluro)

  前端的使用体验

  router.navigateTo(context, "/users/1234", transition: TransitionType.fadeIn);

- [flutter_modular](https://pub.flutter-io.cn/packages/flutter_modular)

  功能强大的路由管理：中间件、懒加载、状态管理、动态路由、分组路由、动画、返回值、命名路由

- [auto_route](https://pub.flutter-io.cn/packages/auto_route)

  设计精简、低耦合其它功能

  功能：中间件、自动生成路由代码、动态路由、动画、返回值、命名路由

### 安装插件

- 官网

https://pub.flutter-io.cn/packages/auto_route

- pubspec.yaml

```yaml
dependencies:
  flutter:
    sdk: flutter

  # 路由管理
  auto_route: ^0.4.4

dev_dependencies:
  flutter_test:
    sdk: flutter

  # 路由生成
  auto_route_generator: ^0.4.4
  build_runner:
```

### 路由定义

- lib/common/router/router.dart

```dart
@MaterialAutoRouter()
class $AppRouter {
  @initial
  IndexPage indexPageRoute;

  WelcomePage welcomePageRoute;

  SignInPage signInPageRoute;

  SignUpPage signUpPageRoute;

  ApplicationPage applicationPageRoute;

  DetailsPage detailsPageRoute;
}
```

> 注意 `$` 符号

### 自动生成路由控制类

- 执行命令

```sh
flutter packages pub run build_runner build
```

- 自动生成 lib/common/router/router.gr.dart

```dart
// GENERATED CODE - DO NOT MODIFY BY HAND

// **************************************************************************
// AutoRouteGenerator
// **************************************************************************

import 'package:flutter/material.dart';
import 'package:flutter/cupertino.dart';
import 'package:auto_route/auto_route.dart';
import 'package:flutter_ducafecat_news/pages/index/index.dart';
import 'package:flutter_ducafecat_news/pages/welcome/welcome.dart';
import 'package:flutter_ducafecat_news/pages/sign_in/sign_in.dart';
import 'package:flutter_ducafecat_news/pages/sign_up/sign_up.dart';
import 'package:flutter_ducafecat_news/pages/application/application.dart';
import 'package:flutter_ducafecat_news/common/router/auth_grard.dart';
import 'package:flutter_ducafecat_news/pages/details/details.dart';

abstract class Routes {
  static const indexPageRoute = '/';
  static const welcomePageRoute = '/welcome-page-route';
  static const signInPageRoute = '/sign-in-page-route';
  static const signUpPageRoute = '/sign-up-page-route';
  static const applicationPageRoute = '/application-page-route';
  static const detailsPageRoute = '/details-page-route';
}

class AppRouter extends RouterBase {
  @override
  Map<String, List<Type>> get guardedRoutes => {
        Routes.applicationPageRoute: [AuthGuard],
        Routes.detailsPageRoute: [AuthGuard],
      };

  //This will probably be removed in future versions
  //you should call ExtendedNavigator.ofRouter<Router>() directly
  static ExtendedNavigatorState get navigator =>
      ExtendedNavigator.ofRouter<AppRouter>();

  @override
  Route<dynamic> onGenerateRoute(RouteSettings settings) {
    final args = settings.arguments;
    switch (settings.name) {
      case Routes.indexPageRoute:
        if (hasInvalidArgs<IndexPageArguments>(args)) {
          return misTypedArgsRoute<IndexPageArguments>(args);
        }
        final typedArgs = args as IndexPageArguments ?? IndexPageArguments();
        return MaterialPageRoute<dynamic>(
          builder: (_) => IndexPage(key: typedArgs.key),
          settings: settings,
        );
      case Routes.welcomePageRoute:
        if (hasInvalidArgs<WelcomePageArguments>(args)) {
          return misTypedArgsRoute<WelcomePageArguments>(args);
        }
        final typedArgs =
            args as WelcomePageArguments ?? WelcomePageArguments();
        return MaterialPageRoute<dynamic>(
          builder: (_) => WelcomePage(key: typedArgs.key),
          settings: settings,
        );
      case Routes.signInPageRoute:
        if (hasInvalidArgs<SignInPageArguments>(args)) {
          return misTypedArgsRoute<SignInPageArguments>(args);
        }
        final typedArgs = args as SignInPageArguments ?? SignInPageArguments();
        return MaterialPageRoute<dynamic>(
          builder: (_) => SignInPage(key: typedArgs.key),
          settings: settings,
        );
      case Routes.signUpPageRoute:
        if (hasInvalidArgs<SignUpPageArguments>(args)) {
          return misTypedArgsRoute<SignUpPageArguments>(args);
        }
        final typedArgs = args as SignUpPageArguments ?? SignUpPageArguments();
        return MaterialPageRoute<dynamic>(
          builder: (_) => SignUpPage(key: typedArgs.key),
          settings: settings,
        );
      case Routes.applicationPageRoute:
        if (hasInvalidArgs<ApplicationPageArguments>(args)) {
          return misTypedArgsRoute<ApplicationPageArguments>(args);
        }
        final typedArgs =
            args as ApplicationPageArguments ?? ApplicationPageArguments();
        return MaterialPageRoute<dynamic>(
          builder: (_) => ApplicationPage(key: typedArgs.key),
          settings: settings,
        );
      case Routes.detailsPageRoute:
        if (hasInvalidArgs<DetailsPageArguments>(args)) {
          return misTypedArgsRoute<DetailsPageArguments>(args);
        }
        final typedArgs =
            args as DetailsPageArguments ?? DetailsPageArguments();
        return MaterialPageRoute<dynamic>(
          builder: (_) => DetailsPage(key: typedArgs.key),
          settings: settings,
        );
      default:
        return unknownRoutePage(settings.name);
    }
  }
}

//**************************************************************************
// Arguments holder classes
//***************************************************************************

//IndexPage arguments holder class
class IndexPageArguments {
  final Key key;
  IndexPageArguments({this.key});
}

//WelcomePage arguments holder class
class WelcomePageArguments {
  final Key key;
  WelcomePageArguments({this.key});
}

//SignInPage arguments holder class
class SignInPageArguments {
  final Key key;
  SignInPageArguments({this.key});
}

//SignUpPage arguments holder class
class SignUpPageArguments {
  final Key key;
  SignUpPageArguments({this.key});
}

//ApplicationPage arguments holder class
class ApplicationPageArguments {
  final Key key;
  ApplicationPageArguments({this.key});
}

//DetailsPage arguments holder class
class DetailsPageArguments {
  final Key key;
  DetailsPageArguments({this.key});
}

```

### 路由跳转

- 方式 1：带 context 方式

```dart
ExtendedNavigator.of(context).pushNamed(Routes.signUpPageRoute);
```

- 方式 2：不带 context 方式

```dart
ExtendedNavigator.ofRouter<AppRouter>().pushNamed(Routes.signUpPageRoute);
```

- 方式 3：如果你只有一个导航

```dart
ExtenedNavigator.rootNavigator.pushNamed(Routes.signUpPageRoute);
```

### 转场动画

- lib/common/router/router.dart

```dart
Widget zoomInTransition(BuildContext context, Animation<double> animation,
    Animation<double> secondaryAnimation, Widget child) {
  // you get an animation object and a widget
  // make your own transition
  return ScaleTransition(scale: animation, child: child);
}

@MaterialAutoRouter()
class $AppRouter {
  ...

  @CustomRoute(transitionsBuilder: zoomInTransition)
  ApplicationPage applicationPageRoute;
}
```

- 重新生成

```sh
flutter packages pub run build_runner build
```

### 登录检查中间件

- 创建 lib/common/router/auth_grard.dart

```dart
import 'package:auto_route/auto_route.dart';
import 'package:flutter_ducafecat_news/common/router/router.gr.dart';
import 'package:flutter_ducafecat_news/common/utils/utils.dart';

class AuthGuard extends RouteGuard {
  @override
  Future<bool> canNavigate(ExtendedNavigatorState navigator, String routeName,
      Object arguments) async {
    var isAuth = await isAuthenticated();
    if (isAuth == false) {
      ExtendedNavigator.rootNavigator.pushNamed(Routes.signInPageRoute);
    }

    return isAuth;
  }
}
```

- 注册 lib/main.dart

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'ducafecat.tech',
      debugShowCheckedModeBanner: false,
      builder: ExtendedNavigator<AppRouter>(
        initialRoute: Routes.indexPageRoute,
        router: AppRouter(),
        guards: [AuthGuard()],
      ),
    );
  }
}
```

- 定义 lib/common/router/router.dart

```dart
@MaterialAutoRouter()
class $AppRouter {
  ...

  @GuardedBy([AuthGuard])
  @CustomRoute(transitionsBuilder: zoomInTransition)
  ApplicationPage applicationPageRoute;
}
```

- 重新生成

```sh
flutter packages pub run build_runner build
```

## 参数传递

- 设定初始参数 lib/pages/details/details.dart

```dart
class DetailsPage extends StatefulWidget {
  final String cid;
  DetailsPage({Key key, this.cid}) : super(key: key);
```

- 定义 lib/common/router/router.dart

```dart
@MaterialAutoRouter(generateNavigationHelperExtension: true)
class $AppRouter {
  ...
```

- 重新生成

```sh
flutter packages pub run build_runner build
```

- lib/common/router/router.gr.dart

```dart
//DetailsPage arguments holder class
class DetailsPageArguments {
  final Key key;
  final String cid;
  DetailsPageArguments({this.key, this.cid});
}
```

- 导航参数

```dart
ExtendedNavigator.rootNavigator.pushDetailsPageRoute(cid: '123');
```

- 获取返回值

```dart
    ExtendedNavigator.rootNavigator
        .pushNamed(Routes.signUpPageRoute)
        .then((onValue) {
      print(onValue);
    });
```

## 资源

### 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

### 蓝湖设计稿（加微信给授权 ducafecat）

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

### YAPI 接口管理

http://yapi.demo.qunar.com/

### 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.8

### 参考

https://pub.flutter-io.cn/packages/auto_route

### VSCode 插件

- Flutter、Dart
- [Flutter Widget Snippets](https://marketplace.visualstudio.com/items?itemName=alexisvt.flutter-snippets)
- [Awesome Flutter Snippets](https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets)
- [Paste JSON as Code](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)
- [bloc](https://marketplace.visualstudio.com/items?itemName=FelixAngelov.bloc)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
