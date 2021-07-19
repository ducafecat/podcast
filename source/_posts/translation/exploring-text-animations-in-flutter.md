---
title: 在 Flutter 中实现文字动画
tags: flutter
categories: 译文
date: 2021-07-19 00:00:00
---

![](2021-07-19-07-42-06.png)

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/exploring-text-animations-in-flutter-9b74103940d2

## 参考

- https://pub.dev/packages/animated_text_kit

## 正文

![](2021-07-19-07-12-36.png)

动画期望在更新您的应用程序的整体客户机体验从视觉分析，运动，和自定义动画的巨大的一部分，你真的可以想象！.就像协调到应用程序中的一些不同的东西一样，动画应该是有帮助的，而不是基本上是一个正常的复杂格式。

在 Flutter，动画是直接做到的，而且很多古怪的东西可以用比原生 Android 更少的努力来完善。

在本帖中，我们将探索 Flutter 文本动画。我们还将实现一个演示程序的文本动画，并显示一个冷静和美丽的文本动画收集使用的动画工具包在您的 Flutter 应用程序。

https://pub.dev/packages/animated_text_kit

### 简介

一个 Flutter 小工具包，包含一些很酷的和伟大的内容动画分类。我们将制作非凡的和优秀的内容动画利用动画 `animated_text_kit` 工具包包。

### 属性

以下是 AnimatedTextKit 的一些属性:

- animatedTexts: 动画文本: 此属性用于列出[ AnimatedText ] ，以便随后在动画中显示
- isRepeatingAnimation: 重复动画: 此属性用于设置动画是否应该通过将其值更改为 false 来重复。默认情况下，它被设置为 true
- totalRepeatCount: 累计重复计数: 此属性用于设置动画应重复的次数。默认情况下，设置为 3
- repeatForever: 此属性用于设置动画是否会永远重复。如果你想永远重复，还需要将动画设置为 true
- onFinished: 此属性用于将 onFinished [ VoidCallback ]添加到动画小部件。只有当[ isrepetinganimation ]设置为 false 时，此方法才会运行
- onTap: 此属性用于将 onTap [ VoidCallback ]添加到动画小部件
- stopPauseOnTap: 此属性用于暂停，是否需要点击删除剩余的暂停时间？.默认情况下，它被设置为 false

### 安装

- 第一步: 添加依赖项

> 将依赖项添加到 pubspec.yaml 文件。

```yaml
animated_text_kit: ^4.2.1
```

- 第二步: 导入

```dart
import 'package:animated_text_kit/animated_text_kit.dart';
```

- 第三步: 在应用程序的根目录中运行 flutter 软件包。

```sh
flutter packages get
```

### 如何实现 dart 文件中的代码

你需要分别在你的代码中实现它:

> 在 lib 文件夹中创建一个名为 home_page_screen.dart 的新 dart 文件。

我们将在主页屏幕上创建九个不同的按钮，当用户点击按钮时，动画将工作。所有按钮都有不同的动画效果。我们将在下面深入讨论这个问题。当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-07-19-07-19-27.png)

#### 旋转动画文字

在正文中，我们将添加一个列小部件。在这个小部件中，添加一个具有高度和宽度的 Container。其子属性，添加一个 `_rotate()` 小部件。

```dart
Center(
  child: Column(
    crossAxisAlignment: CrossAxisAlignment.center,
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      Container(
        decoration: BoxDecoration(color: Colors.red),
        height: 300.0,
        width: 350.0,
        child: Center(
          child: _rotate(),
        ),
      ),
    ],
  ),
)
```

在 `_rotate()` 小部件中。我们将返回 Row 小部件。在内部，添加文本和 defaultextstyle()。它是子属性，我们将添加 AnimatedTextKit()小部件。在里面，我们将添加 repeatForever 是真实的，isRepeatingAnimation 也是真实的，并添加 animatedtext。在 animatedtext 中，我们将添加三个 RotateAnimatedText()。用户还可以添加持续时间，旋转。

```dart
Widget _rotate(){
  return Row(
    mainAxisSize: MainAxisSize.min,
    children: <Widget>[
      const SizedBox(width: 10.0, height: 100.0),
      const Text(
        'Flutter',
        style: TextStyle(fontSize: 40.0),
      ),
      const SizedBox(width: 15.0, height: 100.0),
      DefaultTextStyle(
        style: const TextStyle(
          fontSize: 35.0,
        ),
        child: AnimatedTextKit(
            repeatForever: true,
            isRepeatingAnimation: true,
            animatedTexts: [
          RotateAnimatedText('AWESOME'),
          RotateAnimatedText('Text'),
          RotateAnimatedText('Animation'),
        ]),
      ),
    ],
  );
}
```

当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](rotate.gif)

#### 打字机动画文字

在正文中，我们将添加与上面相同的方法。但是在子属性中的更改，添加一个 `_typer` 小部件。

```dart
Widget _typer(){
  return SizedBox(
    width: 250.0,
    child: DefaultTextStyle(
      style: const TextStyle(
        fontSize: 30.0,
        fontFamily: 'popin',
      ),
      child: AnimatedTextKit(
        isRepeatingAnimation: true,
          animatedTexts: [
            TyperAnimatedText('When you talk, you are only repeating'
                ,speed: Duration(milliseconds: 100)),
            TyperAnimatedText('something you know.But if you listen,'
                ,speed: Duration(milliseconds: 100)),
            TyperAnimatedText(' you may learn something new.'
                ,speed: Duration(milliseconds: 100)),
            TyperAnimatedText('– Dalai Lama'
                ,speed: Duration(milliseconds: 100)),
          ]
    ),
  ),
  );
}
```

在这个小部件中，我们将返回 SizedBox()。在内部，我们将添加 DefaultTextStyle()并添加 AnimatedTextKit()小部件。在这个小部件中，我们将添加 animatedtext。在内部，我们将添加四个带有速度持续时间的 TyperAnimatedText()。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](type.gif)

#### 淡出动画文本

在正文中，我们将添加与上面相同的方法。但是如果改变子属性，添加一个 `_fade` 小部件。

```dart
Widget _fade(){
  return SizedBox(
    child: DefaultTextStyle(
      style: const TextStyle(
        fontSize: 32.0,
        fontWeight: FontWeight.bold,
      ),
      child: Center(
        child: AnimatedTextKit(
          repeatForever: true,
          animatedTexts: [
            FadeAnimatedText('THE HARDER!!',
                duration: Duration(seconds: 3),fadeOutBegin: 0.9,fadeInEnd: 0.7),
            FadeAnimatedText('YOU WORK!!',
                duration: Duration(seconds: 3),fadeOutBegin: 0.9,fadeInEnd: 0.7),
            FadeAnimatedText('THE LUCKIER!!!',
                duration: Duration(seconds: 3),fadeOutBegin: 0.9,fadeInEnd: 0.7),
            FadeAnimatedText('YOU GET!!!!',
                duration: Duration(seconds: 3),fadeOutBegin: 0.9,fadeInEnd: 0.7),
          ],
        ),
      ),
    ),
  );

}
```

在这个小部件中，我们将返回 SizedBox()。在内部，我们将添加 DefaultTextStyle()并添加 AnimatedTextKit()小部件。在这个小部件中，我们将添加 animatedtext。在内部，我们将添加 4 个 FadeAnimatedText() ，其中包括速度持续时间、 fadeOutBegin 和 fadeInEnd。比 fadeInEnd 要好。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](fade.gif)

#### 缩放动画文字

在正文中，我们将添加与上面相同的方法。但是在子属性中的更改，添加一个 `_scale` 小部件。

```dart
Widget _scale(){
  return SizedBox(
    child: DefaultTextStyle(
      style: const TextStyle(
        fontSize: 50.0,
        fontFamily: 'SF',
      ),
      child: Center(
        child: AnimatedTextKit(
          repeatForever: true,
          animatedTexts: [
            ScaleAnimatedText('Eat',scalingFactor: 0.2),
            ScaleAnimatedText('Code',scalingFactor: 0.2),
            ScaleAnimatedText('Sleep',scalingFactor: 0.2),
            ScaleAnimatedText('Repeat',scalingFactor: 0.2),
          ],
        ),
      ),
    ),
  );
}
```

在这个小部件中，我们将返回 SizedBox()。在内部，我们将添加 DefaultTextStyle()并添加 AnimatedTextKit()小部件。在这个小部件中，我们将添加 animatedtext。在内部，我们将添加四个带有 scalingFactor 的 ScaleAnimatedText()。scalingFactor 设置了动画文本的缩放因子。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](scale.gif)

#### TextLiquidFill 动画

在正文中，我们将添加与上面相同的方法。但是如果更改子属性，则添加一个 `_textLiquidFillAnimation` 小部件。

```dart
Widget _textLiquidFillAnimation(){
  return SizedBox(
    child: Center(
      child: TextLiquidFill(
        text: 'Flutter Devs',
        waveDuration: Duration(seconds: 5),
        waveColor: Colors.blue,
        boxBackgroundColor: Colors.green,
        textStyle: TextStyle(
          fontSize: 50.0,
          fontWeight: FontWeight.bold,
        ),
      ),
    ),
  );
}
```

在这个小部件中，我们将返回 SizedBox()。在内部，我们将添加 TextLiquidFill()小部件。在这个小部件中，我们将添加文本、 waveDuration、 waveColor 和 boxBackgroundColor。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](textLiquidFill.gif)

#### 动画文字

在正文中，我们将添加与上面相同的方法。但是如果改变子属性，添加一个 `_wave` 小部件。

```dart
Widget _wavy(){
  return DefaultTextStyle(
    style: const TextStyle(
      fontSize: 25.0,
    ),
    child: AnimatedTextKit(
      animatedTexts: [
        WavyAnimatedText("Flutter is Google's UI toolkit,",
            speed: Duration(milliseconds: 200)),
        WavyAnimatedText('for building beautiful Apps',
            speed: Duration(milliseconds: 200)),
      ],
      isRepeatingAnimation: true,
      repeatForever: true,
    ),
  );
}
```

在这个小部件中，我们将返回 DefaultTextStyle()。在内部，我们将添加 AnimatedTextKit()小部件。在这个小部件中，我们将添加 animatedtext。在内部，我们将添加两个 WavyAnimatedText()和文本的速度持续时间。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](wavy.gif)

#### 闪烁动画文字

在正文中，我们将添加与上面相同的方法。但是在子属性中的更改，添加一个 `_flicker` 小部件。

```dart
Widget _flicker(){
  return SizedBox(
    width: 250.0,
    child: DefaultTextStyle(
      style: const TextStyle(
        fontSize: 30,
      ),
      child: AnimatedTextKit(
        repeatForever: true,
        animatedTexts: [
          FlickerAnimatedText('FlutterDevs specializes in creating,',
              speed: Duration(milliseconds: 1000),entryEnd: 0.7),
          FlickerAnimatedText('cost-effective and',
              speed: Duration(milliseconds: 1000),entryEnd: 0.7),
          FlickerAnimatedText("efficient applications!",
              speed: Duration(milliseconds: 1000),entryEnd: 0.7),
        ],
      ),
    ),
  );
}
```

在这个小部件中，我们将返回 SizedBox()。在内部，我们将添加 DefaultTextStyle()并添加 AnimatedTextKit()小部件。在这个小部件中，我们将添加 animatedtext。在内部，我们将添加四个具有 entryEnd 和速度的 FlickerAnimatedText()。entryEnd 被标记为文本闪烁输入间隔的结束。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](flicker.gif)

#### 彩色动画文本

在正文中，我们将添加与上面相同的方法。但是在子属性中的更改，添加一个 `_colorize` 的小部件。

```dart
Widget _colorize(){
  return SizedBox(
    child: Center(
      child: AnimatedTextKit(
        animatedTexts: [
          ColorizeAnimatedText(
            'Mobile Developer',
            textStyle: colorizeTextStyle,
            colors: colorizeColors,
          ),
          ColorizeAnimatedText(
            'Software Testing',
            textStyle: colorizeTextStyle,
            colors: colorizeColors,
          ),
          ColorizeAnimatedText(
            'Software Engineer',
            textStyle: colorizeTextStyle,
            colors: colorizeColors,
          ),
        ],
        isRepeatingAnimation: true,
        repeatForever: true,
      ),
    ),
  );
}
```

在这个小部件中，我们将返回 SizedBox()。在内部，我们将添加 AnimatedTextKit()小部件。在这个小部件中，我们将添加 animatedtext。在内部，我们将添加三个带有 textStyle 和颜色的 colorizeanmatedtext()。

```dart
List<MaterialColor> colorizeColors = [
  Colors.red,
  Colors.yellow,
  Colors.purple,
  Colors.blue,
];

static const colorizeTextStyle = TextStyle(
  fontSize: 40.0,
  fontFamily: 'SF',
);
```

用户可以根据文本改变颜色。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频

![](colorize.gif)

#### 打字机动画文字

在正文中，我们将添加与上面相同的方法。但是如果更改子属性，则添加 `_typeWriter` 小部件。

```dart
Widget _typeWriter(){
  return SizedBox(
    child: DefaultTextStyle(
      style: const TextStyle(
        fontSize: 30.0,
      ),
      child: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Center(
          child: AnimatedTextKit(
            repeatForever: true,
            animatedTexts: [
              TypewriterAnimatedText('FlutterDevs specializes in creating cost-effective',
                  curve: Curves.easeIn,speed: Duration(milliseconds: 80)),
              TypewriterAnimatedText('and efficient applications with our perfectly crafted,',
                  curve: Curves.easeIn,speed: Duration(milliseconds: 80)),
              TypewriterAnimatedText('creative and leading-edge flutter app development solutions',
                  curve: Curves.easeIn,speed: Duration(milliseconds: 80)),
              TypewriterAnimatedText('for customers all around the globe.',
                  curve: Curves.easeIn,speed: Duration(milliseconds: 80)),
            ],
          ),
        ),
      ),
    ),
  );
}
```

在这个小部件中，我们将返回 SizedBox()。在内部，我们将添加 DefaultTextStyle()并添加 AnimatedTextKit()小部件。在这个小部件中，我们将添加 animatedtext。内部，我们将添加四个打字机动画文本()与曲线和速度。当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](typeWriter.gif)

#### 所有代码

```dart
import 'package:flutter/material.dart';
import 'package:flutter_animation_text/colorize_animation_text.dart';
import 'package:flutter_animation_text/fade_animation_text.dart';
import 'package:flutter_animation_text/flicker_animation_text.dart';
import 'package:flutter_animation_text/rotate_animation_text.dart';
import 'package:flutter_animation_text/scale_animation_text.dart';
import 'package:flutter_animation_text/text_liquid_fill_animation.dart';
import 'package:flutter_animation_text/typer_animation_text.dart';
import 'package:flutter_animation_text/typewriter_animated_text.dart';
import 'package:flutter_animation_text/wavy_animation_text.dart';


class HomePageScreen extends StatefulWidget {
  @override
  _HomePageScreenState createState() => _HomePageScreenState();
}

class _HomePageScreenState extends State<HomePageScreen> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Color(0xffFFFFFF),
      appBar: AppBar(
        backgroundColor: Colors.black,
        title: Text('Flutter Animations Text Demo'),
        automaticallyImplyLeading: false,
        centerTitle: true,
      ),
      body: Center(
          child: Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: <Widget>[

                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Rotate Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => RotateAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),
                ),
                SizedBox(height: 8,),
                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Typer Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => TyperAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),
                SizedBox(height: 8,),
                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Fade Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => FadeAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),
                SizedBox(height: 8,),
                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Scale Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => ScaleAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),
                SizedBox(height: 8,),
                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('TextLiquidFill Animation',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => TextLiquidFillAnimation()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),
                SizedBox(height: 8,),
                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Wavy Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => WavyAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),
                SizedBox(height: 8,),
                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Flicker Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => FlickerAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),
                SizedBox(height: 8,),
                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Colorize Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => ColorizeAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),
                SizedBox(height: 8,),

                // ignore: deprecated_member_use
                RaisedButton(
                  child: Text('Typewriter Animation Text',style: TextStyle(color: Colors.black),),
                  color: Colors.tealAccent,
                  onPressed:() {
                    Navigator.of(context).push(
                        MaterialPageRoute(builder:(context) => TypewriterAnimationText()));
                  },
                  shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(20))),
                  padding: EdgeInsets.all(13),

                ),

              ],
            ),
          )
      ), //center
    );
  }
}
```

### 总结

在这篇文章中，我已经简单地解释了文本动画的基本结构，您可以根据自己的选择修改这个代码。这是一个小的介绍文本动画用户交互从我这边，它的工作使用扑动。

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
