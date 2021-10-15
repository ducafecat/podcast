---
title: Flutter 导航 Navigator v2 与 GetX
tags: flutter
categories: Flutter见闻
date: 2021-10-14 00:00:00
---

![](2021-10-15-08-42-33.png)

## 前言

因为有不少群友想让我讲下 导航 2.0，那我就来说下吧，这不是个新东西了，2019 年 10 月出的

2.0 的推出并不会影响你 1.0 的使用，他只是加法，这种设计非常好，我的老代码还能跑

- 2.0 的出现，我认为主要是解决 2 个问题：
  - Web 开发 url 参数， http://xxxxxxx/detail/123
  - 对 Page 的堆栈管理（删除某几个，复用某几个，防止无限叠加）

今天我们会先回顾下 1.0 ，然后讲下 2.0 特性，再结合 GetX 来说说一个路由需要解决那些问题，让我们开始吧。

## 本节目标

- 回顾 导航 1.0
  - 匿名路由
  - 命名路由
  - onGenerateRoute 手动解析
- 新导航 2.0 特性
  - Page
  - Route
    - RouteInformationParser
    - RouterDelegate
- GetX 解决了什么问题
  - 路由定义
  - 简化操作
  - 全局操控
  - 路由守卫

## 视频

## 代码

https://github.com/ducafecat/flutter_navigator_v2

## 参考

- https://flutter.cn/community/tutorials/understanding-navigator-v2
- https://docs.google.com/document/d/1Q0jx0l4-xymph9O6zLaOY4d_f7YFpNWX_eGbzYxr9wY/edit#heading=h.l6kdsrb6j9id
- https://www.raywenderlich.com/19457817-flutter-navigator-2-0-and-deep-links#toc-anchor-001
- https://medium.com/flutter/learning-flutters-new-navigation-and-routing-system-7c9068155ade

## 正文

### 1. 回顾 Navigator v1

#### 1.1 匿名路由

![](2021-10-14-15-03-44.png)

主要是通过 `Push()` `Pop()` 来操作路由，简单场景也能满足业务

见代码

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const NavigatorApp());
}

class NavigatorApp extends StatelessWidget {
  const NavigatorApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: ListPage(),
    );
  }
}

class ListPage extends StatelessWidget {
  const ListPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: TextButton(
          child: const Text('Navigator.push -> Details'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) {
                return const DetailPage();
              }),
            );
          },
        ),
      ),
    );
  }
}

class DetailPage extends StatelessWidget {
  const DetailPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: TextButton(
          child: const Text('Navigator.pop -> Pop'),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}

```

#### 1.2 命名路由

这种方式就优雅了很多，事先定义好路由名字，点赞

上代码

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const NavigatorApp());
}

class NavigatorApp extends StatelessWidget {
  const NavigatorApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        '/': (context) => const ListPage(),
        '/details': (context) => const DetailPage(),
      },
    );
  }
}

class ListPage extends StatelessWidget {
  const ListPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: TextButton(
          child: const Text('Navigator.pushNamed -> Details'),
          onPressed: () {
            Navigator.pushNamed(
              context,
              '/details',
            );
          },
        ),
      ),
    );
  }
}

class DetailPage extends StatelessWidget {
  const DetailPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: TextButton(
          child: const Text('Navigator.pop -> Pop'),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}

```

#### 1.3 onGenerateRoute 手动解析

上面的命名路由是好，但是 传参数 不灵活，所有采用 `onGenerateRoute` 来动态处理

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const NavigatorApp());
}

class NavigatorApp extends StatelessWidget {
  const NavigatorApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      onGenerateRoute: (settings) {
        // Handle '/'
        if (settings.name == '/') {
          return MaterialPageRoute(builder: (context) => const ListPage());
        }

        // Handle '/details/:id'
        var uri = Uri.parse(settings.name!);
        if (uri.pathSegments.length == 2 &&
            uri.pathSegments.first == 'details') {
          var id = uri.pathSegments[1];
          return MaterialPageRoute(builder: (context) => DetailPage(id: id));
        }

        return MaterialPageRoute(builder: (context) => const UnknownPage());
      },
    );
  }
}

class ListPage extends StatelessWidget {
  const ListPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: TextButton(
          child: const Text('Navigator.pushNamed -> Details'),
          onPressed: () {
            Navigator.pushNamed(
              context,
              '/details/001',
            );
          },
        ),
      ),
    );
  }
}

class DetailPage extends StatelessWidget {
  final String id;
  const DetailPage({Key? key, required this.id}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: TextButton(
          child: Text('Navigator.pop -> Pop, id = ' + id),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}

class UnknownPage extends StatelessWidget {
  const UnknownPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
      body: Center(
        child: TextButton(
          child: const Text('Navigator.pop -> Unknown'),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}

```

### 2. 新 Navigator 2.0

[Navigator 2.0 参考](https://docs.google.com/document/d/1Q0jx0l4-xymph9O6zLaOY4d_f7YFpNWX_eGbzYxr9wY/edit#)

新加的对象有:

- Page: 一个不可更改的对象，用于设置 Navigator 的历史堆栈。
- Router: 配置要由 Navigator 显示的页面列表。通常此页面列表根据平台或应用的状态变化而变化。
- RouteInformationParser: 它从 RouteInformationProvider 中获取 RouteInformation，并将其解析为用户定义的数据类型。
- RouterDelegate: 定义了 Router 如何学习应用状态变化以及如何响应这些变化的应用特定行为。它的工作是监听 RouteInformationParser 和应用状态，并利用当前的 Pages 列表构建 Navigator。
- BackButtonDispatcher: 向 Router 报告返回按钮按下的情况。

![](2021-10-14-16-08-13.png)

#### 2.1 Page 组件

这是新导航的基础，`Page` 是一个抽象类，需要继承后使用，最终通过重写 `createRoute` 方法生成 Route 路由。

上代码

- `MyPage`

```dart
import 'package:flutter/material.dart';

class MyPage<T> extends Page<T> {
  const MyPage({
    required LocalKey key,
    required String name,
    required this.builder,
  }) : super(key: key, name: name);

  final WidgetBuilder builder;

  @override
  Route<T> createRoute(BuildContext context) {
    return MaterialPageRoute(
      settings: this,
      builder: builder,
    );
  }

  @override
  String toString() => '$name';
}

```

- `MyApp`

```dart
import 'package:flutter/material.dart';
import 'package:flutter_navigator_v2/pages/home.dart';
import 'package:flutter_navigator_v2/pages/list.dart';
import 'package:flutter_navigator_v2/router/page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  final _navigatorKey = GlobalKey<NavigatorState>();
  final pages = [
    MyPage(
      key: const ValueKey('/'),
      name: '/home',
      builder: (context) => const HomePage(),
    ),
    MyPage(
      key: const ValueKey('/list'),
      name: '/list',
      builder: (context) => const ListPage(),
    )
  ];

  bool _onPopPage(Route<dynamic> route, dynamic result) {
    setState(() => pages.remove(route.settings));
    return route.didPop(result);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        body: Navigator(
          key: _navigatorKey,
          onPopPage: _onPopPage,
          pages: List.of(pages),
        ),
      ),
    );
  }
}

```

#### 2.2 Router 组件

Router 是 Navigator 2.0 中新增的另一个非常重要的组件，继承自 StatefulWidget，可以管理自己的状态。

![](2021-10-14-16-12-48.png)

核心对象：

- `MaterialApp.router` 初始化
- `RouterDelegate` 必须项，路由代理
- `RouteInformationParser` 必须项，路由信息解析
- `backButtonDispatcher` 返回事件 回退键
- `TransitionDelegate` 转场动画

![](2021-10-14-16-14-35.png)

上代码

- lib/router/router_names.dart

```dart
import 'package:flutter_navigator_v2/pages/home.dart';
import 'package:flutter_navigator_v2/pages/list.dart';

final routerNames = {
  "/": (context) => const HomePage(),
  "/list": (context) => const ListPage(),
};
```

- lib/router/route_parser.dart

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

class MyRouteParser extends RouteInformationParser<String> {
  // 接受系统传递给我们的路由信息 routeInformation，然后，返回转发给我们之前定义的路由代理 RouterDelegate，
  // 解析后的类型为 RouteInformationParser 的泛型类型，即这里的 String
  @override
  Future<String> parseRouteInformation(RouteInformation routeInformation) {
    var location = routeInformation.location;
    return SynchronousFuture(location!);
  }

  // 返回一个 RouteInformation 对象，表示从传入的 configuration 恢复路由信息。
  @override
  RouteInformation restoreRouteInformation(String configuration) {
    return RouteInformation(location: configuration);
  }
}
```

- lib/router/router_delegate.dart

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:flutter_navigator_v2/router/page.dart';
import 'package:flutter_navigator_v2/router/router_names.dart';

class MyRouteDelegate extends RouterDelegate<String>
    with PopNavigatorRouterDelegateMixin<String>, ChangeNotifier {
  final _stack = <String>[];

  static MyRouteDelegate of(BuildContext context) {
    final delegate = Router.of(context).routerDelegate;
    assert(delegate is MyRouteDelegate, 'Delegate type must match');
    return delegate as MyRouteDelegate;
  }

  MyRouteDelegate({
    required this.onGenerateRoute,
  });

  final RouteFactory onGenerateRoute;

  @override
  GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();

  @override
  String? get currentConfiguration => _stack.isNotEmpty ? _stack.last : null;

  List<String> get stack => List.unmodifiable(_stack);

  void toName(String newRoute) {
    _stack.add(newRoute);
    notifyListeners();
  }

  void push(String newRoute) {
    _stack.add(newRoute);
    notifyListeners();
  }

  void remove(String routeName) {
    _stack.remove(routeName);
    notifyListeners();
  }

  void pop() {
    _stack.remove(_stack.last);
    notifyListeners();
  }

  bool _onPopPage(Route<dynamic> route, dynamic result) {
    if (_stack.isNotEmpty) {
      if (_stack.last == route.settings.name) {
        _stack.remove(route.settings.name);
        notifyListeners();
      }
    }
    return route.didPop(result);
  }

  @override
  Future<void> setInitialRoutePath(String configuration) {
    return setNewRoutePath(configuration);
  }

  @override
  Future<void> setNewRoutePath(String configuration) {
    _stack
      ..clear()
      ..add(configuration);
    return SynchronousFuture<void>(null);
  }

  @override
  Widget build(BuildContext context) {
    return Navigator(
      key: navigatorKey,
      onPopPage: _onPopPage,
      pages: [
        for (final name in _stack)
          MyPage(
            key: ValueKey(name),
            name: name,
            builder: routerNames[name] as Widget Function(BuildContext),
          ),
      ],
    );
  }
}

```

- lib/example_05_router_delegate.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_navigator_v2/pages/home.dart';
import 'package:flutter_navigator_v2/router/route_parser.dart';
import 'package:flutter_navigator_v2/router/router_delegate.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  final delegate = MyRouteDelegate(
    onGenerateRoute: (RouteSettings settings) {
      return MaterialPageRoute(
        settings: settings,
        builder: (BuildContext context) {
          return const HomePage();
        },
      );
    },
  );

  @override
  Widget build(BuildContext context) {
    // Navigator 2.0 之后，Flutter 也提供了 MaterialApp 的新构造函数 router 来帮助我们直接在应用顶层构造出全局的 Router 组件
    return MaterialApp.router(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      routeInformationParser: MyRouteParser(), // 路由信息解析
      routerDelegate: delegate, // 路由代理
    );
  }
}

```

### 3. GetX

作为一个路由组件需要解决的问题:

- 路由定义
- 简化操作
- 全局操控
- 路由守卫
- 转场动画
- 适配 Web SEO 场景
- 堆栈管理（欠缺）

#### `GetPage` 对象

继承了 `Page`, 在定义路由阶段就声明了功能：命名路由、转场、嵌套、中间件...

```dart
class GetPage<T> extends Page<T> {
  @override
  final String name;
  final GetPageBuilder page;
  final bool? popGesture;
  final Map<String, String>? parameter;
  final String? title;
  final Transition? transition;
  final Curve curve;
  final Alignment? alignment;
  final bool maintainState;
  final bool opaque;
  final Bindings? binding;
  final List<Bindings> bindings;
  final CustomTransition? customTransition;
  final Duration? transitionDuration;
  final bool fullscreenDialog;
  final RouteSettings? settings;
  final List<GetPage>? children;
  final List<GetMiddleware>? middlewares;
  final PathDecoded path;
  final GetPage? unknownRoute;
```

#### `GetNavigation` 导航对象及方法

丰富的方法

![](2021-10-14-16-26-52.png)

#### `GetMiddleware` 路由中间件

在 Page 的生命周期里处理

![](2021-10-14-16-29-22.png)

## 结束语

关于 GetX 的使用，不在这里重复，这里只是想阐述一个路由组件生产环境下要面对的问题。

[GetX 快速上手 -> 点这里](https://ducafecat.tech/categories/Flutter-Getx/)

---

© 猫哥

- https://ducafecat.tech/

- https://github.com/ducafecat

- 微信群 ducafecat

- b 站 https://space.bilibili.com/404904528

![](https://ducafecat.tech/img/public-qrcode.png)

## 往期

### 开源

#### GetX Quick Start

https://github.com/ducafecat/getx_quick_start

#### 新闻客户端

https://github.com/ducafecat/flutter_learn_news

### strapi 手册译文

https://getstrapi.cn

### 微信讨论群 ducafecat

### 系列集合

#### 译文

https://ducafecat.tech/categories/%E8%AF%91%E6%96%87/

#### 开源项目

https://ducafecat.tech/categories/%E5%BC%80%E6%BA%90/

#### Dart 编程语言基础

https://space.bilibili.com/404904528/channel/detail?cid=111585

#### Flutter 零基础入门

https://space.bilibili.com/404904528/channel/detail?cid=123470

#### Flutter 实战从零开始 新闻客户端

https://space.bilibili.com/404904528/channel/detail?cid=106755

#### Flutter 组件开发

https://space.bilibili.com/404904528/channel/detail?cid=144262

#### Flutter Bloc

https://space.bilibili.com/404904528/channel/detail?cid=177519

#### Flutter Getx4

https://space.bilibili.com/404904528/channel/detail?cid=177514

#### Docker Yapi

https://space.bilibili.com/404904528/channel/detail?cid=130578
