---
title: Flutter Getx 01 - 路由、中间件、鉴权、传值、跳转
date: 2021-4-5 00:00:00
tags: flutter getx
categories: Flutter Getx
---

![](2021-04-04-22-50-24.png)

# Getx

![](2021-04-04-21-09-42.png)

![](2021-04-05-13-44-20.png)

https://pub.flutter-io.cn/packages/get

[vscode 插件](https://marketplace.visualstudio.com/items?itemName=get-snippets.get-snippets)

[Android Studio/Intellij 插件](https://plugins.jetbrains.com/plugin/14975-getx-snippets)

# 本节目标

- GetPage 对象
- 路由层级控制
- 路由中间件、鉴权
- 404 处理
- 路由跳转、替换、清除
- 路由传值、返回值
- 路由转场动画

## 开发环境

- Flutter 2.1.0-12.1.pre
- Dart 2.13.0
- get: ^3.26.0

## 参考

- [getx example](https://github.com/jonataslaw/getx/tree/master/example)
- [getx_pattern](https://kauemurakami.github.io/getx_pattern/)
- [GetX Snippets](https://marketplace.visualstudio.com/items?itemName=get-snippets.get-snippets)

## 视频

https://www.bilibili.com/video/BV1yU4y1876r/

## 代码

https://github.com/ducafecat/getx_quick_start

## 正文

### 初始 getx 项目

- pubspec.yaml

```yaml
dependencies:
  ...
  get: ^3.26.0
```

- lib/pages/home/index.dart

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomeView extends StatelessWidget {
  const HomeView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("首页"),
      ),
      body: ListView(
        children: [
          // 路由&导航
          Divider(),

        ],
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
part of 'app_pages.dart';

abstract class AppRoutes {
  static const Home = '/home';
}
```

- lib/common/routes/app_pages.dart

```dart
import 'package:get/get.dart';

part 'app_routes.dart';

class AppPages {
  static const INITIAL = AppRoutes.Home;

  static final routes = [
    GetPage(
      name: AppRoutes.Home,
      page: () => HomeView(),
    ),
  ];
}
```

- lib/main.dart

```dart
Future<void> main() async {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      debugShowCheckedModeBanner: false,
      initialRoute: AppPages.INITIAL,
      getPages: AppPages.routes,
    );
  }
}
```

### 编写 GetPage 定义

- lib/pages/list/index.dart

```dart
class ListView extends StatelessWidget {
  const ListView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("列表页"),
      ),
    );
  }
}
```

- lib/pages/list_detail/index.dart

```dart
class DetailView extends StatelessWidget {
  const DetailView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("详情页"),
      ),
      body: ListView(
        children: [
          ListTile(
            title: Text("导航-返回"),
            subtitle: Text('Get.back()'),
            onTap: () => Get.back(),
          ),
        ],
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
abstract class AppRoutes {
  static const Home = '/home';
  static const List = '/list';
  static const Detail = '/detail';
```

- lib/common/routes/app_pages.dart

```dart
    GetPage(
      name: AppRoutes.Home,
      page: () => HomeView(),
      children: [
        GetPage(
          name: AppRoutes.List,
          page: () => ListView(),
          children: [
            GetPage(
              name: AppRoutes.Detail,
              page: () => DetailView(),
            ),
          ],
        ),
      ],
    ),
```

### 导航操作 命名、视图对象

- lib/pages/home/index.dart

```dart
  ListTile(
    title: Text("导航-命名路由 home > list"),
    subtitle: Text('Get.toNamed("/home/list")'),
    onTap: () => Get.toNamed("/home/list"),
  ),
  ListTile(
    title: Text("导航-命名路由 home > list > detail"),
    subtitle: Text('Get.toNamed("/home/list/detail")'),
    onTap: () => Get.toNamed("/home/list/detail"),
  ),
  ListTile(
    title: Text("导航-类对象"),
    subtitle: Text("Get.to(DetailView())"),
    onTap: () => Get.to(DetailView()),
  ),
```

### 导航-清除上一个

- lib/pages/home/index.dart

```dart
  ListTile(
    title: Text("导航-清除上一个"),
    subtitle: Text("Get.off(DetailView())"),
    onTap: () => Get.off(DetailView()),
  ),
```

### 导航-清除所有

- lib/pages/home/index.dart

```dart
  ListTile(
    title: Text("导航-清除所有"),
    subtitle: Text("Get.offAll(DetailView())"),
    onTap: () => Get.offAll(DetailView()),
  ),
```

### 导航-arguments 传值+返回值

- lib/pages/home/index.dart

```dart
  ListTile(
    title: Text("导航-arguments传值+返回值"),
    subtitle: Text(
        'Get.toNamed("/home/list/detail", arguments: {"id": 999})'),
    onTap: () async {
      var result = await Get.toNamed("/home/list/detail",
          arguments: {"id": 999});
      Get.snackbar("返回值", "success -> " + result["success"].toString());
    },
  ),
```

- lib/pages/list_detail/index.dart

```dart
  _buildBackListTileRow(Map? val) {
    return val == null
        ? Container()
        : ListTile(
            title: Text("传值 id = " + val["id"].toString()),
            subtitle: Text('Get.back(result: {"success": true}'),
            onTap: () => Get.back(result: {"success": true}),
          );
  }

  @override
  Widget build(BuildContext context) {
    final details = Get.arguments as Map;
    final parameters = Get.parameters;

    return Scaffold(
      appBar: AppBar(
        title: Text("详情页"),
      ),
      body: ListView(
        children: [
          ListTile(
            title: Text("导航-返回"),
            subtitle: Text('Get.back()'),
            onTap: () => Get.back(),
          ),
          _buildBackListTileRow(details),
          _buildBackListTileRow(parameters),
        ],
      ),
    );
  }
```

### 导航-parameters 传值+返回值

- lib/pages/home/index.dart

```dart
  ListTile(
    title: Text("导航-parameters传值+返回值"),
    subtitle: Text('Get.toNamed("/home/list/detail?id=666")'),
    onTap: () async {
      var result = await Get.toNamed("/home/list/detail?id=666");
      Get.snackbar("返回值", "success -> " + result["success"].toString());
    },
  ),
```

- lib/pages/list_detail/index.dart

```dart
  @override
  Widget build(BuildContext context) {
    final parameters = Get.parameters;
```

### 导航-参数传值+返回值

- lib/common/routes/app_routes.dart

```dart
  static const Detail_ID = '/detail/:id';
```

- lib/common/routes/app_pages.dart

```dart
  ...
  GetPage(
    name: AppRoutes.Detail_ID,
    page: () => DetailView(),
  ),
```

- lib/pages/home/index.dart

```dart
  ListTile(
    title: Text("导航-参数传值+返回值"),
    subtitle: Text('Get.toNamed("/home/list/detail/777")'),
    onTap: () async {
      var result = await Get.toNamed("/home/list/detail/777");
      Get.snackbar("返回值", "success -> " + result["success"].toString());
    },
  ),
```

### 导航-not found

- lib/pages/notfound/index.dart

```dart
class NotfoundView extends StatelessWidget {
  const NotfoundView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("路由没有找到"),
      ),
      body: ListTile(
        title: Text("返回首页"),
        subtitle: Text('Get.offAllNamed(AppRoutes.Home)'),
        onTap: () => Get.offAllNamed(AppRoutes.Home),
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
  static const NotFound = '/notfound';
```

- lib/common/routes/app_pages.dart

```dart
  static final unknownRoute = GetPage(
    name: AppRoutes.NotFound,
    page: () => NotfoundView(),
  );
```

- lib/main.dart

```dart
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      ...
      unknownRoute: AppPages.unknownRoute,
    );
  }
```

### 导航-中间件-认证 Auth

- lib/pages/login/index.dart

```dart
class LoginView extends StatelessWidget {
  const LoginView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("登录"),
      ),
      body: ListTile(
        title: Text("返回首页"),
        subtitle: Text('Get.offAllNamed(AppRoutes.Home)'),
        onTap: () => Get.offAllNamed(AppRoutes.Home),
      ),
    );
  }
}
```

- lib/pages/my/index.dart

```dart
class MyView extends StatelessWidget {
  const MyView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("我的"),
      ),
      body: ListTile(
        title: Text("返回首页"),
        subtitle: Text('Get.offAllNamed(AppRoutes.Home)'),
        onTap: () => Get.offAllNamed(AppRoutes.Home),
      ),
    );
  }
}
```

- lib/common/routes/app_routes.dart

```dart
  static const Login = '/login';
  static const My = '/my';
```

- lib/common/middleware/router_auth.dart

```dart
class RouteAuthMiddleware extends GetMiddleware {
  @override
  int priority = 0;

  RouteAuthMiddleware({required this.priority});

  @override
  RouteSettings? redirect(String route) {
    Future.delayed(Duration(seconds: 1), () => Get.snackbar("提示", "请先登录APP"));
    return RouteSettings(name: AppRoutes.Login);
  }
}
```

- lib/common/routes/app_pages.dart

```dart
    // 白名单
    GetPage(
      name: AppRoutes.Login,
      page: () => LoginView(),
    ),

    GetPage(
      name: AppRoutes.My,
      page: () => MyView(),
      middlewares: [
        RouteAuthMiddleware(priority: 1),
      ],
    ),
```

- lib/pages/home/index.dart

```dart
  ListTile(
    title: Text("导航-中间件-认证Auth"),
    subtitle: Text('Get.toNamed(AppRoutes.My)'),
    onTap: () => Get.toNamed(AppRoutes.My),
  ),
```

### Transition 转场动画

- lib/common/routes/app_pages.dart

```dart
  GetPage(
    name: AppRoutes.Detail_ID,
    page: () => DetailView(),
    transition: Transition.downToUp,
  ),
```

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
