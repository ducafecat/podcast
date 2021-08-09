---
title: Flutter 动画转场欢迎屏 concentric_transition
tags: flutter
categories: 译文
date: 2021-08-10 00:00:00
---

![](2021-08-10-07-06-27.png)

## 原文

> https://medium.com/flutterdevs/explore-concentric-transition-in-flutter-82ef4194d3d9

## 代码

https://github.com/tiamo/flutter-concentric-transition/tree/master/example

## 参考

- https://pub.flutter-io.cn/packages/concentric_transition

## 正文

![](2021-08-10-06-27-08.png)

Flutter 小部件是使用现代框架构建的。这就像是一种反应。在这里，我们从小部件开始创建任何应用程序。屏幕中的每个组件都是一个小部件。这个小部件描述了根据他目前的配置和状态，他的前景应该是什么样的。小部件展示类似于它的想法和当前的设置和状态。

Flutter 自动化测试使您能够满足您的应用程序的高响应性，因为它有助于在您的应用程序中发现 bug 和各种问题。Flutter 是一个工具，开发移动，桌面，网络应用程序与代码 & 是一个免费和开放源码的工具。

在本文中，我们将用 Flutter concentric_transition 探索 Concentric Transition 在 Flutter。利用该软件包，可以很容易地实现 Flutter 向心过渡。那么让我们开始吧。

https://pub.dev/packages/concentric_transition

### Concentric Transition

Concentric Transition 插件是一个非常好的 Flutter 插件。用户可以使用这个插件创建一个动画类型的入门屏幕，并创建自定义动画屏幕，如同心页面路由器，自定义剪刀，等等，就像我们使用同心页面，然后为我们提供动画类型的页面路线，将我们从一个屏幕到另一个屏幕。

![](1.gif)
![](2.gif)
![](3.gif)
![](4.gif)
![](5.gif)
![](6.gif)

### 特点

- Concentric PageView 页面
- Concentric Clipper 剪切图
- Concentric PageRoute 转场路由

### 依赖包

你需要分别在你的代码中实现它:

- 第一步: 添加依赖项

将依赖项添加到 pubspec ー yaml 文件。

```yaml
dependencies:
  concentric_transition: ^1.0.1+1
```

- 第二步: 导入包

```dart
import 'package:concentric_transition/concentric_transition.dart';
```

- 第三步: 运行 Run flutter package get

### 加入代码

> 你需要分别在你的代码中实现它:

在定义 ConcentricPageView 之前，我们将创建一个类，在这个类中，我们将定义其标题、图像、颜色等，如下面的代码引用所示。

```dart
class OnboardingData {
  final String? title;
  final Image? icon;
  final Color bgColor;
  final Color textColor;

  OnboardingData({
    this.title,
    this.icon,
    this.bgColor = Colors.white,
    this.textColor = Colors.black,
  });
}
```

在此之后，我们将在一个列表中定义入职类数据，该列表将在屏幕上显示给我们的数据。让我们用下面的代码来理解一下。

```dart
final List<OnboardingData> onBoardingData = [
  OnboardingData(
    icon:Image.asset('assets/images/melon.png'),
    title: "Fresh Lemon\nfruits",
    //textColor: Colors.white,
    bgColor: Color(0xffCFFFCE),
  ),
  OnboardingData(
    icon:Image.asset('assets/images/orange.png'),
    title: "Fresh Papaya\nfruits",
    bgColor: Color(0xffFFE0E1),
  ),
  OnboardingData(
    icon:Image.asset('assets/images/papaya.png'),
    title: "Fresh Papaya\nfruits",
    bgColor: Color(0xffFCF1B5),
    //textColor: Colors.white,
  ),
];
```

现在，我们将在动画的颜色和持续时间所在的主体中使用 ConcentricPageView 小部件。使用列小部件，我们将在一个框中显示图像，并在图像下方显示其标题。让我们用下面的代码来理解一下。

```dart
ConcentricPageView(
  colors: colors,
  radius: 30,
  curve: Curves.ease,
  duration: Duration(seconds: 2),
  itemBuilder: (index, value) {
    OnboardingData page = onBoardingData[index % onBoardingData.length];
    return Container(
      child: Theme(
        data: ThemeData(
          textTheme: TextTheme(
            headline6: TextStyle(
              color: page.textColor,
              fontWeight: FontWeight.w600,
              fontFamily: 'Helvetica',
              letterSpacing: 0.0,
              fontSize: 17,
            ),
            subtitle2: TextStyle(
              color: page.textColor,
              fontWeight: FontWeight.w300,
              fontSize: 18,
            ),
          ),
        ),
        child: OnBoardingPage(onboardingDataPage: page),
      ),
    );
  },
),
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-08-10-06-32-55.png)

### 完整代码

```dart
import 'dart:ui';
import 'package:concentric_transition/concentric_transition.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:flutter/painting.dart';



class ConcentricPageViewDemo extends StatelessWidget {
  final List<OnboardingData> onBoardingData = [
    OnboardingData(
      icon:Image.asset('assets/images/melon.png'),
      title: "Fresh Lemon\nfruits",
      //textColor: Colors.white,
      bgColor: Color(0xffCFFFCE),
    ),
    OnboardingData(

      icon:Image.asset('assets/images/orange.png'),
      title: "Fresh Papaya\nfruits",
      bgColor: Color(0xffFFE0E1),
    ),
    OnboardingData(

      icon:Image.asset('assets/images/papaya.png'),
      title: "Fresh Papaya\nfruits",
      bgColor: Color(0xffFCF1B5),
      //textColor: Colors.white,
    ),
  ];

  List<Color> get colors => onBoardingData.map((p) => p.bgColor).toList();

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        body: ConcentricPageView(
          colors: colors,
          radius: 30,
          curve: Curves.ease,
          duration: Duration(seconds: 2),
          itemBuilder: (index, value) {
            OnboardingData page = onBoardingData[index % onBoardingData.length];
            return Container(
              child: Theme(
                data: ThemeData(
                  textTheme: TextTheme(
                    headline6: TextStyle(
                      color: page.textColor,
                      fontWeight: FontWeight.w600,
                      fontFamily: 'Helvetica',
                      letterSpacing: 0.0,
                      fontSize: 17,
                    ),
                    subtitle2: TextStyle(
                      color: page.textColor,
                      fontWeight: FontWeight.w300,
                      fontSize: 18,
                    ),
                  ),
                ),
                child: OnBoardingPage(onboardingDataPage: page),
              ),
            );
          },
        ),
      ),
    );
  }
}

class OnBoardingPage extends StatelessWidget {
  final OnboardingData onboardingDataPage;

  const OnBoardingPage({
    Key? key,
    required this.onboardingDataPage,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      margin: EdgeInsets.symmetric(
        horizontal: 30.0,
      ),
      child: Column(
//        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          _buildPicture(context),
          SizedBox(height: 30),
          _buildText(context),
        ],
      ),
    );
  }

  Widget _buildText(BuildContext context) {
    return Text(
      onboardingDataPage.title!,
      style: Theme.of(context).textTheme.headline6,
      textAlign: TextAlign.center,
    );
  }

  Widget _buildPicture(
      BuildContext context, {
        double size = 190,
        double iconSize = 170,
      }) {
    return Container(
      width: size,
      height: size,
      decoration: BoxDecoration(
        borderRadius: BorderRadius.all(Radius.circular(60.0)),
        color: onboardingDataPage.bgColor
//            .withBlue(page.bgColor.blue - 40)
            .withGreen(onboardingDataPage.bgColor.green + 20)
            .withRed(onboardingDataPage.bgColor.red - 100)
            .withAlpha(90),
      ),
      padding:EdgeInsets.all(15),
      margin: EdgeInsets.only(
        top: 140,
      ),
      child:onboardingDataPage.icon,
    );
  }
}

class OnboardingData {
  final String? title;
  final Image? icon;
  final Color bgColor;
  final Color textColor;

  OnboardingData({
    this.title,
    this.icon,
    this.bgColor = Colors.white,
    this.textColor = Colors.black,
  });
}
```

### Conclusion

在这篇文章中，我解释了探索 Concentric Transition Flutter，你可以根据自己的修改和实验，这个小介绍是从探索 Concentric Transition Flutter 从我们这边演示。

我希望这个博客将提供您尝试在您的 Flutter 项目探索 Concentric Transition 充分的信息。我们向您展示了什么探索 Concentric Transition 在 Flutter 是和工作在您的 Flutter 应用，所以请尝试它。

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
