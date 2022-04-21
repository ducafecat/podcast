---
title: 在 Flutter 添加页面过渡动画
tags: flutter
categories: 译文
date: 2022-04-21 22:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220421215950.png)

## 原文

> https://medium.com/@aayushkedawat/adding-page-transition-animations-in-flutter-8886a04fea3c

## 参考

- https://pub.dev/packages/page_animation_transition

## 正文

大家好，在这篇文章中，我们将学习如何添加动画，同时从一个页面到其他在 Flutter。我们将覆盖不同类型的动画和实现基本动画 Flutter 使用包页动画过渡。

[page_animation_transition](https://pub.dev/packages/page_animation_transition)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/40ee03520353bab2bece9861db71a77cafb0a46646b3b1470638736e5befbba3.gif)

### 引言

动画在提升用户体验方面起着至关重要的作用，但动画到底是什么呢？

设计语言，例如 Material，定义了在路线(或屏幕)之间转换时的标准行为。不过，有时候，自定义屏幕之间的转换可以使应用程序更加独特。

在本教程中，我们将使用包页面 [page_animation_transition](https://pub.dev/packages/page_animation_transition) 来简化在页面上添加转换。

**使用插件探索不同的转换**

### 第一步: 在 pubspec.yaml 中添加页面动画转换

[page_animation_transition](https://pub.dev/packages/page_animation_transition)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/0bb48e9c7e617e26a9f7544c014060d5be9d60ad5b746001b4678e41995ed981.png)

### 步骤 2: 在 PageOne 上导入库

假设您正在从 PageOne 过渡到 PageTwo

以下是图书馆支持的动画类型:

> _BottomToTopTransition
> TopToBottomTransition
> RightToLeftTransition
> LeftToRightTransition
> FadeAnimationTransition
> ScaleAnimationTransition
> RotationAnimationTransition
> TopToBottomFadedTransition
> BottomToTopFadedTransition
> RightToLeftFadedTransition
> LeftToRightFadedTransition_
>
> 底到上转换到底转换
> 右转移
> 左/右转变
> 淡化动画/转换
> 标量动画/转换
> 转动/动画/转变
> 上到下到过渡
> 底部/上部/下部/上部/下部/上部/下部/上部/下部/上部/下部/上部/
> 右转到/ftfaded/transition
> 左/右/插入/转换

### 步骤 3: 添加以下导航代码行

```dart
Navigator.of(context).push(PageAnimationTransition(page: const PageTwo(), pageAnimationType: BottomToTopTransition()));
```

对于预定义的路由:

```dart
onGenerateRoute: (settings) {
    switch (settings.name) {
      case '/pageTwo':
        return PageAnimationTransition(child: PageTwo(), pageAnimationType: LeftToRightFadedTransition());
        break;
      default:
        return null;
    }
  }
```

`Navigator.pushNamed(context, '/pageTwo');`

Pushnamed (context,’/pageTwo’) ;

Output:

输出:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/0d94c5df4678dc40275cc6671fe4de1d874c33b42d48ca7e2174b60719bf647c.gif)

### 其他类型转换的完整代码:

```dart
import 'package:page_animation_transition/animations/bottom_to_top_faded_transition.dart';
import 'package:page_animation_transition/animations/bottom_to_top_transition.dart';
import 'package:page_animation_transition/animations/fade_animation_transition.dart';
import 'package:page_animation_transition/animations/left_to_right_faded_transition.dart';
import 'package:page_animation_transition/animations/left_to_right_transition.dart';
import 'package:page_animation_transition/animations/right_to_left_faded_transition.dart';
import 'package:page_animation_transition/animations/right_to_left_transition.dart';
import 'package:page_animation_transition/animations/rotate_animation_transition.dart';
import 'package:page_animation_transition/animations/scale_animation_transition.dart';
import 'package:page_animation_transition/animations/top_to_bottom_faded.dart';
import 'package:page_animation_transition/animations/top_to_bottom_transition.dart';
import 'package:page_animation_transition/page_animation_transition.dart';
import 'page_two.dart';
import 'package:flutter/material.dart';class PageOne extends StatelessWidget {
  const PageOne({Key? key}) : super(key: key);[@override](http://twitter.com/override)
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Page Animation Transition'),
        centerTitle: true,
      ),
      body: SizedBox(
        width: MediaQuery.of(context).size.width,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: BottomToTopTransition()));
                },
                child: const Text('Bottom To Top')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: TopToBottomTransition()));
                },
                child: const Text('Top to bottom')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: RightToLeftTransition()));
                },
                child: const Text('Right To Left')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: LeftToRightTransition()));
                },
                child: const Text('Left to Right')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: FadeAnimationTransition()));
                },
                child: const Text('Faded')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: ScaleAnimationTransition()));
                },
                child: const Text('Scale')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: RotationAnimationTransition()));
                },
                child: const Text('Rotate')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: TopToBottomFadedTransition()));
                },
                child: const Text('Top to Bottom Faded')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: BottomToTopFadedTransition()));
                },
                child: const Text('Bottom to Top Faded')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: RightToLeftFadedTransition()));
                },
                child: const Text('Right to Left Faded')),
            ElevatedButton(
                onPressed: () {
                  Navigator.of(context).push(PageAnimationTransition(
                      page: const PageTwo(),
                      pageAnimationType: LeftToRightFadedTransition()));
                },
                child: const Text('Left to Right Faded')),
          ],
        ),
      ),
    );
  }
}
```

输出:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a609843013e13f572c6ecc1b53b400b836695a4b0630f2896d76a49efbbe47ef.gif)

## 总结

希望这个博客能帮助你深入了解 Flutter 的转变。谢谢阅读！如果有任何错误，请在评论中让我知道，这样我可以改进。如果这个博客对你有帮助，就鼓掌吧！

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
