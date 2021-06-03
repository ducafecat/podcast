---
title: Flutter 5个必备的桌面插件包将包含在你的下一个版本中
tags: flutter
categories: 译文
date: 2021-06-04 00:00:00
---

![](2021-06-04-07-47-05.png)

## 猫哥说

看到这张图，也许你和我一样向往着宁静的生活。

今天推荐文章中，感觉 字体、动画、下拉 插件还是很有用的，估计你都用上了。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

![](2021-06-04-07-25-03.png)

## 原文

> https://medium.com/fddevops/5-must-have-flutter-desktop-packages-to-include-in-your-next-build-e45d6dfd3995

## 参考

- https://pub.dev/packages/provider
- https://pub.dev/packages/google_fonts
- https://pub.dev/packages/photo_view
- https://pub.dev/packages/animations
- https://pub.dev/packages/flutter_pull_to_refresh

## 正文

是 Google 在 2018 年开发的一个软件开发工具包。自成立以来，它获得了业界的广泛赞誉。使它脱颖而出的是其简单易学的编码语言省道，简单醒目的小部件设计，以及跨平台的开发能力。

Flutter 继续作出巨大的改进，现在是一个稳定的产品都 Flutter 网络和移动。虽然 Flutter Desktop Desktop 仍处于 alpha 阶段，但随着开发人员继续将其用于桌面应用程序开发，您可以期待在未来几个月内得到大量增强。在本文中，我们将向您介绍在下一个版本中必须包含的 5 个桌面软件包。

### 为什么桌面仍然有意义？

如果你相信桌面应用程序的时代已经结束，那么你将是一个很好的公司。毕竟，移动应用程序的开发和使用仍在继续飞速增长，人们的注意力主要集中在移动应用的未来。

尽管如此，许多用户还是喜欢在更大的屏幕上查看应用程序，即使它不是桌面应用程序。桌面用户可以查看更多的信息，方便地导航，并且可以花更多的时间在应用程序上。

### 跨平台开发的兴起

在过去的几年里，对本地开发人员的需求已经有了显著的下降。DRY (不要重复自己)长期以来一直是开发人员的圣杯。JsNode 有“承诺”(没有双关语的意思) ，然后 Xamarin 作为一个跨开发工具可以在多种平台上使用。本地开发中缺少这个特性。

Flego 是第一个跨平台开发工具，现在称为 React Native。Flutter 是一个跨平台的开发工具，它配备了 UI 呈现组件、导航、测试和大量的库。Flutter 引擎包含了开发人员构建和部署他们的应用程序所需的所有特性。

由于这些新的发展，许多人都认为 Flutter 有可能为桌面开发取代 electron。

### Flutter 引擎

Flutter 团队的目标是构建一个跨平台的 UI 工具包，以实现代码的可重用性。这就导致了 Flutter 发动机的发展。从技术的角度来看，Flutter 引擎把像素的屏幕上，当他们是必要的。Flutter 发动机是 Flutter 快速、高质量输出的基石。

Flutter 新的面向桌面的 alpha 版本允许更多的键盘输入、鼠标控制和大屏幕显示。

### 用于 Flutter 的桌面插件

在 Windows、 Mac 和 Linux 操作系统上，有大量的桌面软件包可以使用。下面是这些软件包的一个快速概述。

#### Provider 5.0.0 (Null Safety)

https://pub.dev/packages/provider

它是一个包装器，围绕着一个可继承的 widget，使它可重用且易于使用。你可以在代码中使用 Provider 而不是手动编写 Inheritedwidget，你会得到以下好处:

- 简化资源分配
- 延迟加载
- 一个显着减少样板和使一个新的类每次
- 用户友好的开发工具
- 在代码中使用 IngeritedWidget 的最可靠的方法
- 为类提供更多的可伸缩性

#### Google_fonts

https://pub.dev/packages/google_fonts

这并不奇怪。这个 Flutter 软件包可以让你在 Flutter 应用程序中使用 977 字体中的任何一种以及它们的变体，这些字体都来自 fonts.google.com。

开始使用 google 字体

使用 google 字体包,。或者。Otf 文件不需要存储在 assets 文件夹中，可以在 pubspec 中映射。它们可以在运行时通过 HTTP 命令检索一次，并且可以缓存在应用程序的系统中。这个包是专门为减少应用程序包的大小而设计的。使用 google_fonts 包，开发人员可以选择预绑定字体，然后使用相同的 API 在 HTTP 上选择字体。

#### Flutter Photo View

https://pub.dev/packages/photo_view

一个简单的可缩放的用于 flutter 的图像/内容小部件。PhotoView 允许用户缩放图片，迎合用户的捏、旋转和拖动手势。

它还可以用于显示图像中的任何小部件，如 Container、 Text 或 SVG。虽然 PhotoView Flutter 软件包很容易使用，但是通过它的选项和控制器它是非常可定制的。

- 如何安装？

在 pubspec.yaml 文件中添加 photo_view 作为依赖项

```yaml
dependencies:
  photo_view: ^0.11.1
```

- 导入照片查看:

```dart
import 'package:photo_view/photo_view.dart';
```

- 非常基本的用法

```dart
@override
Widget build(BuildContext context) {
  return Container(
    child: PhotoView(
      imageProvider: AssetImage("assets/large-image.jpg"),
    )
  );
}
```

#### animations

高质量的 Flutter 动画预制。该软件包配备了预先录制的动画，以达到预期的效果。动画可以根据你的内容进行定制，也可以集成到应用程序中以取悦用户:

##### Material Motion for Flutter

Material Motion 是一组过渡模式，帮助用户理解和导航应用程序。目前，这个库提供了以下转换模式:

- Container transform

Container transform 模式旨在促进包含容器的 UI 元素之间的转换。下面显示的图片告诉我们，这个包在两个 UI 元素之间创建了一个可见的连接。

- Shared axis

共享轴模式有助于在具有空间或导航关系的 UI 元素之间进行转换。该模式在 x、 y 和 z 轴上使用共享转换来加强元素之间的关系。

- Fade through

淡入模式用于在互不紧密相关的 UI 元素之间进行过渡。

- Fade

淡入模式用于那些存在于屏幕边界内的 UI 元素，例如在屏幕中心淡出的对话框。

#### Flutter pulltorefresh

该 Flutter 软件包集成了 Flutter 滚动部件和下拉刷新功能。

功能:

- 当你在窗口中向上滚动时，它会加载，当你向下滚动时，它会刷新
- 它最适合所有的滚动小部件，如 GridView 和 ListView
- 配备了一些常见的指示器
- 附带默认指示符和属性的全局设置
- 除了水平和垂直刷新，它还支持反向 ScrollView
- 包含更多的更新风格，比如 Behind，Follow，Unfollow 和 Front
- 支持两级刷新

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
