---
title: Flutter SnackBar 组件插件
tags: flutter
categories: 译文
date: 2021-09-14 00:00:00
---

![](2021-09-14-09-15-10.png)

## 原文

> https://medium.com/flutterdevs/snackbar-widget-in-flutter-476bb2431538

## 代码

https://github.com/flutter-devs/flutter_snackbar_demo

## 参考

- https://pub.flutter-io.cn/packages/another_flushbar

## 正文

### 了解如何创建静态和自定义 SnackBar 小部件在您的 Flutter 应用程序

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/f730b3b2021dd58f1e32d8d62e00e6e780e24f69354e5caca791be2b06752368.png)

无论何时你要编写在 Flutter 构建任何东西的代码，它都会在一个小部件中。Flutter 应用程序屏幕上的每个元素都是一个小部件。屏幕的透视图完全依赖于用于构建应用程序的小部件的选择和分组。此外，应用程序代码的结构是一个小部件树。

在本博客中，我们将了解静态和自定义 SnackBar 小部件及其在 flutter 中的功能。我们将在这个 **SnackBar Widget** 小部件上看到一个简单演示程序的实现。

https://pub.flutter-io.cn/packages/another_flushbar

在 Flutter 中，SnackBar 是一个小工具，它可以轻量级地在你的应用程序中弹出一条快速消息，在发生事情时短暂地标示出用户。使用 SnackBar，你可以在你的应用程序底部弹出一条消息几秒钟。

默认情况下，SnackBar 显示在屏幕的底部，当指定的时间完成，它将从屏幕上消失，我们可以使一个自定义的 SnackBar 也和它包含一些行动，允许用户添加或删除任何行动和图像。SnackBar 需要一个 Scaffold，带有一个 Scaffold 实例，你的 SnackBar 会立即弹出。通过使用 scaffold，可以很容易地在小部件树中的任何位置获得 scaffold 的引用。功能。

> 演示模块:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bcd6543078ae65a9e26b0c3ae36a26051bf808c3262a0f7de4fa75d98d47f6c8.gif)

### 如何实现 dart 文件中的代码:

> 你需要分别在你的代码中实现它:

首先，在这个 Dart 文件中，我创建了两个按钮，第一个按钮用于显示默认的 SnackBar，第二个按钮用于自定义 SnackBar。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/8bdd3d30e80748e96cb2df7b0c947e00e6760f23f7d0c40674c1d930b4d2bebc.png)

Default SnackBar 默认 SnackBar

显示 SnackBar 有两个步骤。首先，您必须创建一个 SnackBar，这可以通过调用以下构造函数来完成。非常简单易用。这是密码。

```dart
final snackBar = SnackBar(content: Text('Yay! A DefaultSnackBar!'));
ScaffoldMessenger._of_(context).showSnackBar(snackBar);
```

但默认情况下，我们的一些要求没有得到满足。所以 May 自定义 SnackBar 正在做这件事。对于自定义 SnackBar，我必须使用 Flushbar 依赖项。这是一个非常和谐的依赖性设计您的自定义 SnackBar 根据您的选择。

> 首先，在 pubspec.yaml 中添加 SnackBar 的依赖项

```dart
another_flushbar: ^1.10.24
```

然后必须创建一个方法来显示自定义 SnackBar。

```dart
void showFloatingFlushbar( {@required BuildContext? context,
  @required String? message,
  @required bool? isError}){
  Flushbar? flush;
  bool? _wasButtonClicked;
  flush = Flushbar<bool>(
    title: "Hey User",
    message: message,
    backgroundColor: isError! ? Colors._red_ : Colors._blueAccent_,
    duration: Duration(seconds: 3),
    margin: EdgeInsets.all(20),


    icon: Icon(
      Icons._info_outline_,
      color: Colors._white_,),
    mainButton: FlatButton(
      onPressed: () {
        flush!.dismiss(true); _// result = true_ },
      child: Text(
        "ADD",
        style: TextStyle(color: Colors._amber_),
      ),
    ),) _// <bool> is the type of the result passed to dismiss() and collected by show().then((result){})_ ..show(context!).then((result) {

    });
}
```

当我们运行应用程序时，我们应该获得屏幕输出，就像下面的屏幕截图一样。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/202f9454cb264a5431f78a4238e5726adaedaa5fea777a62e4ec609661c107cf.png)

Custom SnackBar 自定义 SnackBar

# 全部代码:

```dart
import 'package:another_flushbar/flushbar.dart';
import 'package:flutter/material.dart';

class SnackBarView extends StatefulWidget {
  const SnackBarView({Key? key}) : super(key: key);

  @override
  _SnackBarViewState createState() => _SnackBarViewState();
}

class _SnackBarViewState extends State<SnackBarView> {

  @override
  Widget build(BuildContext context) {

    return Scaffold(
      body: Column(
       mainAxisAlignment: MainAxisAlignment.center,
        children: [
          _buildSnackBarButton()
        ],
      ),
    );
  }

  Widget _buildSnackBarButton() {
    return Column(
      children: [
        Center(
          child: InkWell(
            onTap: (){
              final snackBar = SnackBar(content: Text('Yay! A Default SnackBar!'));
              ScaffoldMessenger._of_(context).showSnackBar(snackBar);
            },
            child: Container(
              height: 40,
              width: 180,
              decoration: BoxDecoration(
                color: Colors._pinkAccent_,
                borderRadius: BorderRadius.all(Radius.circular(15))
              ),
              child: Center(
                child: Text("SnackBar",
                style: TextStyle(
                  color: Colors._white_,
                  fontSize: 16,

                ),),
              ),

            ),
          ),
        ),
        SizedBox(height: 10,),
        Center(
          child: InkWell(
            onTap: (){
              showFloatingFlushbar(context: context,
                  message: 'Custom Snackbar!',
                  isError: false);
              _// showSnackBar(
              //     context: context,
              //     message: 'Custom Snackbar!',
              //     isError: false);_ },
            child: Container(
              height: 40,
              width: 180,
              decoration: BoxDecoration(
                color: Colors._pinkAccent_,
                borderRadius: BorderRadius.all(Radius.circular(15))
              ),
              child: Center(
                child: Text("Custom SnackBar",

                style: TextStyle(
                  color: Colors._white_,
                  fontSize: 16,

                ),),
              ),

            ),
          ),
        ),
      ],
    );
  }
}

void showFloatingFlushbar( {@required BuildContext? context,
  @required String? message,
  @required bool? isError}){
  Flushbar? flush;
  bool? _wasButtonClicked;
  flush = Flushbar<bool>(
    title: "Hey User",
    message: message,
    backgroundColor: isError! ? Colors._red_ : Colors._blueAccent_,
    duration: Duration(seconds: 3),
    margin: EdgeInsets.all(20),


    icon: Icon(
      Icons._info_outline_,
      color: Colors._white_,),
    mainButton: FlatButton(
      onPressed: () {
        flush!.dismiss(true); _// result = true_ },
      child: Text(
        "ADD",
        style: TextStyle(color: Colors._amber_),
      ),
    ),) _// <bool> is the type of the result passed to dismiss() and collected by show().then((result){})_ ..show(context!).then((result) {

    });
}

void showSnackBar(
    {@required BuildContext? context,
    @required String? message,
    @required bool? isError}) {
  final snackBar = SnackBar(
    content: Text(
      message!,
      style: TextStyle(fontSize: 14.0, fontWeight: FontWeight._normal_),
    ),
    duration: Duration(seconds: 3),
    backgroundColor: isError! ? Colors._red_ : Colors._green_,
    width: 340.0,
    padding: const EdgeInsets.symmetric(
      horizontal: 8.0,
    ),
    behavior: SnackBarBehavior.floating,
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(10.0),
    ),

    action: SnackBarAction(
      label: 'Undo',
      textColor: Colors._white_,
      onPressed: () {},
    ),
  );
  ScaffoldMessenger._of_(context!).showSnackBar(snackBar);
}
```

# Conclusion:

# 结语:

在本文中，我已经简单介绍了 SnackBar 小部件的基本概况，您可以根据自己的选择修改这段代码。这是我对 SnackBar Widget On User Interaction 的一个小小介绍，它正在使用 Flutter 工作。

我希望这个博客将提供您尝试在您的 Flutter 项目探索，SnackBar 小工具充分的信息。

如果我做错了什么，请在评论中告诉我，我很乐意改进。

鼓掌如果这篇文章对你有帮助的话。

# GitHub Link:

# 链接:

找到 Flutter SnackBar Demo 的源代码:

https://github.com/flutter-devs/flutter_snackbar_demo

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
