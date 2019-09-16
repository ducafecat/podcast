---
title: Flutter 见闻 - 01 Flutter 1.9 正式发布 实现三端编译发布 android ios web
date: 2019-09-16 00:00:00
tags: flutter
categories: Flutter见闻
---

## 本节目标

- 在 web 平台运行 Flutter
- macOS Catalina 和 iOS 13 支持
- 全新的 Material widget
- 全球语言支持
- Dart 2.5 发布
- 工具链优化

## 在 web 平台运行 Flutter

- 更新 SDK

```sh
> flutter channel master
> flutter upgrade
```

- 启用 web 支持

```sh
> flutter config --enable-web

> flutter devices
```

- 更新现有项目

```sh
> flutter create .
```

- 创建新项目

```sh
> flutter create myapp
```

- 运行 web

```sh
> flutter run -d chrome
```

- 编译 web

```sh
> flutter build web
```

## Flutter Widget Livebook

一个在网页上展示 widget 运行效果的网站，它使用 Flutter 开发，并直接运行在网页上。

https://flutter-widget-livebook.blankapp.org/basics/introduction/

![](2019-09-16-15-06-02.png)

## Panache

则是一款为 Flutter 创建主题的工具，您可以下载创建好的主题，然后将其直接添加到代码中。

https://rxlabz.github.io/panache_web/#/

![](2019-09-16-15-07-30.png)

![](2019-09-16-15-08-15.png)

![](2019-09-16-15-09-15.png)

## macOS Catalina 和 iOS 13 支持

- iOS 13 的拖拽式工具栏

https://github.com/flutter/flutter/pull/35829

- 触感反馈

https://github.com/flutter/flutter/pull/37724

- 开发者已经提交了 pull request

https://github.com/flutter/flutter/issues/35541

- 启用 Bitcode 实验性支持

https://github.com/flutter/flutter/wiki/Creating-an-iOS-Bitcode-enabled-app-(experimental)

## 全新的 Material widget

- ToggleButtons 示例

它能为您的应用按钮实现更加多元化的设计——不论是单选还是多选，选择至少一个或是零个，尖角还是圆角、粗边或细边，图标或文本——ToggleButtons widget 全都可以满足。

https://github.com/csells/flutter_toggle_buttons

- ColorFiltered 示例

允许您更改子 widget 树的颜色，用来灵活的调整配色服务。

https://github.com/csells/flutter_color_filter

## 全球语言支持

还新增了南非语 (Afrikaans)、祖鲁语 (Zulu) 等 24 种语言的支持。

![](2019-09-16-14-48-40.png)

## Dart 2.5 发布

- ML 代码补全

https://github.com/dart-lang/sdk/wiki/Previewing-Dart-code-completions-powered-by-machine-learning

- 用于 Dart-C 互操作的 ffi 外部函数接口

- 改进常量表达式

```dart
const Object i = 3;
const list = [i as int];
const set = {if (list is List<int>) ...list};
const map = {if (i is int) i: "int"};
```

## 工具链优化

- 从 Flutter 1.9 开始，iOS 新项目默认使用 Swift 语言，而非 Objective-C；Android 新项目则默认使用 Kotlin，而非 Java。

- Swift 编译瘦身

- 改善错误信息提示

![](2019-09-16-15-04-00.png)

https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652050546&idx=1&sn=2c81e067ac34da40f89558f426e97af6&chksm=808cb437b7fb3d2127431c7858beb3a03a29186c1c6a78aa0a2e97cd895e756b7ee3367ec935&scene=21#wechat_redirect

## 参考

- [Building a web application with Flutter](https://flutter.dev/docs/get-started/web)
- [flutter_web](https://github.com/flutter/flutter_web)
- [Flutter 1.9 正式发布！| 全平台创新开发体验](https://mp.weixin.qq.com/s/uajbjbVYmcBHtq3Jv8tgzg)
- [腾讯视频链接](https://v.qq.com/x/page/t09240vp8cy.html)
- [Flutter Widget Livebook](https://flutter-widget-livebook.blankapp.org/basics/introduction/)
- [更精准更简洁: Flutter 改进错误信息提示](https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652050546&idx=1&sn=2c81e067ac34da40f89558f426e97af6&chksm=808cb437b7fb3d2127431c7858beb3a03a29186c1c6a78aa0a2e97cd895e756b7ee3367ec935&scene=21#wechat_redirect)
- [Panache](https://rxlabz.github.io/panache_web/#/)
- [iOS 13 的拖拽式工具栏](https://github.com/flutter/flutter/pull/35829)
- [触感反馈](https://github.com/flutter/flutter/pull/37724)
- [开发者已经提交了 pull request](https://github.com/flutter/flutter/issues/35541)
- [启用 Bitcode 实验性支持](<https://github.com/flutter/flutter/wiki/Creating-an-iOS-Bitcode-enabled-app-(experimental)>)
- [ToggleButtons](https://github.com/csells/flutter_toggle_buttons)
- [ColorFiltered](https://github.com/csells/flutter_color_filter)
- [ML 代码补全](https://github.com/dart-lang/sdk/wiki/Previewing-Dart-code-completions-powered-by-machine-learning)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
