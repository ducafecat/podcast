---
title: 在 Flutter 中探索 GetIt
tags: flutter
categories: 译文
date: 2021-09-02 00:00:00
---

![](2021-09-01-17-53-49.png)

## 原文

> https://medium.com/flutterdevs/explore-getit-in-flutter-8db723e9d7cf

## 参考

- https://pub.dev/packages/get_it

## 正文

它的 Flutter 小部件是建立使用一个现代框架。这就像是一种反应。在这里，我们从小部件开始创建任何应用程序。屏幕中的每个组件都是一个小部件。这个小部件描述了根据他目前的配置和状态，他的前景应该是什么样的。使您的小部件不具有直接依赖关系，可以使您的代码更好地组织，更容易测试和维护。但是现在您需要一种从 UI 代码访问这些对象的方法。当我来到 Flutter 从。小部件展示类似于它的想法和当前的设置和状态。Flutter 是一个免费的开源工具，用于开发移动、桌面、 web 应用程序，只需要一个代码库。

在本文中，我们将用 Flutter 获得它的包装来说明什么是 Flutter 获得它。在包的帮助下，以及如何使用他们在您的 Flutter 应用程序。那么让我们开始吧。

# Flutter :

Flutter 是谷歌的用户界面工具包，它可以帮助你在创纪录的时间内为移动设备、网络和桌面构建漂亮的、本地组合的应用程序。 Flutter 提供了很棒的开发工具，具有惊人的 hot reload 性能

# 返回文章页面搞定它:

软件包就是这样一种简单的服务定位器，在这个服务定位器中，你有一个中央注册中心，通过注册类，我们可以得到一个类的实例，我们使用它来代替继承的小部件或提供者来访问对象 Is。从你的用户界面。

服务定位器和依赖注入都是控制反转的一种形式。IOC 允许来自任何地方的请求，从注册其类类型到访问容器。

# 实施方案:

第一步: 添加依赖项

> 将依赖项添加到 pubspec ー yaml 文件。

依赖性:

```dart
dependencies:
  get_it: ^7.2.0
```

第二步: 进口

```dart
import 'package:get_it/get_it.dart';
```

第三步: 启用 AndriodX

```dart
org.gradle.jvmargs=-Xmx1536M
android.enableR8=true
android.useAndroidX=true
android.enableJetifier=true
```

# 代码实施:

在解释 GetIt 之前，我们将在下面的参考文献中给出一个在我们的代码中使用的 GitIt 方法。

这是我们的服务定位器。

```dart
GetIt getIt = GetIt._instance_;
```

接下来，我们创建了一个名为 getappmodel 的抽象类，它扩展了 ChangeNotifier。

```dart
abstract class GetItAppModel extends ChangeNotifier {
  void incrementCounter();
  int get counter;
}
```

现在我们已经创建了一个名为 getappmodelimplementation 的类，它从 getappmodel 类中扩展，我们已经创建了 incrementCounter \(\)方法，该方法增加计数器值。

```dart
class GetItAppModelImplementation extends GetItAppModel {

  int _counter = 0;

  GetItAppModelImplementation() {
  Future.delayed(Duration(seconds: 3)).then((_) => getIt.signalReady(this));
  }

  @override
  int get counter => _counter;

  @override
  void incrementCounter() {
    _counter++;
    notifyListeners();
  }
}
```

在这之后，我们将创建一个计数器应用程序，在这个程序中，我们已经在内部获取了两个文本部件，外部是 floatingActionButton，点击它将调用 getappmodel \(\)类，在计数器应用程序中，该值将在上面的列部件中增加，该列部件由 FutureBuilder 和 getIt.allReady \(\)包装，在未来的属性中定义。

```dart
FutureBuilder(
    future: getIt.allReady(),
    builder: (context, snapshot) {
      if (snapshot.hasData) {
        return Scaffold(

          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: <Widget>[
                Text(
                  'You have pushed the button this many times:',
                ),
                Text(
                  getIt<GetItAppModel>().counter.toString(),
                  style: Theme._of_(context).textTheme.headline4,
                ),
              ],
            ),
          ),
          floatingActionButton: FloatingActionButton(
            onPressed: getIt<GetItAppModel>().incrementCounter,
            tooltip: 'Increment',
            child: Icon(Icons._add_),
          ),
        );
      } else {
        return Column(
          mainAxisAlignment: MainAxisAlignment.center,
          mainAxisSize: MainAxisSize.min,
          children: [
            Text('Initialisation'),
            SizedBox(
              height: 16,
            ),
            CircularProgressIndicator(),
          ],
        );
      }
    })
```

# 全部代码:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_getit_exm/app_model.dart';
import 'package:flutter_getit_exm/main.dart';
class GetItExm extends StatefulWidget {
  GetItExm({Key? key, required this.title}) : super(key: key);

  final String title;

  @override
  _GetItExmState createState() => _GetItExmState();
}

class _GetItExmState extends State<GetItExm> {
  @override
  void initState() {
    getIt
        .isReady<GetItAppModel>()
        .then((_) => getIt<GetItAppModel>().addListener(update));
    super.initState();
  }

  @override
  void dispose() {
    getIt<GetItAppModel>().removeListener(update);
    super.dispose();
  }

  void update() => setState(() => {});

  @override
  Widget build(BuildContext context) {
    return Material(
      child: FutureBuilder(
          future: getIt.allReady(),
          builder: (context, snapshot) {
            if (snapshot.hasData) {
              return Scaffold(
                appBar: AppBar(
                  title: Text(widget.title),
                ),
                body: Center(
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: <Widget>[
                      Text(
                        'You have pushed the button this many times:',
                      ),
                      Text(
                        getIt<GetItAppModel>().counter.toString(),
                        style: Theme._of_(context).textTheme.headline4,
                      ),
                    ],
                  ),
                ),
                floatingActionButton: FloatingActionButton(
                  onPressed: getIt<GetItAppModel>().incrementCounter,
                  tooltip: 'Increment',
                  child: Icon(Icons._add_),
                ),
              );
            } else {
              return Column(
                mainAxisAlignment: MainAxisAlignment.center,
                mainAxisSize: MainAxisSize.min,
                children: [
                  Text('Waiting for initialisation'),
                  SizedBox(
                    height: 16,
                  ),
                  CircularProgressIndicator(),
                ],
              );
            }
          }),
    );
  }
}
```

# 结语:

在本文中，我解释了 Explore GetIt In Flutter，你可以根据自己的修改和实验，这个小介绍来自 Explore GetIt In Flutter 从我们这边的演示。

我希望这个博客将为您提供充分的信息，在尝试在您的 Flutter 项目探索 GetIt 在 Flutter 。我们向你展示了什么是探索和 Flutter 是在您的 Flutter 应用的工作，所以请尝试它。

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
