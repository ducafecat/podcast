---
title: Flutter 多个版本切换控制
tags: flutter
categories: 译文
date: 2021-05-07 07:43:37
---

![](2021-05-07-08-03-12.png)

## 原文

https://medium.com/litslink/flutter-how-to-fluttering-from-one-version-to-other-versions-cf242ffb15f7

## 猫哥说

如果你和猫哥我一样，手上有几个 1 年前的项目，那么经常切换 sdk 版本，就成了必须做的事情，我可不会把老项目去升级新版本。

## 前言

每次当技术改变为一个新的主版本时，将一个项目从较低的版本迁移到较高的版本是痛苦的。幸运的是，Dart 有一个迁移工具，可以帮助您将项目中的定义迁移到新的语法中。

![](2021-05-07-07-46-52.png)

但是即使你已经准备好切换到一个更新的版本，你也必须等待在你的项目中使用的一系列插件，即使在我的情况下，我确实帮助一些开源库迁移到 Null-Safety，这不足以将我的项目迁移到下一个版本。

社区每天都在成长，在大多数情况下，你会找到一些替代插件，这些插件还没有被迁移。如果您停留在以前的 Flutter 版本，本文将帮助您简化在项目之间的切换。

## 参考

- https://flutter.dev/docs/development/tools/sdk/releases
- https://fvm.app/

## 正文

### 版本管理

Flutter 版本有一个相关的 Dart SDK 版本，可以在本地缓存到 Flutter 缓存文件夹中，因此您应该记住一些约束。

作为一个开发者，你有几个选项可以在不同版本之间切换:

- 使用命令行脚本(CLI)进行手动编写
- Flutter 版本管理器 CLI
- Sidekick GUI 图形用户界面

### 手动

当你有两个或三个项目并在它们之间切换时，这个选项对你来说并不常见。

Flutter CLI 具有 git 控制的版本管理，因此即使使用 `git checkout <tag>` 命令也很容易切换。要查看可以签出的版本列表，只需运行 `git tag-l`，然后在找到所需版本时按 `q` 退出。

> 就是用 git 命令啦，没啥特别的，但是国内 github 很慢。。。

### 下载所需版本

你可以访问 Flutter 发行版页面下载所需版本的快照，并替换环境变量版本以使用下载的版本

https://flutter.dev/docs/development/tools/sdk/releases

> 这是我之前常用的方法，免去了下载，每个版本包可是要 1G 了

### Flutter CLI

您可以尝试使用 `flutter dider <version>` 命令来降低它的等级，但是您将面临一个无法从 `2.x` 切换到 `1.x` 的问题。

### Git

幸运的是，Flutter SDK 使用 GIT 来管理版本，因此您可以有一个单一的目录来切换，而不像下载每个版本，并减少硬件上的空间。

假设你的 SDK 副本位于 ~/flutter 然后:

```sh
cd ~/flutter

# Checkout needed version
git checkout 1.22.6

# Download Dart SDK, tools, etc.
flutter doctor

# Check Flutter and Dart version
flutter --version
```

最后，您将看到 `flutter --version` 命令的输出:

```sh
Flutter 1.22.6 • channel unknown • unknown source
Framework • revision 9b2d32b605 (3 months ago) • 2021-01-22 14:36:39 -0800
Engine • revision 2f0af37152
Tools • Dart 2.10.5
```

当你需要切换回最新版本时，反之亦然:

```sh
cd ~/flutter

# Switch back to the stable channel
flutter channel stable

# Switch to latest version
# flutter doctor will be invoked at this step
flutter upgrade

# Check Dart and Flutter  version
flutter --version
```

如果您对此步骤没有意见，可以通过调用 `*.sh` 来存储这些脚本并自动执行降级/升级过程。命令行中的 sh 文件。

### FVM (Flutter Version Manager)

> 这也是猫哥我现在用的方式

以前，当我基于 React 和 ReactNative 开发应用程序时，我使用 NPM (节点包管理器)来管理项目中的依赖关系，Flutter 中的一个类似工具是 pub。有时候我需要更改 NPM 版本，但是它对 Node 版本有限制，所以我需要做同样的步骤，下载几个版本，替换目录等等。为了避免这个例程，我使用了 NVM (节点版本管理器) ，我的日子就要好起来了。

幸运的是，Flutter 有一个名为 FVM 的非官方工具，可以做同样的事情，它管理 Flutter 版本并将它们存储在您的硬件上。

https://fvm.app/

FVM 有两种使用方式:

- 全局指定 flutter 版本
- 指定你当前项目使用的版本

只需按照安装 https://fvm.app/docs/getting_started/installation 和配置说明的 https://fvm.app/docs/getting_started/configuration 来正确设置你的环境。

![](2021-05-07-07-59-28.png)

另一个令人惊奇的选择是 FVM 的 GUI，称为 Sidekick，它使得全局或本地(项目) Flutter SDK 版本管理更加舒适，如果你不是 CLI 的大粉丝。

## 老铁记得 点赞、转发 ，我将更有动力呈现 Flutter 好文~~~~

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
