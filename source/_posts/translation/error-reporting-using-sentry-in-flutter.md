---
title: 在 Flutter 使用 Sentry 收集错误
tags: flutter
categories: 译文
date: 2021-07-05 05:36:05
---

## 猫哥说

这个 Sentry 是一个错误收集平台方案，个人项目是免费的。

现在针对 Flutter 已经很成熟，可以同时收集 Dart、Flutter、原生端的错误。

猫哥在企业中是自己搭建了 Sentry 服务，这个是可以私有化的。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/podiihq/error-reporting-using-sentry-in-flutter-c4e7c8030b88

## 代码

通过这种方式，您将能够监视和获得错误通知，并在客户开始抱怨之前提前解决它们。因为工作代码 = = 快乐的客户。

## 参考

- https://sentry.io
- https://pub.flutter-io.cn/packages/sentry_flutter

## 正文

想象一下你是一个独立的开发者，你在度假之前开发了一个新功能，在周末前几天将它部署到生产环境中，当客户开始积极使用它时，用户的问题和抱怨开始出现，你已经开启了你的度假情绪。正如通常的口号所说，“顾客永远是对的”，你决定优先考虑顾客的满意度而不是假期，并恢复工作情绪。可能会令人沮丧，对吧？

以下是如何向服务报告错误，从而避免在客户之前出现未知的潜在错误或问题，因为工作代码等同于满意的客户。

您可以向许多服务报告代码错误。但是，在本文中，您将了解如何监视应用程序和潜在错误或 bug，并使用 Sentry 报告它们。

### 什么是 Sentry？

Sentry 是一个应用程序监视平台，它使开发人员能够监视、诊断、修复和优化其代码的性能。

### 让我们开始吧

#### 使用 Sentry 创建一个帐户

如果你在 Sentry 上还没有帐号，在这里创建一个:

https://sentry.io/signup

#### 创建一个新的 Sentry Flutter 应用

接下来，登录到刚刚创建的 Sentry 帐户，创建 Flutter 应用程序。

按照下面的步骤成功地创建新项目。

- 创建新项目

登录之后，选择 create project 图标来创建一个新项目。

![](2021-07-05-05-42-13.png)

- 选择开发平台

有各种各样的开发平台支持 Sentry，包括 Python，Express，Spring Boot，Android 等等，但是本文只关注 Flutter。因此，从列表中选择 Flutter。

![](2021-07-05-05-42-34.png)

- 设置默认警报设置

接下来，将默认警报设置设置为在发生任何错误时何时以及如何获取警报的频率。在本文中，我将选择获取任何新问题的警报选项，但您始终可以选择任何您想要的选项。

![](2021-07-05-05-42-54.png)

- 最后，给你的项目起个名字

在本文中，我将给它命名为扑哨测试，然后，创建项目。

![](2021-07-05-05-43-16.png)

- 从 Sentry 获取 DSN

为了向 Sentry 报告错误，您需要一个 DSN (数据源名称) ，它将用 Sentry 服务唯一地标识您的应用程序。因此，在 Sentry 上创建项目之后，我们将从上面步骤中创建的应用程序中复制 DSN。

要获得 DSN，在您刚刚从上面创建的项目中，导航到项目设置并向下滚动到客户机密钥(DSN) ，如下所示:

![](2021-07-05-05-43-44.png)

接下来，在选择客户端密钥之后，客户端密钥选项卡将显示出来，从那里您将复制 DSN，如下所示:

![](2021-07-05-05-43-57.png)

#### 创建一个 Flutter 应用程序，用于向 Sentry 报告错误

这一步假设，您已经有了一些关于如何创建一个新的 Flutter 项目的实践经验。如果你是新的 Flutter 检验官方 Flutter 文件。你也可以看看我的文章《如何用 Flutter 开始》

现在让我们创建一个示例 Flutter 应用程序，用于向 Sentry 服务报告错误。

在终端上，输入 $flutter create Command，后跟应用程序的名称。在这种情况下，我们将使用名称 flutter _ sentry _ test。

```
$ flutter create flutter_sentry_test
```

注意: 您也可以根据自己的喜好在各自的 IDE 上创建应用程序。

#### 导入 Flutter Sentry 包

在应用程序中安装 Sentry，将其添加到 pubspec.yaml 文件中

![](2021-07-05-05-45-05.png)

#### 配置和初始化 Sentry SDK

在 main.dart 文件中，导入 sentry 包。

![](2021-07-05-05-45-28.png)

接下来，添加将捕获应用程序中未处理的异常的配置。在此步骤中，用您在步骤 2 中创建的应用程序中的 Sentry DSN 替换 DSN url。从上面的 Sentry 那里得到一个 DSN。

![](2021-07-05-05-45-44.png)

您还可以通过 Dart 环境变量配置 SENTRY \_ dsn，方法是将 -- Dart-define 标志传递给编译器，如下例所示:

```
--dart-define SENTRY_DSN = 'https://your-sentry.io DSN'
```

#### 验证

最后，在这个步骤中，为了测试目的，通过在代码中添加一个有意识的错误来验证是否发送了错误。

![](2021-07-05-05-46-23.png)

这将抛出一个 State Error，它将被发送到 Sentry.io 服务

你可以通过导航到你的 Sentry 应用程序来确认这一点

在我的例子中，这里是发送到我的 Flutter Sentry 应用程序的 State Error

![](2021-07-05-05-46-54.png)

下面是示例应用程序如何向 Sentry 发送错误的演示:

![](sentry.gif)

点击这里查看完整的代码片段:

https://github.com/JosephineAkello/flutter_sentry_test

通过这种方式，您将能够监视和获得错误通知，并在客户开始抱怨之前提前解决它们。因为工作代码 = = 快乐的客户。

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
