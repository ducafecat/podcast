---
title: 10 个 Flutter 组件推荐 - 3
tags: flutter
categories: 译文
date: 2022-05-15 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220515051539.png)

有人问我书房（工地）设计注意啥，我只能说桌子竟可能的宽，深度太大没有意义，是否升降桌意义不大，工作个 2 小时还不起来活动下么，人体工程椅子千元左右吧主要是结实能调节坐得稳，墙面空间要利用上，墙面暖色，少灯光污染，白天自然光用上，放点绿色，宜家很多扩展可以买。

今天我们再次讨论包，但也讨论 VSCode 扩展。我们主要处理用户界面，所以... 让我们去和愉快的阅读！

## 原文

> https://tomicriedel.medium.com/10-flutter-tips-season-2-part-3-10-15c88da72c2c

## 正文

### Animate do

https://pub.dev/packages/animate_do

在我开始写这篇文章的前一天，我看到了这个软件包，并立即更新了我的整个应用程序。现在听起来不是很有动力吗？好吧，让我先解释一下这个包能做什么。我个人很讨厌动画。我想很多人都同意我的观点。然而，现在你不得不使用动画来吸引用户。这就是 animate do 的用武之地。Animate do 提供了大量已经准备好的动画选择，您可以将其应用到任何小部件。这里是所有动画的列表，其中有很多子点。

- **FadeIn Animations**
- FadeIn
- FadeInDown 摄影: FadeInDown
- FadeInDownBig
- FadeInUp
- FadeInUpBig
- FadeInLeft 左后方
- FadeInLeftBig
- FadeInRight
- FadeInRightBig
- **FadeOut Animations 淡出动画**
- FadeOut 淡出
- FadeOutDown 下降
- FadeOutDownBig 淡出
- FadeOutUp 淡出
- FadeOutUpBig 4. FadeOutUpBig
- FadeOutLeft 后退左转
- FadeOutLeftBig 淡出
- FadeOutRight 完全消失
- FadeOutRightBig 超级大
- **BounceIn Animations**
- BounceInDown
- BounceInUp
- BounceInLeft 左边
- BounceInRight
- **ElasticIn Animations ElasticIn 动画**
- ElasticIn
- ElasticInDown
- ElasticInUp
- ElasticInLeft 左边
- ElasticInRight
- **SlideIns Animations SlideIns 动画**
- SlideInDown 滑下
- SlideInUp
- SlideInLeft 滑进左边
- SlideInRight
- **FlipIn Animations Flipboard Animations**
- FlipInX
- FlipInY 飞翔的
- **Zooms 放大**
- ZoomIn
- ZoomOut
- **SpecialIn Animations 专题动画**
- JelloIn 水母

好吧，这是很多，但我只是想向你们展示这个包裹的可能性。正如我所说的，这个软件包几乎是下一个应用程序的必需品。

### MacOS UI

https://pub.dev/packages/macos_ui

如果你曾经使用 Flutter 为 macOS 开发过应用程序，你可能已经注意到了。没有任何只适用于 macOS 的好小工具。这就是为什么有 macos ui。这个软件包可以让你轻松地创建与 Swift 完全一样的应用程序。同样，这里有数量惊人的小部件和一个非常长的列表，但这次我不需要列出它们。

这只是众多可能性中的一个例子:

![Untitled](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2f9bb3563bcef2e73c29f98b541fec0e079fce1b90108c389dda135ab28c83e3.png)

### Fluent UI

https://pub.dev/packages/fluent_ui

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220515045800.png)

即使是 Windows (尤其是 Windows 11) ，在 Flutter 也没有好的 widget。这就是为什么创建 fluent \_ ui 包的原因。同样，有一个巨大的部件选择，使你的应用程序看起来像一个真正的 Windows 应用程序。

它支持亮/暗主题，Windows 字体，头部，字幕，r 标题，字幕等，显示焦点，页面过渡，钻入，导航和所有的“部件”。

### Onboarding overlay

https://pub.dev/packages/onboarding_overlay

你可能已经从很多应用程序中了解到了这一点，比如一个新手机应用程序，这个程序会向你解释。你可以用 onboarding \_ overlay 完全做到这一点。当我给你们看这个视频时，我想我不需要解释太多:

![https://github.com/talamaska/onboarding_overlay/blob/master/screenshots/demo.gif?raw=true](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/25734fea65f28f2447b52efcac120d85607b56306be1d13dcce66d80fe6add13.com/proxy/0*oMm0nOfhGn0QsQqD)

这个软件包的文档非常棒，而且非常不言自明。

### Flutter Material Pickers

https://pub.dev/packages/flutter_material_pickers

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220515050113.png)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220515050121.png)

正如这个包的名称所示，您可以使用它来调用 Material Design 中的各种选择器。其中一些是新的，因此不包括在 Flutter，而其他一些已经得到改善。

![https://raw.githubusercontent.com/codegrue/flutter_material_pickers/master/images/main_convenience_pickers.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/931a90aa9d433ca6e613a27508bbc50969e29d14846e770ef30fec4a5f5c551f.png)

### Flutter Text Field Fab

https://pub.dev/packages/flutter_otp_text_field

每个人都知道普通的浮动 actionbutton，但是如果你想显示一个很酷的文本字段呢。那么这个包裹正好适合你。这个包的外观非常好，并且很容易在代码中实现。

![https://raw.githubusercontent.com/haefele-software/flutter_text_field_fab/main/assets/flutter_text_field_fab.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a06a6b6c4ce6dd4a77deca700ae2613c3686d408717d63b5a24baaff598a9145.gif)

### Flutter Login

https://pub.dev/packages/flutter_login

这个包装真是太棒了。它为你提供了一个完美的动画插件(或者更确切地说是屏幕)来登录。下面是一个例子:

![https://github.com/NearHuscarl/flutter_login/raw/master/demo/demo.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a5f02253bf94e68d80b4c589390f210bb3a9248f31c76c678e1857a415d5ac58.gif)

最棒的是你可以定制这里的所有东西(我是说所有东西)。

哦，而且实现也非常简单:

### Flutter widget snippets [vscode 扩展]

https://marketplace.visualstudio.com/items?itemName=alexisvt.flutter-snippets

现在，我们来到一个延伸。这个扩展为不同的小部件提供了许多代码片段。我只能建议安装它。

### Flutter Intl [vscode 扩展]

https://marketplace.visualstudio.com/items?itemName=localizely.flutter-int

多语言支持应该是众所周知的。但是在应用程序中不断地实现这个功能真的很烦人。这就是 VSCode 扩展 Flutter Intl 存在的原因。它创建一个绑定。文件和 Flutter 应用程序。

所以: 我可以把这个扩展推荐给每一个想在他的应用程序中轻松实现多语言的人。

https://plugins.jetbrains.com/plugin/13666-flutter-intl

哦，如果您使用 Jetbrains IDE (intellijidea 或 AndroidStudio) ，您可以在这里找到扩展。

### Flutter Riverpod Snippets [vscode 扩展]

https://marketplace.visualstudio.com/items?itemName=robert-brunhage.flutter-riverpod-snippet

如果你使用 Riverpod，那么你一定要看看这个扩展。它是开发的 GDE 罗伯特布伦哈格，使开发与 Riverpod 非常容易。

你可以在这里轻松下载。

再见，祝你有愉快的一天！

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
