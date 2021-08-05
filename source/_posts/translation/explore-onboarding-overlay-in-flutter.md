---
title: Flutter 新人指导插件 onboarding_overlay
tags: flutter
categories: 译文
date: 2021-08-05 00:00:00
---

![](2021-08-05-09-00-43.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/explore-onboarding-overlay-in-flutter-5ed882e123ac

## 参考

- https://pub.flutter-io.cn/packages/onboarding_overlay

## 正文

![](2021-08-05-08-43-35.png)

Flutter 是一个开源的用户界面软件开发软件开发工具包。Flutter 是一个开源项目，由 Google 负责维护。目前，在 2021 年 3 月。谷歌已经发布了另一个新版本的 Flutter 2。作为一个软件开发工具包应用程序的 Flutter 是很好的，但是当构建一个大的应用程序时，在代码中会有一些问题或者 bug 需要调试。

Flutter 提供了多种调试工具，如时间轴检查器、内存和性能检查器等。这些工具简化了开发者的调试过程，下面列出了调试 Flutter 应用程序的不同工具。

你好朋友，我将谈论我的新博客上探索上板扑覆盖。我们还将实现一个探索 Onboarding 覆盖小部件演示，并使用它们在您的 Flutter 应用程序。那么让我们开始吧。

### Flutter

Flutter 是谷歌的用户界面工具包，它可以帮助你在创纪录的时间内用一个代码库为移动、网络和桌面构建漂亮的本地组合应用程序。这意味着你可以使用一种编程语言和一个代码库来创建两个不同的应用程序(iOS 和 Android)。

### Onboarding Overlay

![](2021-08-05-08-49-40.png)

![](2021-08-05-08-49-53.png)

![](2021-08-05-08-50-04.png)

按照自定义的设计指南，Onboarding Overlay Package 动画包实现了 Onboarding 覆盖，在这里，我们可以使用任何 Onboarding 覆盖中的小部件，我们使用它向用户介绍一个他们不知道的功能。Onboarding overlay 是一个灵活的 Onboarding 小部件，可以通过任意数量的步骤和任意起始点启动和停止。

https://pub.flutter-io.cn/packages/onboarding_overlay

### Implementation

你需要分别在你的代码中实现它:

- 第一步: 添加依赖项。

将依赖项添加到 pubspec.yaml 文件。

```yaml
dependencies:
  onboarding_overlay: ^2.1.0
```

- 步骤 2: 导入包:

```dart
import 'package:onboarding_overlay/onboarding_overlay.dart';
```

- 第三步: 在应用程序的根目录中运行 flutter 软件包。

### Implementation

在 `libfolder` 中创建一个名为 `onboarding_overlay_demo.dart` 的新 dart 文件。

首先，我们必须定义 GlobalKey 和它的内部，它的名字是 onBoardingKey 和 scaffoldKey。

```dart
final GlobalKey<OnboardingState> onboardingKey = GlobalKey<OnboardingState>();
final GlobalKey<ScaffoldState> scaffoldKey = GlobalKey<ScaffoldState>();
```

现在我们将使用 Onboarding Widget，其中我们将使用 Steps 属性，其中我们已经使用了一些不同类型的属性，如标题，titleTextColor，labelBoxPadding，labelBoxDecoration，bodyText 等，我们已经使用了下面的代码，可以在参考的帮助下理解。

```dart
OnboardingStep(
  focusNode: focusNodes[0],
  title: 'Tap anywhere to continue Tap anywhere to continue',
  titleTextColor: Colors.black,
  bodyText: 'Tap anywhere to continue Tap anywhere to continue',
  labelBoxPadding: const EdgeInsets.all(16.0),
  labelBoxDecoration: BoxDecoration(
      shape: BoxShape.rectangle,
      borderRadius: const BorderRadius.all(Radius.circular(8.0)),
      color: const Color(0xFF00E1FF),
      border: Border.all(
        color: const Color(0xFF1E05FB),
        width: 1.0,
        style: BorderStyle.solid,
      )),
  arrowPosition: ArrowPosition.bottomCenter,
  hasArrow: true,
  hasLabelBox: true,
  fullscreen: true,
),
```

### Code File

```dart
import 'package:flutter/material.dart';
import 'package:onboarding_overlay/onboarding_overlay.dart';class OnBoardingOverlayDemo extends StatefulWidget {
  const OnBoardingOverlayDemo({
    Key? key,
    required this.focusNodes,
  }) : super(key: key);

  final List<FocusNode> focusNodes;

  @override
  _OnBoardingOverlayDemoState createState() => _OnBoardingOverlayDemoState();
}

class _OnBoardingOverlayDemoState extends State<OnBoardingOverlayDemo> {
  late int _counter;

  @override
  void initState() {
    super.initState();
    _counter = 0;
  }

  @override
  void dispose() {
    super.dispose();
  }

  void _increment(BuildContext context) {
    setState(() {
      _counter++;
      Onboarding.of(context)!.show();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: IconButton(
          focusNode: widget.focusNodes[4],
          icon: const Icon(Icons.menu),
          onPressed: () {},
        ),
        title: Focus(
          focusNode: widget.focusNodes[3],
          child: const Text('Title'),
        ),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Focus(
              focusNode: widget.focusNodes[0],
              child: const Text('You have pushed the button this many times:'),
            ),
            Focus(
              focusNode: widget.focusNodes[2],
              child: Text(
                '$_counter',
                style: Theme.of(context).textTheme.headline4,
              ),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        focusNode: widget.focusNodes[1],
        onPressed: () {
          _increment(context);
        },
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

### Conclusion

在这篇文章中，我已经解释了在 Flutter 探索在板上覆盖，你可以根据自己的修改和实验，这个小介绍是从我们这边的 Flutter 探索在板上覆盖演示。

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
