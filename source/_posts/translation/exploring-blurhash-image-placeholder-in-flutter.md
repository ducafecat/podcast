---
title: flutter 采用 BlurHash 算法实现图像缩略图
tags: flutter
categories: 译文
date: 2021-08-19 00:00:00
---

![](2021-08-19-06-32-44.png)

## 原文

> https://medium.com/flutterdevs/exploring-blurhash-image-placeholder-in-flutter-24dad611c487

## 代码

https://github.com/ducafecat/getx_quick_start

## 参考

- https://pub.dev/packages/flutter_blurhash
- https://blurha.sh/
- https://github.com/woltapp/blurhash

## 正文

![](2021-08-19-06-19-55.png)

根据联想速度的不同，从网络中叠加一张图片可能需要几分钟时间。在获取图片时，完全可以预期它会显示一个占位符。有一些显示占位符的策略。例如，您可以显示一个彩色框。在任何情况下，如果占位符可以像真实的图片一样，那就更令人愉快了。因此，你可以使用 BlurHash。

在这个博客中，我们将探索 Flutter 图片占位符。我们将看到如何实现 blur hash 的演示程序，以及如何使用 BlurHash 作为图像占位符，在您的 flutter 应用程序中使用 `flutter_blurhash` 包。

https://pub.dev/packages/flutter_blurhash

### 简介

BlurHash 是图片占位符的保守描述。它的工作原理是从图片生成一个散列字符串。生成的散列字符串将用于传递占位符。本文介绍了在 Flutter 应用程序中解析要作为图片占位符交付的 BlurHash 字符串的最佳方法。

### 演示

![](demo.gif)

这个演示视频展示了如何在 Flutter 中使用 blurhash。它展示了 blurhash 如何在您的 Flutter 应用程序中使用 `flutter_blurhash` 包工作。它显示了图像占位符的紧凑表示形式。它会显示在你的设备上。

### 构造函数

这里有 BlurHash 的构造函数:

```dart
const BlurHash({
  required this.hash,
  Key? key,
  this.color = Colors.blueGrey,
  this.imageFit = BoxFit.fill,
  this.decodingWidth = _DEFAULT_SIZE,
  this.decodingHeight = _DEFAULT_SIZE,
  this.image,
  this.onDecoded,
  this.onReady,
  this.onStarted,
  this.duration = const Duration(milliseconds: 1000),
  this.curve = Curves.easeOut,
})
```

构造函数期望您传递一个边界散列，它是利用 BlurHash 算法得到的散列字符串。万一你现在没有散列字符串，你可以利用他们的权威站点 blurha。Sh 来创建您需要利用的图片的散列字符串。

### 参数

BlurHash 的一些参数是:

- hash: 需要此参数。要解码的散列
- onDecoded: 此参数用于在对哈希进行解码时回调
- image: 此参数用于远程下载资源
- imageFit: 此参数用于如何适应解码和下载的图像
- color: 此参数用于在解码前显示背景颜色
- duration: 此参数用于动画持续时间。默认值为 const Duration(milliseconds: 1000).
- curve: 此参数用于动画曲线。默认为 Curves.easeOut.
- onStarted: 此参数用于在下载开始时调用的回调
- onReady: 此参数用于在下载映像时回调

### 实施

- 第一步: 添加依赖项

将依赖项添加到 pubspec ー yaml 文件。

```yaml
flutter_blurhash: ^0.6.0
```

- 第二步: 导入

```dart
import 'package:flutter_blurhash/flutter_blurhash.dart';
```

- 第三步: 在应用程序的根目录中运行 flutter 软件包。

```sh
$ flutter packages get
```

### 如何实现 dart 文件中的代码

你需要分别在你的代码中实现它:

> 在 lib 文件夹中创建一个名为 main.dart 的新 dart 文件。

下面是一个只传递散列参数的模型。因为没有传递 Image 参数，它将显示占位符直到时间结束。

```dart
BlurHash(
  hash: 'LHA-Vc_4s9ad4oMwt8t7RhXTNGRj',
),
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-08-19-06-27-18.png)

在主体中，我们将添加 `BlurHash()` 方法。在此方法中，我们将添加 imageFit，这意味着您可以通过将 BoxFit 枚举作为 imageFit 参数传递给构造函数来设置如何将图像适配到可访问空间。如果没有传递参数，值默认为 BoxFit.fill。imageFit 竞争的使用类似于 FittedBox 的合适属性。目前，我们将增加持续时间意味着图片已有效下载后，图片将活跃一个给定的期限之前完全显示。持续时间可以通过传递持续时间尊重作为持续时间争用来设置。如果没有传递参数，默认值是 1 秒

```dart
BlurHash(
  imageFit: BoxFit.fitWidth,
  duration: const Duration(seconds: 4),
  curve: Curves.bounceInOut,
  hash: 'LHA-Vc_4s9ad4oMwt8t7RhXTNGRj',
  image: 'https://images.unsplash.com/photo-1486072889922-9aea1fc0a34d?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8bW91dGFpbnxlbnwwfHwwfHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60',
),
```

现在，我们将设置曲线意味着动画曲线可以通过设置曲线参数。默认值是 Curves.easeOut。最后，我们将添加 image 表示图像参数传递的位置，图像的远程 URL 作为值。当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

现在，我们将设置曲线意味着动画曲线可以通过曲线参数设置。默认值是 Curves.easeOut。最后，我们将添加一个图像，这意味着图像参数是以图片的远端 URL 作为值传递的。当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-08-19-06-29-17.png)

### 完整代码

```dart
import 'package:flutter/material.dart';
import 'package:flutter_blur_hash_demo/splash_screen.dart';
import 'package:flutter_blurhash/flutter_blurhash.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Splash(),
    );
  }
}

class BlurHashDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) =>
         Scaffold(
          appBar: AppBar(
            backgroundColor: Colors.teal,
            automaticallyImplyLeading: false,
              title: Text("Flutter BlurHash Demo")
          ),
          body: Center(
            child: BlurHash(
              imageFit: BoxFit.fitWidth,
              duration: const Duration(seconds: 4),
              curve: Curves.bounceInOut,
              hash: 'LHA-Vc_4s9ad4oMwt8t7RhXTNGRj',
              image: 'https://images.unsplash.com/photo-1486072889922-9aea1fc0a34d?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8bW91dGFpbnxlbnwwfHwwfHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60',
            ),
          ),
      );
}
```

### 结语

在这篇文章中，我简单地解释了 BlurHash 图像占位符的基本结构; 您可以根据自己的选择修改这段代码。这是一个小的介绍 BlurHash 图片占位符用户交互从我这边，它的工作使用 Flutter。

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
