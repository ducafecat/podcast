---
title: 在你的 Flutter 项目中隐藏敏感信息
tags: flutter
categories: 译文
date: 2021-07-02 00:00:00
---

![](2021-07-02-06-19-17.png)

## 猫哥说

有的时候我们需要在项目中隐藏敏感信息，比如你的阿里 OSS 账号 AccessKey ，写入代码中上传 git 仓库，是一件很危险的事情，所以我们需要用环境变量的方案来隐藏，记得你的 .env 文件要加入 .gitignore 文件中进行过滤呀。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutter-community/learn-the-best-way-to-hide-sensitive-keys-in-your-flutter-app-ac7638435401

## 代码

https://github.com/Wizpna/flutter_dotenv_tutorial.git

## 参考

- https://pub.dev/packages/flutter_dotenv

## 正文

![](2021-07-02-06-02-01.png)

我很高兴能写这个话题，因为这是一个移动应用程序开发者必须很少或已知的知识领域。

作为一个应用程序开发者，在谷歌游戏商店或苹果商店上开发和部署应用程序并不意味着你已经耗尽了移动应用程序开发周期。

移动应用程序开发周期还包括提高应用程序安全性。

这就是为什么我分享这篇文章，以便您将学习如何隐藏敏感的安全密钥在您的 Flutter 应用程序。

在本文的最后，您将学习如何使用一个名为 `Flutter_dotenv` 的 Flutter 插件来隐藏您的 Flutter 应用程序中的敏感键。

https://pub.dev/packages/flutter_dotenv

### 那么让我们开始吧

使用 Visual Studio、 IntelliJ 或 Android Studio 创建您的 flutter 应用程序，然后打开“ pubspec.yaml”文件，并安装以下包。

```yaml
dependencies:
  flutter_dotenv: ^5.0.0
```

在您的 flutter 项目的根目录下创建一个.env 文件

![](2021-07-02-06-05-06.png)

将新创建的. env 文件添加到 pubspec.yaml 文件中的资产包中。

```yaml
assets:
  - .env
```

请注意: 添加新创建的。在 pubspec.yaml 文件中，请运行 flutter Pub get in the terminal，或者单击 Pub get in IntelliJ 或 Android Studio 将该文件添加到当前的工作目录文件夹中。

在成功添加了。在 pubspec.yaml 文件中添加您的敏感键。你创建的 env 文件。(例如，见下图)

![](2021-07-02-06-05-56.png)

下一步是在 main.dart 文件中初始化/加载. env 文件内容，如下图所示:

![](2021-07-02-06-06-12.png)

下一步将访问。环形文件。你可以访问。使用下面的代码。

```dart
dotenv.env['VAR_NAME'];
```

请参阅下面的图片以获得正确的理解

![](2021-07-02-06-06-59.png)

使用物理设备或模拟器测试运行项目

![](2021-07-02-06-07-18.png)

> 请注意: 为了这个教程的缘故，我必须显示我添加在我的灵敏度键。因为我希望你们都能看到它，了解如何将敏感的密钥存储在 env 文件中，并在 flutter 应用程序中的任何地方访问它。

这种将敏感密钥存储在 env 文件中的模式有助于在黑客对应用进行反编译时，安全引导敏感密钥不被暴露。

永远记住添加。文件作为一个条目在您的 `.gitignore` 文件。(一) `.gitignore` 文件是一个纯文本文件，其中每一行包含 git working copy 中不包含的文件/目录。)

![](2021-07-02-06-08-14.png)

如果你读到这里，恭喜你！

这是你刚刚参与的项目的源代码。

https://github.com/Wizpna/flutter_dotenv_tutorial.git

如果你发现这篇文章有帮助和教育，请击击击掌按钮尽可能多的次数，以显示您的支持

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
