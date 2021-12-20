---
title: Flutter 2.8 发布 新特性速览
tags: flutter
categories: 译文
date: 2021-12-14 00:00:00
---

## 视频

https://www.bilibili.com/video/bv1sR4y1x758

## 参考

- https://medium.com/flutter/whats-new-in-flutter-2-8-d085b763d181
- https://medium.com/flutter/announcing-general-availability-for-the-google-mobile-ads-sdk-for-flutter-574e51ea6783
- https://flutter.dev/monetization
- https://api.flutter-io.cn/flutter/widgets/HtmlElementView-class.html
- https://pub.flutter-io.cn/packages/webview_flutter

## 正文

### 性能提升

- 程序启动
  - 在低端 Android 设备上启动的时间减少了约 50%，高端设备上减少了约 10%
  - 低端 Android 设备的初始帧出现间隔时间最多减少了约 300ms
  - 在低端设备上的启动时间至少减少了 100ms
- 应用内存
  - 减少了加载前约 40MB 的内存使用
  - 后续的内存占用量进一步减少了约 10%
- 性能分析
  - 性能跟踪中的新的 流事件让开发人员可以跟踪光栅缓存图片的生命周期

### Flutter DevTools

新版的 DevTools 添加了一个新的「增强跟踪」功能， 用来帮助开发者诊断消耗较大的构建、布局和绘制操作引起的 UI 卡顿。

![](2021-12-14-07-12-52.png)

启用任何一个追踪功能后，时间轴中将视情况展示 Widget 的构建、RenderObject 布局和 RenderObject 绘制的事件。

![](2021-12-14-07-13-07.png)

此外，新版的 DevTools 增加了应用程序启动性能的分析支持。 该配置文件包含从 Dart VM 初始化到第一个 Flutter 帧渲染的 CPU 样本。 在你按下「Profile app start up」按钮并加载应用程序启动配置文件后， 你将看到为配置文件选择了「AppStartUp」用户标签。 你还可以通过在可用用户标签列表中选择此用户标签过滤器（如果存在）来加载应用程序启动配置文件。 选择此标签会显示你的应用程序启动的个人资料数据。

![](2021-12-14-07-13-30.png)

### Web 平台的平台视图 (PlatformView)

支持嵌入 `HtmlElementView` 对象，这真是太好了，我可以嵌入地图、iframe 有待确认。

https://api.flutter-io.cn/flutter/widgets/HtmlElementView-class.html

### 适用于 Flutter 广告的 Google 广告

Google Mobile SDK for Flutter 已于 11 月正式发布

https://medium.com/flutter/announcing-general-availability-for-the-google-mobile-ads-sdk-for-flutter-574e51ea6783

https://flutter.dev/monetization

这很重要哦，你可以收钱了。

### WebView 3.0

https://pub.flutter-io.cn/packages/webview_flutter

- 支持使用 POST 和 GET 来加载内容
- 加载文件或字符串内容为 HTML
- 支持透明背景
- 在加载内容前设置 Cookies

### webview_flutter_web 插件支持 iframe 插入

```yaml
dependencies:
  webview_flutter: ^3.0.0
  webview_flutter_web: ^0.1.0 # 显式依赖未经认可的插件
```

![](2021-12-14-07-22-19.png)

### 官方推荐的插件

https://pub.flutter-io.cn/flutter/favorites

- 路由 新导航 v2
  - https://pub.flutter-io.cn/packages/beamer
  - https://pub.flutter-io.cn/packages/routemaster
  - https://pub.flutter-io.cn/packages/go_router
- sqlite 库 drift
  - https://pub.flutter-io.cn/packages/drift
- freezed model 代码生成器，支持注解
  - https://pub.flutter-io.cn/packages/freezed
- dart_code_metrics 代码静态质量检查
  - https://pub.flutter-io.cn/packages/dart_code_metrics
- ui 相关库
  - https://pub.flutter-io.cn/packages/flex_color_scheme
  - https://pub.flutter-io.cn/packages/flutter_svg
  - https://pub.flutter-io.cn/packages/feedback
  - https://pub.flutter-io.cn/packages/toggle_switch
  - https://pub.flutter-io.cn/packages/auto_size_text

### 特定平台插件开发

- 通过 pubspec.yaml 配置
  - https://docs.flutter.dev/development/packages-and-plugins/developing-packages#plugin-platforms
- 用 dart 写插件
  - https://docs.flutter.dev/development/packages-and-plugins/developing-packages#dart-only-platform-implementations
- Dart FFI 现在很成熟了
  - https://dart.dev/guides/libraries/c-interop

### Firebase

http://firebase.flutter.dev/

- 所有 FlutterFire 插件都从 beta 阶段升级到稳定阶段
- 在 DartPad 中为几个 Firebase 服务提供新支持
- 新的库可以更容易地为身份验证和实时 Firestore 查询构建 UI
- 新的 Firestore 对象/文档映射用于 Flutter，可在 Alpha 中使用

![](2021-12-14-08-51-20.png)

![](2021-12-14-08-52-55.png)

![](2021-12-14-08-53-07.png)

UI 也升级了

![](2021-12-14-08-53-23.png)

![](2021-12-14-08-53-35.png)

![](2021-12-14-08-53-46.png)

### 桌面开发

- 中文、韩文输入支持

- Announcing FlutterFire for Desktop
  https://invertase.io/blog/announcing-flutterfire-desktop

### DartPad

- 切换 flutter 版本
- 预载入很多包
- 方便的嵌入你的 gist 代码

![](2021-12-14-08-56-24.png)

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
