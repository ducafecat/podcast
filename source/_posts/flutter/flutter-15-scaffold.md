---
title: Flutter 零基础入门中文教学 - 15 基础组件 MaterialApp Scaffold
date: 2019-10-9 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- MaterialApp
- Scafford
- Scaffold.of

## MaterialApp

Material 风格的程序的构建，如 Key 导航 路由 首页 样式 多语言 调试

- 构造

```dart
  const MaterialApp({
    Key key,
    // 导航键 , key的作用提高复用性能
    this.navigatorKey,
    // 主页
    this.home,
    // 路由
    this.routes = const <String, WidgetBuilder>{},
    // 初始命名路由
    this.initialRoute,
    // 路由构造
    this.onGenerateRoute,
    // 未知路由
    this.onUnknownRoute,
    // 导航观察器
    this.navigatorObservers = const <NavigatorObserver>[],
    // 建造者
    this.builder,
    // APP 标题
    this.title = '',
    // 生成标题
    this.onGenerateTitle,
    // APP 颜色
    this.color,
    // 样式定义
    this.theme,
    // 主机暗色模式
    this.darkTheme,
    // 样式模式
    this.themeMode = ThemeMode.system,
    // 多语言 本地化
    this.locale,
    // 多语言代理
    this.localizationsDelegates,
    // 多语言回调
    this.localeListResolutionCallback,
    this.localeResolutionCallback,
    // 支持的多国语言
    this.supportedLocales = const <Locale>[Locale('en', 'US')],
    // 调试显示材质网格
    this.debugShowMaterialGrid = false,
    // 显示性能叠加
    this.showPerformanceOverlay = false,
    // 检查缓存图片的情况
    this.checkerboardRasterCacheImages = false,
    // 检查不必要的setlayer
    this.checkerboardOffscreenLayers = false,
    // 显示语义调试器
    this.showSemanticsDebugger = false,
    // 显示debug标记 右上角
    this.debugShowCheckedModeBanner = true,
  })
```

## Scaffold

Scaffold 是一个页面布局脚手架，实现了基本的 Material 布局，继承自 StatefulWidget，是有状态组件。我们知道大部分的应用页面都是含有标题栏，主体内容，底部导航菜单或者侧滑抽屉菜单等等构成，那么每次都重复写这些内容会大大降低开发效率，所以 Flutter 提供了 Material 风格的 Scaffold 页面布局脚手架，可以很快地搭建出这些元素部分

- 构造

```dart
const Scaffold({
    Key key,
    // 菜单栏
    this.appBar,
    // 中间主体内容部分
    this.body,
    // 悬浮按钮
    this.floatingActionButton,
    // 悬浮按钮位置
    this.floatingActionButtonLocation,
    // 悬浮按钮动画
    this.floatingActionButtonAnimator,
    // 固定在下方显示的按钮
    this.persistentFooterButtons,
    // 左侧 侧滑抽屉菜单
    this.drawer,
    // 右侧 侧滑抽屉菜单
    this.endDrawer,
    // 底部菜单
    this.bottomNavigationBar,
    // 底部拉出菜单
    this.bottomSheet,
    // 背景色
    this.backgroundColor,
    // 自动适应底部padding
    this.resizeToAvoidBottomPadding,
    // 重新计算body布局空间大小，避免被遮挡
    this.resizeToAvoidBottomInset,
    // 是否显示到底部，默认为true将显示到顶部状态栏
    this.primary = true,
    this.drawerDragStartBehavior = DragStartBehavior.down,
  })
```

## Scaffold.of

Scaffold.of 函数来获取 ScaffoldState 对象

contenxt 是动态获取的

所以需要用 Builder 套一个构造器

```dart
  static ScaffoldState of(BuildContext context, { bool nullOk = false }) {
    assert(nullOk != null);
    assert(context != null);
    final ScaffoldState result = context.ancestorStateOfType(const TypeMatcher<ScaffoldState>());
    if (nullOk || result != null)
      return result;
    throw FlutterError(
      ...
```

## 示例

```dart
    return MaterialApp(
      // APP 标题
      title: 'Material App',

      // APP 颜色
      color: Colors.yellow,

      // 样式
      theme: ThemeData(primaryColor: Colors.green),

      // 主机暗色模式 android 下无效 ios 可以
      darkTheme: ThemeData(primaryColor: Colors.yellow),

      // 调试显示材质网格
      debugShowMaterialGrid: false,

      // 显示性能叠加
      showPerformanceOverlay: false,

      // 检查缓存图片的情况
      checkerboardRasterCacheImages: false,

      // 检查不必要的setlayer
      checkerboardOffscreenLayers: false,

      // 显示语义调试器
      showSemanticsDebugger: false,

      // 显示debug标记 右上角
      debugShowCheckedModeBanner: false,

      // 主页
      home: Scaffold(
        // 菜单栏
        appBar: AppBar(
          title: Text('Material App Bar'),
        ),

        // 悬浮按钮
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(Icons.add_photo_alternate),
        ),

        // 悬浮按钮位置
        floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,

        // 固定在下方显示的按钮
        persistentFooterButtons: [
          Text('persistentFooterButtons1'),
          Text('persistentFooterButtons2'),
        ],

        // 左侧 侧滑抽屉菜单
        drawer: Drawer(
          child: Text('data'),
        ),

        // 右侧 侧滑抽屉菜单
        endDrawer: Drawer(
          child: Text('data'),
        ),

        // 底部菜单
        bottomNavigationBar: Text('bottomNavigationBar'),

        // 底部拉出菜单
        bottomSheet: Text('bottomSheet'),

        // 背景色
        backgroundColor: Colors.amberAccent,

        // 自动适应底部padding
        resizeToAvoidBottomPadding: true,

        // 压缩顶部菜单空间
        primary: false,

        // drawerDragStartBehavior: DragStartBehavior.start,

        // 正文
        body: Builder(
          builder: (BuildContext context) {
            return Center(
              child: Container(
                child: RaisedButton(
                  onPressed: () {
                    // Scaffold.of(context).openDrawer();
                    Scaffold.of(context).showSnackBar(new SnackBar(
                      content: new Text('Hello!'),
                    ));
                  },
                  child: Text('data'),
                ),
              ),
            );
          },
        ),
      ),

      //
    );

```

## 代码

https://github.com/ducafecat/flutter-learn/blob/master/container_widget

## 参考

- [MaterialApp class](https://api.flutter.dev/flutter/material/MaterialApp-class.html)
- [Scaffold class](https://api.flutter.dev/flutter/material/Scaffold-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
