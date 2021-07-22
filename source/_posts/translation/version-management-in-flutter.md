---
title: flutter fvm 版本控制
tags: flutter
categories: 译文
date: 2021-07-22 00:00:00
---

![](2021-07-22-08-39-22.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/version-management-in-flutter-c232b04f1919

## 参考

- https://github.com/leoafarias/fvm
- https://fvm.app/

## 正文

![](2021-07-22-08-19-43.png)

Flutter 是一个可移植的 UI 工具包。换句话说，它是一个全面的应用软件开发工具包(SDK) ，包括小部件和工具。Flutter 是一个免费的开源工具，用于开发移动、桌面和 web 应用程序。Flutter 是一种跨平台的开发工具。这意味着用同样的代码，我们可以同时创建 ios 和 android 应用程序。这是在整个过程中节省时间和资源的最佳方式。在这方面，hot reload 正在获得移动开发者的支持。允许我们通过热重装快速查看在代码中实现的更改。

Flutter 管理版本允许不同类型的 Flutter 版本可在项目的基础上。这意味着我们可以为不同类型的项目定义特定类型的 Flutter 版本，它允许我们释放多个通道，在本地缓存它，因此切换版本。那我们就不用等安装好了。

在本文中，我们将学习 Flutter 版本管理。在这里，我们将看到如何建立工作版本管理抖动。我们开始吧。我们开始吧。

### 版本管理(FVM)

在进行 Flutter 项目时，需要发布更新的 Flutter 和应用程序，并进行验证，切换不同类型的软件开发工具包进行测试，这需要时间。为了避免这一点，我们使用 Flutter 版本管理，它为我们提供了不同类型的 Flutter 版本我们的机器。因此，每次 Flutter 可以测试应用程序对更新 Flutter 版本没有等待安装，将能够切换到 Flutter 版本相应。

### 安装

首先需要确定 Flutter 是否已经安装，以及 Flutter 是否是稳定通道。如果没有，则在命令行中键入以下代码。

```dart
// set flutter to stable channel
flutter channel stable// check flutter channel
flutter channel// output
Flutter channels:
  master
  dev
  beta
* stable
```

在这之后，我们必须确定我们的 Flutter 是否已经安装或没有，如果没有，那么首先我们将安装 FVM

```sh
$ pub global activate fvm
```

step 现在我们将看到在安装过程结束时给出了一些警告，因此我们需要将 fvm 路径添加到 shell 配置文件(。在进行下一个步骤之前，请使用 bashrc、 bash_profile 等

```sh
export PATH=”$PATH:`pwd`/flutter/bin”$ fvm install
export PATH=”$PATH:`pwd`/bin/cache/dart-sdk/bin”
export PATH=”$PATH:`pwd`/.pub-cache/bin”
```

![](2021-07-22-08-27-39.png)

### SDK 版本说明

DVM 允许我们安装多种类型的 Flutter 释放或通道安装通道使用稳定和安装 Flutter 释放版本我们将使用 v2.0.5 或 1.17.0-dev. 3.1 和一旦我们运行-跳过-安装，它将跳过安装

```sh
$ fvm install stable or fvm install 2.0.5
```

![](2021-07-22-08-28-19.png)

### Project Config SDK Version

在此之后，我们将看到，无论项目是否配置为使用特定的版本，如果没有，我们将在没有参数的适当版本上安装它。

```sh
$ fvm install
```

### 已安装的 Flutter 版本列表

现在，通过输入以下命令，我们可以通过使用下面的命令 FVM 将存储 SDK 版本来列出我们机器上已安装的版本。

```sh
$ fvm list
```

### 升级 SDK 版本

使用升级 SDK 版本命令时，我们需要升级我们目前的 SDK 版本，所以你必须调用您的 Flutter SDK 命令作为正常的 Flutter 安装。

```sh
$ fvm flutter upgrade
```

### 设置 IDE

现在我们来看看如何配置 IDE，下面我们展示了如何在 android studio 和 VS Code 中进行配置，现在让我们来看看。

- Android Studio

在根项目目录中复制下面的绝对符号链接。

```sh
Example: /absolute/path-to-your-project/.fvm/flutter_sdk
```

然后我们将在 Android Studio 的菜单中打开 Languages and Frameworks-> Now search for flutter or flutter and change the path to flutter SDK。然后实施改变。现在您可以使用选定的 Flutter 版本运行它并调试它。如果你想看到新的设置，然后我们可以使用 Android 工作室将重新启动。

![](2021-07-22-08-30-46.png)

- VS Code

现在我们将在这里配置 VS Code，我们将看到如何完成 VS Code 过程。

目录的路径，我们可以在代码中看到 FVM 安装的所有版本

```sh
"dart.flutterSdkPaths": ["$YOUR_PATH/fvm/versions",],
```

为了获得上面的路径，我们将执行 fvm list 命令

```sh
// copy this path
Versions path:  $YOUR_PATH/fvm/versions
```

输入 cmd + shift + p 来使用 sdk，然后输入 change sdk，现在你可以选择你喜欢的版本了。

![](2021-07-22-08-32-16.png)

### 总结

在这篇文章中，我对版本管理做了一个简单的解释，你可以根据自己的需要对其进行修改和实验，这个简单的介绍来自于版本管理的 Flutter。

我希望这个博客将提供您尝试在 Flutter 版本管理充分的信息。我们向您展示了 Flutter 探索版本管理和工作在您的 Flutter 应用程序，所以请尝试它。

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
