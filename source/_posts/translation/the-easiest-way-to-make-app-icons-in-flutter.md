---
title: flutter 最简单的应用程序图标制作方法
tags: flutter
categories: 译文
date: 2021-09-17 00:00:00
---

![](2021-09-17-09-01-55.png)

## 原文

> https://medium.com/@bharadwaj.palakurthy/the-easiest-way-to-make-app-icons-in-flutter-9fe1bc9dd646

## 参考

- https://pub.flutter-io.cn/packages/flutter_launcher_icons

## 正文

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e50c1729b6a2a06718db1590902fa4d9a84bf207fe77d8cba5daf2766fd4ad3a.png)

让我们承认这一点ーー管理应用程序图标是一项重复的任务。他们必须生成的多分辨率和手动放置在几个文件夹，这是一个世俗的任务采取。你可能需要做一些小的改变或者修改，现在你必须重复整个替换图标的过程。

不仅如此，根据我们选择的平台或操作系统的版本，还应用了不同的规则。所以把这些都记在心里，这个过程最好是自动化，而不是手动完成。我们将在这里使用这个名为“ flutter_launcher_icons”的 flutter 包来自动生成所有需要的分辨率。

### Flutter Launcher Icons:

一个命令行工具，简化了更新应用程序启动图标的任务。完全灵活，允许你选择你想要更新启动器图标的平台，如果你想要的话，选择保留你的旧启动器图标，以防你想在未来的某个时候返回。

### 先决条件

在任何情况下，当从图形编辑器导出时，应该是:

- Format: 32-bit 格式: 32 位**PNG 巴布亚新几内亚**
- Icon size 图标大小**must be up to 1024x1024 pixels 必须达到 1024x1024 像素**
- 确保在 40 像素处可见\(这是最小的图标\)**\(Apple Requirement\) \(苹果需求\)**
- 最大尺寸**1024KB \(Android Requirement\) 1024KB \(Android 版本要求\)**
- 图标必须用**no transparency 没有透明度**
- 形状必须是正方形**no rounded corners 没有圆角**
- 需要一个自适应的 android 图标**background 背景**and 及**foreground 前景**to be separated 分开

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e3f60e43c5b3946488dc86982a76dbab056195e66d946fb867c1fe77f67cdda6.png)

安卓产品图标关键字

The intended look might be different from the guidelines provided by the platforms. So we’ll be creating 3 different flavors for android, iOS, adaptive icons.

预期的外观可能与平台提供的指导方针不同。因此，我们将为 android、 iOS 和自适应图标创建三种不同的风格。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2af230975ce3c7d2008bf1962f3c24b0a1d57db3d8b0752bf1cdb32eb97ed7b1.png)

预期外观

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a03aa910cb3c1a3353928b9684a8137f244380db9ee87340a26f4e8e3ef3eae9.png)

Android and iOS \(no transparency\) 安卓和 iOS \(没有透明度\)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/5aa8577609eab9bc5e67af5c0b5a34cd4ecf8515fc80d4d22a782edb8471bb9c.png)

Adaptive Icons for Android 8.0 and above 8.0 及以上版本的自适应图标

### 实施方案:

我们将使用一个名为 flutter_launcher_icons 的包

现在我们需要分别在你的代码中实现它:

- 第一步: 添加依赖项。

> 将 dependency 添加到位于 Flutter 项目根目录中的 pubspec.yaml 文件:

```dart
dev_dependencies:
  flutter_launcher_icons: any

```

- 第二步: 配置属性

```dart
flutter_icons:
  ios: true
  android: true
  image_path_ios: "assets/launcher/icon.png"
  image_path_android: "assets/launcher/icon.png"
  adaptive_icon_background: "assets/launcher/background.png"
  adaptive_icon_foreground: "assets/launcher/foreground.png"
```

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e189e5ab390a63ccc1a010f83fb7e2e484a53a324348238aa74c561c70f203f2.png)

图像在你的 assets/launcher/

- 第三步: 运行包

> 设置完配置后，剩下要做的就是运行包。

```dart
flutter pub get
flutter pub run flutter_launcher_icons:main
```

- 第四步: 跑步

如果一切顺利，资产已经产生。现在，您已经准备好构建应用程序并运行它了。恭喜你

### 属性:

> 目前，它只能用于为 android/ios 分配图标

- **image_path 图像路径**: : 图标图像文件的位置，你想用它作为应用程序启动图标
- **image_path_android 图片/path/android**: : 特定于 Android 平台的图标图像文件的位置
- **image_path_ios 图片/path/ios**: : 特定于 iOS 平台的图标图像文件位置

> 接下来的两个属性只在生成 Android 启动器图标时使用

- **adaptive_icon_background 背景**: You can pass in a solid color \(E.g. “#ffffff”\) or image asset \(E.g. “assets/images/christmas-background.png”\) which will be used to fill out the background of the adaptive icon.
- **adaptive_icon_foreground 自适应图标前景**: The image asset which will be used for the icon foreground of the adaptive icon : 将用于自适应图标的前景图标的图像资产

# Thank you\!

# 谢谢

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
