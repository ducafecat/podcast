---
title: Google I/O 2021 发布 Flutter 2.2
tags: flutter
categories: 译文
date: 2021-05-19 08:42:13
---

![](2021-05-19-08-43-51.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://medium.com/flutter/announcing-flutter-2-2-at-google-i-o-2021-92f0fcbd7ef9

## 参考

- https://flutter.dev/docs/whats-new
- https://www.slashdata.co/reports/?category=mobile-desktop

## 正文

在今天的 Google i/o 大会上，我们发布了 Flutter 2.2，这是我们最新发布的开源工具包，可以为单一平台上的任何设备构建漂亮的应用程序。Flutter 2.2 是 Flutter 目前为止最好的版本，它的更新让开发者比以往任何时候都更容易通过应用程序内购买、支付和广告赚钱; 连接到云服务和 api，扩展应用程序以支持新功能; 以及工具和语言功能，允许开发者消除一整类错误，提高应用程序性能和减少包大小。

### 建立在 Flutter 2 的基础上

Flutter 2.2 是建立在 Flutter 2 的基础上的，它将 Flutter 从它的移动根源扩展到整合了 web、桌面和嵌入式使用。它是专门为环境计算世界设计的，用户有各种各样不同的设备和形式因素，并寻找跨越他们的日常生活的一致体验。使用 Flutter 2.2，企业、初创公司和企业家都可以构建高质量的解决方案，这些解决方案可以充分发挥其潜在市场的潜力，让创造性灵感(而不是目标平台)成为唯一的限制因素。

Flutter 是目前最流行的跨平台开发框架。

最近的一项移动开发者研究强调了 Flutter 的增长。分析公司 SlashData 的移动开发人员 2021 年人口预测显示，Flutter 现在是最流行的跨平台开发框架，有 45% 的开发人员选择了它，在 2020 年第一季度到 2021 年第一季度之间增长了 47% 。我们自己的数据证实了 Flutter 的这种转变; 在过去的 30 天里，Play Store 中超过八分之一的新应用程序都是使用 Flutter 构建的。

在 i/o，我们分享到仅仅在 Play Store 中就有超过 20 万个应用程序使用了 Flutter。这些应用程序来自腾讯，其微信即时通讯应用程序在 iOS 和安卓系统上被超过 12 亿用户使用; ByteDance，TikTok 的发起者，现在已经使用 Flutter 开发了 70 个不同的应用程序; 以及其他来自宝马、 SHEIN、 Grab 和滴滴等公司的应用程序。当然，Flutter 不仅仅被大公司使用。一些最具创新性的应用程序来自于你可能从未听说过的名字: Wombo，沃姆博，自拍应用程序; Fastly,间歇性禁食应用程序; Kite，一个漂亮的投资交易应用程序。

![](2021-05-19-08-50-47.png)

![](2021-05-19-08-51-18.png)

![](2021-05-19-08-51-43.png)

### 介绍 Flutter 2.2

Flutter 2.2 发布的重点是改进开发体验，以帮助您向客户提供更可靠、性能更好的应用程序。

Sound null safety 现在是默认的新项目。空安全增加了对空引用异常的保护，使开发人员能够在代码中表达不可空类型。由于 Dart 的实现是合理的，编译器可以在运行时消除空检查，从而提高应用程序的性能。这个生态系统反应迅速，大约有 5000 个软件包已经升级到支持空安全。

这个版本还有很多性能改进: 对于 web 应用，我们使用服务工作者提供后台缓存; 对于 Android 应用，Flutter 支持延迟组件; 对于 iOS，我们一直在开发工具来预编译着色器，以消除或减少首次运行的 jank。我们还为 DevTools 套件添加了一些新特性，帮助您理解如何在应用程序中分配内存，以及对第三方工具扩展的支持。

此外，我们一直致力于一些重要的改进领域，如改善网页目标的可访问性。

我们的工作超越了 Flutter 的核心。我们还与其他谷歌团队合作，帮助将 Flutter 整合到我们更广泛的开发者堆栈中。特别是，我们继续建立可信赖的服务，帮助开发者负责任地将他们的应用货币化。我们的新广告 SDK 是更新在这个版本的零安全和自适应横幅格式的支持。我们还引入了一个新的支付插件，与谷歌支付团队建立了合作关系，让你可以在 iOS 和安卓系统上支付实体商品。我们更新了我们的应用内购买插件，以及一个匹配的代码。

- ads
  https://developers.google.com/admob/flutter/quick-start

- pay
  http://pub.dev/packages/pay

- in_app_purchase
  https://pub.dev/packages/in_app_purchase

- codelabs flutter-in-app-purchases
  https://codelabs.developers.google.com/codelabs/flutter-in-app-purchases

作为“秘密武器”的 Flutter，Dart 也得到一个更新，在这个版本。Dart 2.13 扩展了对本地互操作性的支持，在 FFI 中支持数组和打包结构。它还包括对类型别名的支持，这提高了可读性，并为某些重构场景提供了一种温和的方式。我们继续为更广泛的生态系统添加集成，使用 Dart GitHub Action 和针对基于云的业务逻辑部署进行优化的策划的 Docker Official Image。

- github action
  https://github.com/marketplace/actions/setup-dart-sdk

- ocker Official Image
  https://hub.docker.com/_/dart

### 不仅仅是一个谷歌项目

虽然谷歌仍然是 Flutter 项目的主要贡献者，但我们很高兴看到 Flutter 周围更广泛的生态系统的发展。

![](2021-05-19-08-56-12.png)

最近几个月来，Flutter 增长的一个特别领域是不断扩展到越来越多的平台和操作系统。在 Flutter 参与，我们宣布，丰田是带来扑动到他们的下一代汽车信息娱乐系统。上个月，Canonical 发布了他们的第一个 Ubuntu 版本，带有 Flutter 的集成支持，Snap 的集成和韦兰的支持。

https://medium.com/googleplaydev/seamless-multi-platform-app-development-with-flutter-ea0e8003b0f9#f53d

https://ubuntu.com/blog/ubuntu-21-04-is-here

两个新的合作伙伴展示了这个不断增长的生态系统。三星正在将 Flutter 移植到 Tizen，并提供了一个其他人也可以贡献的开放源代码库。索尼正在努力为嵌入式 Linux 提供解决方案。

https://github.com/flutter-tizen/flutter-tizen

https://github.com/sony/flutter-embedded-linux

设计师也从这个项目的开源性质中受益，Adobe 公布了更新后的 XD to Flutter 插件。Adobe XD 为设计师提供了一个很好的实验和迭代的方式，现在有了增强的 Flutter 支持，设计师和开发人员可以在同一资产上协作，比以往更快地将伟大的想法投入到生产中。

https://medium.com/adobetech/announcing-xd-to-flutter-v2-0-82d09f3909a7

最后，微软将继续与我们合作; 除了 Surface 团队一直在使用 Flutter 构建可折叠体验的工作外，本周还将看到为 Windows 10 构建的 UWP 应用程序 Flutter 的 alpha 支持。我们很高兴看到更多的应用程序能够利用 Flutter 内置的平台适应性，在移动、桌面、网络和其他领域提供更好的体验。

https://flutter.dev/desktop#windows-uwp

### 建立伟大的 Experiences

最重要的是，我们建立 Flutter 来帮助开发者建立伟大的体验。我们被应用程序开发可以做得更好这个想法激励着: 我们可以通过消除传统的阻碍接触到你的受众的障碍来增强你的能力。

我们喜欢看你如何让 Flutter 工作。一个例子是美国退伍军人管理局的一个项目。下面的视频展示了他们的 Flutter 应用程序是如何帮助他们提供创伤后应激障碍士兵康复。

通过在 Google i/o 上举办各种各样的关于 Flutter 的研讨会、演示会和点播会议，我们很高兴与大家分享我们的工作。别忘了看看我们有趣的照相亭网络应用，它是用 Flutter 开发的，它可以让你和我们的 Dash 吉祥物和她的朋友们创建一张自拍！

https://events.google.com/io/program/content?4=topic_flutter

https://photobooth.flutter.dev/

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

![](/img/public-qrcode.png)

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
