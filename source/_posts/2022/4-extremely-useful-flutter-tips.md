---
title: 4 个非常有用的 Flutter 技巧
tags: flutter
categories: 译文
date: 2022-05-11 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220511061146.png)

今天我将向您展示 4 个非常有用的 Flutter 技巧，您可以立即应用到您的项目。我不会向您展示任何包或扩展，就像我通常做的那样，但是非常简单，但是非常有用的提示！

## 原文

> https://tomicriedel.medium.com/4-extremely-useful-flutter-tips-57686f1e3707

## 正文

今天我将向您展示 4 个非常有用的 Flutter 技巧，您可以立即应用到您的项目。我不会向您展示任何包或扩展，就像我通常做的那样，但是非常简单，但是非常有用的提示！

### 简化 Assert 管理

管理 Assert 可能非常困难。如果你想在你的应用程序中多次使用一个图像，你必须一次又一次地指定路径。但是有一个简单得多的解决方案。创建一个 App Assets 类，用于存储所有的 App Assert。现在您可以轻松地使用 `AppAssets.appLogo` 或 `AppAssets.noConnection` 调用 Assert。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/06f8f17f37d444f1637cba69d89514eb854cacb63f238734950c7d17cdc97457.png)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/4a31cf09ccec7f67753e1eb970996cca64a82bcaae00544c81f443550f601433.png)

### 更容易 imports

在一个文件的开头看到和管理成千上万的导入真的很烦人。这就是为什么我要向你们展示一种轻松减少进口的方法。

假设你有一个文件夹叫做 Constants, 里面的文件包括 `app_colors.dart`, `app_fonts.dart`, `app_theme.dart`, `app_constants` and `app_assets.dart` are.

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/15770749db4b1bb2fff56d4b17bdb681831ab693634c2b9619b910e4a9e15cd0.png)

在这个文件夹中，您现在创建一个名为 constant.dart 的新文件。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/51864fc203e5f3ea47220452f91a8ce67107075b072c2f9a4d5e5e09b039474d.png)

在这里，您为每个文件编写一个导出语句。现在你可以通过简单的导入 constant.dart 来访问你的每个文件:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/8dd70299148c3f7f4bb39dd87688903cd2ed9a09fc035ab4ef659fece1068a92.png)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bce876ed3de8b6841edeefacf7177cf763e13eb6f46fa9cf4fc008f908164360.png)

### 从按钮上移除飞溅效果

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/5da1983a1d3e8c693ac7ddef7f7d6b20d0bf0964861ccf9d6b2bb055f0d72cb0.gif)

当你点击一个按钮时，每个人都知道这种飞溅效果，我一点也不喜欢。

所以我将向你们展示如何用一条线消除这种效果。

为此你必须使用 `splashFactory`:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/248ecbc9e81424709aa6412e952f76208b4145f30ff9833dd9061aa22593ee2f.png)

现在你的按钮在按下的时候看起来像这样:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/257d73d09393efeb549ccb771f3aa4d735971246739e5382fc64901088965e69.gif)

(我一直在点击按钮)

### 更简单的平台小工具

每个 Flutter 开发人员可能都知道当你查询用户是 iOS 还是 Android 时的情况。因此，您然后显示一个特定的 wdiget，例如 Switch 或 CupertinoSwitch。但是如果我告诉你，你不需要一个查询，也不需要两个小工具呢？怎么做到的？这就是我现在要展示给你们的:

许多可用于安卓和 iOS 的小工具都有一个。安卓版本的自适应扩展。例如，让我们用。适应的:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a6464c595fd48b5d702da42dfffd57684cea98f16c189dfdbe828d939f048d9b.png)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a43e35ded87dc43f3dd7042402371ce73d8b8346d0b54a5ea88735c8ba8a8e0b.png)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/da6718f224f4910a55400a0cb106ee2ed32301937d96bfd05519848cc80eda12.png)

好的，这已经很好了，但是最好的还在后面: 这也可以用于图标。要做到这一点，你只需要使用 `Icons.adaptive.share` 在 Android 和 iOS 上显示一个共享图标。

我不知道这些小工具具体适用于哪些部件，但无论如何，`Slider`、 `SwitchListTile` 和 `CircularProgressIndicator` 都可以使用这个特性。

### 可见性小工具

使用 bool 来查询一个小部件是否应该可见通常是这样的:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e45f73d516585564688a4dbb0d723f2599d4c590ee2ec341b60412bcc6e919e7.png)

但是还有一个名为可见性的小工具可以做到这一点:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e5b8dbbd3edab4a4d14c0c8bcd42bc7a49f9f9197643eda7c1bad748928b8fd1.png)

这样看起来好多了，对吧？

end

谢谢你的阅读，祝你有愉快的一天！

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
