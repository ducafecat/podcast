---
title: 企业级Flutter项目-走出第一步
tags: flutter
categories: 译文
date: 2021-05-14 00:00:00
---

![](2021-05-14-11-08-00.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://matteozajac.medium.com/flutter-in-business-first-steps-d8d0648c913b

## 参考

- https://pub.dev/packages/flutter_bloc
- https://pub.dev/packages/chopper
- https://pub.dev/packages/json_serializable
- https://dart.dev/guides/language/effective-dart
- https://plugins.jetbrains.com/plugin/12400-flutter-pub-version-checker

## 正文

大多数时候你必须为你的应用程序的技术债务付款。如果你在 MVP 之后没有良好的体系结构，那么现在是时候停下来，重构一下，让你的未来变得更加容易。事实上，在没有架构的情况下编写较小的应用程序更容易---- 很难不同意这一点---- 但是作为一个成熟的技术专家来考虑。

测试覆盖率，设计模式，代码分析，这些都是我正在考虑的。本文将介绍我们如何在提高代码质量和团队愉悦感的同时，交付出色的应用程序。

### 从架构开始

Provider, BLoC, Redux ーー如果这些词听起来不熟悉，请在继续前进之前对它们有基本的了解。

它们都有优点和缺点，你可以自己选择。

拥有 Flutter 的知识和如何人们已经适应项目结构 BLoC 似乎是最简单的方式开始。

恕我直言，展示和理解 BLoC 如何工作的最好方式是看下面的图表。

![](2021-05-14-10-48-30.png)

- 表示层将事件发送到 BLoC
- 数据层异步执行较长的操作，例如从 API 或数据库获取数据
- 对用户界面产生返回值

就这么简单。

自己实现 BLoC 模式这真的是很好的锻炼，你应该一次性完全理解它背后的流程。如果你已经这样做了

![](2021-05-14-10-50-01.png)

然后使用..。

### BLoC 库

幸运的是，社区没有让人失望。你不必每次都写 BLoC，只需使用这个方便的 library ー FlutterBloc。

我想指出几个关键特征:

- Event — 没有样板的事件-状态通信,
- Dependency 依赖注入通过 BlocProvider,
- BlocBuilder 根据接收的状态构建小部件,
- BlocDelegate 使全局处理错误更加容易,
- BLoC 可以(也应该)进行测试

https://pub.dev/packages/flutter_bloc

### 采用 REST API

![](2021-05-14-10-52-06.png)

如果你创建了一个移动应用程序，你将连接到一个远程数据源。最常用的方法是 REST api 和 JSON。当然，你已经这样做了很多次，所以没有更多的解释。

来自 Android world 的消息表明你已经使用过 Retrofit、 GSON 或莫希 JSON 转换器。这些真的是非常棒的工具。

Flutter 中使用 chopper 库

https://pub.dev/packages/chopper

在这两种情况下，您都需要为您的 API 定义抽象类，并使用 `flutter pub run build_runner build` 生成它。

接下来，没有类似 GSON 的库可以将 JSON 转换为 POJO。您需要编写自己的映射器函数，或者使用 `json_serializable`，它通过注释 Dart 类自动生成转换到 JSON 和从 JSON 转换的代码。这个过程本身非常简单，你肯定会习惯的。

https://pub.dev/packages/json_serializable

### 本地持久化

![](2021-05-14-10-55-34.png)

在大多数情况下，当需要缓存我们的数据时，Sqflite 是我们的首选。它只是一个 SQLite Dart 实现，支持:

- 原始 SQL 查询,
- 插入/查询/更新/删除的方便助手,
- 批次ー避免性能问题。

### 分析代码

在项目中拥有并保持代码样式对于团队来说可能是至关重要的。与体系结构一样，它也是维护项目和团队成员之间的质量、一致性的关键因素。

默认情况下，ide 集成了默认的静态分析，您可以根据需要扩展和调整这些分析。在他们的文档中很好地描述了 Effective 有自己的线头规则ーー Effective Dart。如果您喜欢这种风格(我确实喜欢) ，来自 Google 的开发团队就创建了一个带有这种规则集的包(pedantic | Dart 包)

- Effective Dart
  https://dart.dev/guides/language/effective-dart

- pedantic
  https://pub.dev/packages/pedantic

### 值得一提

手动检查每个包的版本可能有点烦人。对于 Android Studio 用户，你可以查看这个插件 Flutter Pub Version Checker ー For IntelliJ IDEA，Android Studio 为你提供。突出显示带有新版本的软件包非常方便。

https://plugins.jetbrains.com/plugin/12400-flutter-pub-version-checker

### 待续

这是一个关于我们公司内部使用的库和方法的快速总结。如果你正在寻找一些开始点，它也应该有助于你的项目，但作为 Flutter 已经演变，我们有许多可行的解决方案，共同的问题，这只是其中之一。在下一篇文章中，我将展示体系结构图，解释特定的层，并实现一个列表屏幕(从远程、本地持久化获取)。

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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

#### 开源

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
