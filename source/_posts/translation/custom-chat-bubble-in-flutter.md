---
title: Flutter 自定义聊天气泡
tags: flutter
categories: 译文
date: 2021-07-13 00:00:00
---

![](2021-07-13-07-26-06.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/custom-chat-bubble-in-flutter-6aa7d24fc683

## 代码

https://github.com/flutter-devs/flutter_custom_chat_bubble

## 参考

- https://pub.flutter-io.cn/packages/get#reactive-state-manager
- https://dart.dev/guides/language/extension-methods

## 正文

![](2021-07-13-07-13-02.png)

对话聊天应用程序显示聊天中的消息会在强烈的阴影背景下上升。现代聊天应用程序显示的聊天气泡的斜率取决于气泡在屏幕上的位置。在 flutter 应用中，有时需要使用聊天气泡。然而，将一个库用于一个特别无关紧要的任务并不好。

在这个博客，我们将探索自定义聊天气泡 flutter。我们将看到如何实现一个自定义聊天泡泡的演示程序，以及如何使一个自定义聊天泡泡最简单的不使用任何第三方库在您的 flutter 应用程序。

### 配置 assets

- 第一步: 添加 assets

将 assets 添加到 `pubspec.yaml` 文件。

```yaml
assets:
  - assets/images/
```

- 第二步: 在应用程序的根目录中运行 `flutter packages get` 。

### 如何实现 dart 文件中的代码:

你需要分别在你的代码中实现它:

在 lib 文件夹中创建一个名为 `custom_shape.dart` 的新 dart 文件。

首先，创建自定义形状自定义 `CustomPainter` 类。这将用于在聊天气泡结束时绘制自定义形状。用户可以在自定义形状中添加任何颜色。

```dart
import 'package:flutter/material.dart';

class CustomShape extends CustomPainter {
  final Color bgColor;

  CustomShape(this.bgColor);

  @override
  void paint(Canvas canvas, Size size) {
    var paint = Paint()..color = bgColor;

    var path = Path();
    path.lineTo(-5, 0);
    path.lineTo(0, 10);
    path.lineTo(5, 0);
    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) {
    return false;
  }
}
```

在 lib 文件夹中创建一个名为 `send_message_screen.dart` 的新 dart 文件。

首先，我们将创建一个构造器的最终字符串消息。

```dart
final String message;
const SentMessageScreen({
  Key key,
  @required this.message,
}) : super(key: key);
```

在构建方法中，我们将返回 Padding()。在内部，我们将添加 Row() 小部件。在这个小部件中，我们将添加 mainAxisAlignment 并添加 messageTextGroup。我们将定义下面的代码。

```dart
return Padding(
  padding: EdgeInsets.only(right: 18.0, left: 50, top: 15, bottom: 5),
  child: Row(
    mainAxisAlignment: MainAxisAlignment.end,
    children: <Widget>[
      SizedBox(height: 30),
      messageTextGroup,
    ],
  ),
);
```

我们将深入定义 messageTextGroup:

我们将创建一个最终的 messageTextGroup 等于 Flexible ()小部件。在这个小部件中，我们将添加 Row ()小部件。在内部，添加主轴对齐是结束，并启动了跨轴对齐。在儿童内部，我们将添加装饰框的 Conatiner 和添加颜色，边框半径。它的子属性，我们将添加一个可变消息文本。我们将添加 CustomPaint () ，我们将使用上面的画家类是带颜色的 CustomShape。

```dart
final messageTextGroup = Flexible(
    child: Row(
      mainAxisAlignment: MainAxisAlignment.end,
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Flexible(
          child: Container(
            padding: EdgeInsets.all(14),
            decoration: BoxDecoration(
              color: Colors.cyan[900],
              borderRadius: BorderRadius.only(
                topLeft: Radius.circular(18),
                bottomLeft: Radius.circular(18),
                bottomRight: Radius.circular(18),
              ),
            ),
            child: Text(
              message,
              style: TextStyle(color: Colors.white, fontSize: 14),
            ),
          ),
        ),
        CustomPaint(painter: CustomShape(Colors.cyan[900])),
      ],
    ));
```

在 lib 文件夹中创建一个名为 `received_message_screen.dart` 的新 dart 文件。

类似地，我们现在可以创建一个接收到的消息屏幕。我们只需要翻转定制的形状，并把它放在开始，而不是结束。我们将使用转换小部件翻转自定义形状小部件。在转换小部件中，我们将添加对齐为中心，转换为 `Matrix4.rotationY(math. pi)`。

```dart
final messageTextGroup = Flexible(
    child: Row(
      mainAxisAlignment: MainAxisAlignment.start,
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Transform(
          alignment: Alignment.center,
          transform: Matrix4.rotationY(math.pi),
          child: CustomPaint(
            painter: CustomShape(Colors.grey[300]),
          ),
        ),
        Flexible(
          child: Container(
            padding: EdgeInsets.all(14),
            decoration: BoxDecoration(
              color: Colors.grey[300],
              borderRadius: BorderRadius.only(
                topRight: Radius.circular(18),
                bottomLeft: Radius.circular(18),
                bottomRight: Radius.circular(18),
              ),
            ),
            child: Text(
              message,
              style: TextStyle(color: Colors.black, fontSize: 14),
            ),
          ),
        ),
      ],
    ));
```

在 lib 文件夹中创建一个名为 `home_page.dart` 的新 dart 文件。

在正文中，我们将添加一个 Container ()小部件。在里面，添加装饰框和添加图像。它是子属性，我们可以在 ListView ()中同时添加发送和接收消息屏幕。

```dart
Container(
  decoration: BoxDecoration(
      image: DecorationImage(
          image: AssetImage("assets/bg_chat.jpg"),
          fit: BoxFit.cover)),
  child: ListView(
    children: [
      SentMessageScreen(message: "Hello"),
      ReceivedMessageScreen(message: "Hi, how are you"),
      SentMessageScreen(message: "I am great how are you doing"),
      ReceivedMessageScreen(message: "I am also fine"),
      SentMessageScreen(message: "Can we meet tomorrow?"),
      ReceivedMessageScreen(message: "Yes, of course we will meet tomorrow"),
    ],
  ),
),
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-07-13-07-19-48.png)

### 完整代码

```dart
import 'package:flutter/material.dart';
import 'package:flutter_custom_chat_bubble/received_message_screen.dart';
import 'package:flutter_custom_chat_bubble/send_messsage_screen.dart';

class HomePage extends StatefulWidget {
  HomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.cyan[900],
        automaticallyImplyLeading: false,
        title: Text(widget.title),
      ),
      body: Container(
        decoration: BoxDecoration(
            image: DecorationImage(
                image: AssetImage("assets/bg_chat.jpg"),
                fit: BoxFit.cover)),
        child: ListView(
          children: [
            SentMessageScreen(message: "Hello"),
            ReceivedMessageScreen(message: "Hi, how are you"),
            SentMessageScreen(message: "I am great how are you doing"),
            ReceivedMessageScreen(message: "I am also fine"),
            SentMessageScreen(message: "Can we meet tomorrow?"),
            ReceivedMessageScreen(message: "Yes, of course we will meet tomorrow"),
          ],
        ),
      ),
    );
  }
}
```

### Conclusion:

在文章中，我已经解释了自定义聊天气泡的基本结构，您可以根据自己的选择修改这个代码。这是一个小的介绍自定义聊天泡泡用户交互从我这边，它的工作使用 flutter。

我希望这个博客将提供您尝试在您的 flutter 项目自定义聊天气泡充分的信息。我们将为工作的演示程序自定义聊天气泡使用任何第三方库在您的 flutter 应用程序。所以请尝试一下。

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
