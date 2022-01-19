---
title: Flutter 创建动画屏幕
tags: flutter
categories: 译文
date: 2022-01-21 00:00:00
---

![](2022-01-20-07-40-53.png)

## 原文

> https://medium.com/@baran.aslankan4/create-an-animation-screen-in-flutter-683d9a534d67

## 代码

https://github.com/ducafecat/getx_quick_start

## 参考

- https://pub.flutter-io.cn/packages/animated_text_kit
- https://pub.flutter-io.cn/packages/simple_animations
- https://pub.flutter-io.cn/packages/google_fonts

## 正文

大家好，今天我要向大家展示如何在 Flutter 中创建一个动画屏幕，就像这样;

### 安装依赖项

您可以从 [pub.dev](http://pub.dev/) 获得依赖项

这些是我在我的项目中使用的版本:

```dart
animated_text_kit: ^4.2.1
simple_animations: ^3.1.1
google_fonts: ^2.1.0
```

在获得依赖关系之后，我们在编码之前还有一个步骤。

有一个叫做 Liquid Studio 的应用程序是由 Felix Blaschke 创建的，这个应用程序生成飞镖代码来制作一些动画。

https://felixblaschke.github.io/liquid-studio/#/

### 现在是设计部分:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/5be7678fba1af2347fb75002f1218ad03d750dcf73b3920be7b9c8037cbd88f3.png)

有许多选项和颜色选择，为此我使用了默认设置和颜色。

现在，为了使它动画化，我们需要添加另一个层称为 Plasma。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/8b9650e51a2158a46df6b8e5446636e6a318c16d561c1565e3f77a97961eb195.png)

点击添加图层按钮并选择 Plasma。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/f557b6269ac54cb256f322e18fb20b10751e6ada7419f4c4f06e1a8196a55219.png)

现在你可以自定义渐变层和 plasma layers，我为自己做了一些定制。

现在要导出，点击左上角的导出按钮。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/4c5e476cc5f53fdc7280dd538fc7abfa8b4c62350e8e5745515092b32d34c52b.png)

单击导出场景以导出所有图层。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/ddf8e9fce504d7bb16759ddb8df364ba60359d2e7283a5434376b6ef2631f0ce.png)

现在我们有了彩色动画的代码，我们将在 IDE 中使用这段代码，但在此之前，让我们先创建类结构。

让我们创建一个有状态的小部件并首先返回一个 Scaffold。

现在，我们希望跳过动画页当我们点击屏幕，要做到这一点，我们可以使用 GestureDetector 小部件。

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(

      ),
    );
  }
```

现在粘贴我们在 liquid studio 中导出的代码，作为 GestureDetector 小部件的子部件。

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        child: Container(
          decoration: BoxDecoration(
            gradient: LinearGradient(
              tileMode: TileMode.mirror,
              begin: Alignment.topRight,
              end: Alignment.bottomRight,
              colors: [
                Color(0xfff44336),
                Color(0xff2196f3),
              ],
              stops: [
                0,
                1,
              ],
            ),
            backgroundBlendMode: BlendMode.srcOver,
          ),
         PlasmaRenderer(
              type: PlasmaType.infinity,
              particles: 10,
              color: Color(0x442eaeaa),
              blur: 0.31,
              size: 1,
              speed: 1.86,
              offset: 0,
              blendMode: BlendMode.plus,
              particleType: ParticleType.atlas,
              variation1: 0,
              variation2: 0,
              variation3: 0,
              rotation: 0,
            ),
        ),
      ),
    );
  }
```

它应该看起来像这样，现在你会有一个错误，这是因为我们还没有导入简单的动画。

导入类顶部的简单动画库。

```dart
import 'package:simple_animations/simple_animations.dart';
```

现在我们的动画颜色已经准备好了，可以点击导航另一个页面，我们将使用 GestureDetector 小部件的点击方法。

在 Container Widget 下，create on tap method 并导航到所需的页面。

```dart
onTap: (){
          Navigator.of(context).pushReplacementNamed('/home');
        },
```

现在，当我们测试代码时，应该是这样的:

现在只剩下一件事情要做，那就是动画文字部分。

在这段代码中，Container 小部件有一个子元素，那就是 plasma 渲染器，plasma 渲染器可以帮助我们创建颜色动画。现在，为了向屏幕添加文本，我们需要向 Container Widget 添加另一个子元素。要添加子元素，我们需要使用 Stack Widget 覆盖所有子元素。

点击 PlasmaRenderer Widget，然后点击点击 Widget 后出现的灯泡。

选择 wrap with Column 选项。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/21e150d28b37fbc38aec62ee85779ee42bd16aa9a6eb1b6c8eb41e1b8439ed6d.png)

现在我们必须在这里停下来！

让我告诉你为什么我选择用列选择，它也可以是行。

我们将使用 Stack Widget，这个 Widget 可以帮助我们把 Widget 放在彼此之上，基本上就是叠加 Widget。

但是我为什么要选择专栏呢？

因为堆栈，列和行窗口小部件没有子窗口，他们有子窗口，如果我选择窗口小部件而不是列，那么我将不得不创建括号和改变子窗口小部件，所以我选择列的原因基本上是为了更快地编码。这一点都不重要，但我只是想提一下

所以我们已经创建了我们的堆栈小部件，其中一个子部件是 PlasmaRenderer，现在我们要添加动画文本作为子部件，但在创建中心小部件之前，让我们在屏幕中央显示文本。

为了获得动画文本，我们需要使用 AnimatedTextKit 小部件，因此我们必须导入库。

```dart
import 'package:animated_text_kit/animated_text_kit.dart';Center(
          child: AnimatedTextKit( ),
              ),
```

在 PlasmaRenderer 之后添加此代码。

获得了一个名为 animatedTexts 的列表，这个列表将包含我们想要在屏幕上显示的所有文本。

在 pub.dev 中，AnimatedTextKit 包中有许多文本动画，我选择了旋转文本，但是您可以尝试不同的动画。

```dart
animatedTexts: [
                    RotateAnimatedText('Hello',textStyle: const TextStyle(fontSize: 32,fontWeight: FontWeight.bold,color: Colors.white)),
                    RotateAnimatedText('Animation screen',textStyle: const TextStyle(fontSize: 32,fontWeight: FontWeight.bold,color: Colors.white)),
                    RotateAnimatedText('Click to get started',textStyle: const TextStyle(fontSize: 32,fontWeight: FontWeight.bold,color: Colors.white)),
                  ],
```

在这里 RotateAnimatedText 采用我们的字符串，你可以添加样式来改变字体大小和粗体，此外你还可以导入谷歌字体和添加一些自定义字体。对于颜色部分，我选择了白色，你可以选择任何你想要的颜色。

现在文本已经完成，但是我们需要一些动画时间的定制。

我们可以使用一些方法来帮助我们定制我们的动画。

- totalRepeatCount: 您可以编写希望动画重复的次数
- pause: 动画文本之间的暂停时间

```dart
totalRepeatCount: 1,
pause: const Duration(milliseconds: 3000),
```

将这些内容写入 AnimatedTextKit 小部件。

我希望它只执行一次，所以我设置重复计数 1。

Pause 采用 Duration 方法，我将其设置为 3000 毫秒，也就是 3 秒。

整个代码应该是这样的:

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        child: Container(
          decoration: BoxDecoration(
            gradient: LinearGradient(
              tileMode: TileMode.mirror,
              begin: Alignment.topLeft,
              end: Alignment.bottomRight,
              colors: [
                Color(0xffff1100),
                Color(0xff008eff),
              ],
              stops: [
                0,
                1,
              ],
            ),
            backgroundBlendMode: BlendMode.srcOver,
          ),
          child: Stack(
            children:[
               PlasmaRenderer(
              type: PlasmaType.infinity,
              particles: 10,
              color: Color(0x442eaeaa),
              blur: 0.31,
              size: 1,
              speed: 1.86,
              offset: 0,
              blendMode: BlendMode.plus,
              particleType: ParticleType.atlas,
              variation1: 0,
              variation2: 0,
              variation3: 0,
              rotation: 0,
            ),
              Center(
                child: AnimatedTextKit(
                  animatedTexts: [
                    RotateAnimatedText('Hello',textStyle: const TextStyle(fontSize: 32,fontWeight: FontWeight.bold,color: Colors.white)),
                    RotateAnimatedText('Animation screen',textStyle: const TextStyle(fontSize: 32,fontWeight: FontWeight.bold,color: Colors.white)),
                    RotateAnimatedText('Click to get started',textStyle: const TextStyle(fontSize: 32,fontWeight: FontWeight.bold,color: Colors.white)),
                  ],
                      totalRepeatCount: 1,
                      pause: const Duration(milliseconds: 3000),
                ),
              ),],
          ),
        ),
          onTap: (){
            Navigator.of(context).pushReplacementNamed('/home'); }
      ),
    );
  }
```

您可以自定义您的动画和使用更多的方法或更多的动画文本根据您的口味。

这就是我今天的全部内容，谢谢阅读，希望你喜欢，下次写作时再见，祝你有个愉快的一天;

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
