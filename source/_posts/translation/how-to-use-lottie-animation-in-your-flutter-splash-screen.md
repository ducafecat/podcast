---
title: flutter 启动屏幕使用 Lottie 动画
tags: flutter
categories: 译文
date: 2021-05-27 00:00:00
---

![](2021-05-27-09-14-32.png)

## 猫哥说

因为出差关系来了重庆，很美的一个城市，走在街道上感觉就是在爬山，生活节奏相对比较慢，希望疫情远离我们。

感谢群里重庆好友能抽时间出来聚会。

正题开始

lottie 是一个夸平台的动画库，用这个可以做出酷炫动画。

其实作为一个前端还是要稍微会一点点美术、动画、几何数学。

https://lottiefiles.com/

https://github.com/airbnb/lottie-web

![](2021-05-28-16-29-00.png)

![](2021-05-28-16-30-44.png)

![](2021-05-28-16-31-07.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://mamuseferha.medium.com/how-to-use-lottie-animation-in-your-flutter-splash-screen-788f1380641d

## 代码

https://github.com/debbsefe/Lottie-Animation-Splash-Screen

## 参考

- https://lottiefiles.com/
- https://github.com/airbnb/lottie-web
- https://github.com/debbsefe/Lottie-Animation-Splash-Screen

## 正文

在构建移动应用程序时，启动画面非常常见。它们通常是在应用程序开始时显示的静态屏幕。这个屏幕可以帮助你告诉你的用户应用程序是关于什么的，通过显示你的应用程序标志或应用程序名称。

如果你想更进一步，真正吸引用户的注意力，可以考虑在启动画面上使用动画图片。使用 Lottie 显示一个动画图像就像在你的应用程序中使用 Image 小部件一样简单。

### 开始

首先，创建一个新的 flutter 项目。

```sh
flutter pub add lottie
```

通过粘贴以下代码创建启动画面小部件。

```dart
class SplashScreen extends StatefulWidget {
  const SplashScreen({Key key}) : super(key: key);

  @override
  _SplashScreenState createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen>
    with TickerProviderStateMixin {
  AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: Duration(seconds: (5)),
      vsync: this,
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Lottie.asset(
        'assets/splash_lottie.json',
        controller: _controller,
        height: MediaQuery.of(context).size.height * 1,
        animate: true,
        onLoaded: (composition) {
          _controller
            ..duration = composition.duration
            ..forward().whenComplete(() => Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(builder: (context) => HomePage()),
                ));
        },
      ),
    );
  }
}
```

Splash screen 小部件是一个有状态小部件，它在其 build 方法中保存 Lottie 文件。动画控制器在 initState 中创建，并在控制器属性中使用。

若要在动画完成后导航到下一个页面，请使用带有 LottieComposition 的 onLoaded 回调。这允许您有更多的信息和控制的 Lottie 文件。

```dart
onLoaded: (composition) {
  _controller
  ..duration = composition.duration
  ..forward().whenComplete(() => Navigator.pushReplacement(
    context,
    MaterialPageRoute(builder: (context) => HomePage()),));
    },
```

动画完成后，导航到下一页。

我在代码中添加了一个启动画面导航到的主页小部件。

```dart
class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(child: Text('Homepage')),
    );
  }
}
```

以 scaffold 为中心的简单文本小部件。

现在你可以继续为你的用户创建更具视觉吸引力的应用了。

![](2021-05-27-09-02-46.png)

不要忘记将 Lottie 文件作为资产添加到 pubspec.yaml 中，否则，动画将不会显示。你也可以在 GitHub 上找到完整的项目。

https://github.com/debbsefe/Lottie-Animation-Splash-Screen

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
