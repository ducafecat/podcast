---
title: 介绍 Flutter 3
tags: flutter
categories: 译文
date: 2022-05-12 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/91d2f6296411c8c8e6be032cd0c894d94d6b6ddd5f7c573fa45e1bea4c2d2d79.com/max/1400/0*ZQ9Xa7CINFVMA95w)

## 原文

> https://medium.com/flutter/introducing-flutter-3-5eb69151622f

## 我们在手机、桌面和网络上开发多平台用户界面的旅程达到了顶峰

我们很高兴地宣布 Flutter 3 的发布是 Google i/o 主题演讲的一部分。Flutter 3 通过 macOS 和 Linux 桌面应用程序支持，以及 Firebase 集成的改进，新的生产力和性能特性，以及对 Apple Silicon 的支持，完成了我们从以移动为中心到多平台框架的路线图。

## The journey to Flutter 3

我们开始尝试将 Flutter 作为应用程序开发的革命性尝试: 将网络的迭代开发模型与硬件加速的图形渲染和像素级控制结合起来，这些都是以前游戏的专利。在 Flutter 1.0 beta 之后的四年里，我们逐渐在这些基础上进行了构建，增加了新的框架功能和新的小工具，与底层平台进行了更深入的集成，丰富的包库以及许多性能和工具的改进。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c7e1ee691afbea408ae023f3d3ecd1a4f4e68a59e2933151f592ec0eb279838b.com/max/1400/0*pL2z2iYzWPrMu5hw)

随着产品的成熟，越来越多的人开始用它来开发应用程序。今天，已经有超过 50 万个应用程序是用 Flutter 开发的。来自像 [data.ai](https://www.data.ai/en/) 这样的研究公司的分析[broad list of customers](https://flutter.dev/showcase)，以及公开的推荐信 [WeChat](https://play.google.com/store/apps/details?id=com.tencent.mm&hl=en_US&gl=US)，显示 Flutter 在很多领域被广泛的用户使用: 从微信这样的社交应用到金融和银行应用，如 Betterment 和 [Nubank](https://play.google.com/store/apps/details?id=com.nu.production&hl=en_US&gl=US); 从 [Fastic](https://fastic.com/) 和 trip. com 这样的商务应用到 Fastic 和 Tabcorp 这样的生活方式应用; 从 My BMW 这样的伴侣应用到巴西政府这样的公共机构。

> 今天，已经有超过 50 万个应用程序是用 Flutter 开发的。

开发者告诉我们 Flutter 可以更快地为更多平台开发出漂亮的应用程序:

- 91\% 的开发者同意 Flutter 缩短了开发和发布应用程序的时间。
- 85\% 的开发者同意 Flutter 使他们的应用比以前更漂亮。
- 85\% 的用户认为这使得他们可以在更多的平台上发布他们的应用。

在 [recent blog post by Sonos](https://tech-blog.sonos.com/posts/renovating-setup-with-flutter/) 最近的一篇博客文章中讨论了他们的改进设置体验，他们强调了第二点:

> “可以毫不夸张地说，\(Flutter\)解锁了一定程度的“优质”，这与我们团队以前交付的任何东西都不同。对于我们的设计师来说，最重要的是构建新 ui 的容易性，这意味着我们的团队花在规格说“不”上的时间更少，花在迭代上的时间更多。如果这听起来值得的话，我们建议让 Flutter 试一试，我们很高兴我们这样做了。”

## 介绍 Flutter 3

今天，我们将介绍 Flutter 3，它是我们填补 Flutter 支持的平台之旅的高潮。使用 Flutter 3，你可以从一个代码库中为六个平台构建漂亮的体验，给开发人员提供无与伦比的生产力，并使创业公司从一开始就能将新想法带到完全可寻址的市场。

在之前的版本中，我们为 iOS 和 Android 增加了 [web](/flutter/flutter-web-support-hits-the-stable-milestone-d6b84e83b425) 和 [Windows support](/flutter/announcing-flutter-for-windows-6979d0d01fed) 支持，现在 Flutter 3 增加了 macOS 和 Linux 应用的稳定支持。添加平台支持需要的不仅仅是呈现像素: 它包括新的输入和交互模型、编译和构建支持、可访问性和国际化以及特定于平台的集成。我们的目标是为您提供充分利用底层操作系统的灵活性，同时根据您的选择共享尽可能多的 UI 和逻辑。

在 macOS 上，我们已经投资支持英特尔和苹果硅，支持通用二进制应用 [Universal Binary](https://developer.apple.com/documentation/apple-silicon/building-a-universal-macos-binary) ，允许应用程序打包在两种架构上本地运行的可执行文件。在 Linux 上，Canonical 和 Google 已经合作提供了一个高度集成的、最好的开发选项。

关于 Flutter 如何实现漂亮的桌面体验，一个很好的例子就是 [Superlist](https://superlist.com/),，它今天开始 beta 测试。通过一个新的应用程序，Superlist 提供了强大的协作功能，将列表、任务和自由形式的内容组合成一个新的待办事项清单和个人计划。Superlist 团队选择 Flutter 是因为它能够提供快速、高品牌的桌面体验，而且我们认为他们迄今为止的进展证明了为什么它被证明是一个很好的选择。

Flutter 3 也改善了许多基本原理，改善了性能， Material 你支持，和生产力更新。

除了上面提到的工作，在这个版本中，Flutter 完全是苹果硅片上的原生开发。Flutter 自发布以来一直兼容 m1 驱动的苹果设备，现在 Flutter 充分利用了 Dart 对苹果硅的支持，使 m1 驱动的设备能够更快地编译，并支持 macOS 应用程序的通用二进制文件。

我们的 [Material Design 3](https://m3.material.io/) 工作在这个版本中已经基本完成，允许开发者利用一个可适应的、跨平台的设计系统，该系统提供动态的颜色方案和更新的可视化组件:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/10796f15c6eda1bb3755f7699442ebb986fa2f9bfe7c7dc0fd34f1c6ad95a6f7.com/max/1400/0*LM_w2DE9aM-_9J0Z)

我们详细的技术博客文章扩展了这些和许多其他新功能在扑扑 3。

Flutter 是由 Dart 提供动力的，它是一种用于多平台开发的高生产率、可移植的语言。我们在这个周期的工作包括新的语言功能，减少样板和援助可读性，实验性的 RISC-V 支持，升级连接，和新的文档。关于 Dart 2.17 所有新改进的详细信息，请查看专门的博客 [dedicated blog](https://medium.com/dartlang) 。

## Firebase 和 Flutter

当然，构建一个应用程序不仅仅是一个 UI 框架。应用程序发布者需要一套全面的工具来帮助你构建、发布和运行你的应用程序，包括认证、数据存储、云功能和设备测试等服务。有许多服务支持 Flutter，包括 Sentry、 AppWrite 和 AWS Amplify。

谷歌提供的应用程序服务是 Firebase，SlashData 的开发者基准研究显示，62\% 的 Flutter 开发者在他们的应用程序中使用 Firebase。因此在过去的几个版本中，我们一直在使用 Firebase 来扩展和更好地集成 Flutter，使其成为一流的集成。这包括将 Flutter 的 Firebase 插件升级到 1.0，添加更好的文档和工具，以及像 FlutterFire UI 这样的新小部件，它们为开发人员提供了可重用的用于验证和配置文件屏幕的用户界面。

今天我们将宣布 Flutter/Firebase 集成到 Firebase 提供的完全支持的核心部分的毕业。我们正在将源代码和文档移动到主要的 Firebase repo 和 site 中，您可以指望我们进化 Firebase 支持与 Android 和 iOS 一致的 Flutter。

此外，我们还对支持使用 Crashlytics 的 Flutter 应用程序进行了重大改进，Crashlytics 是 Firebase 流行的实时崩溃报告服务。通过 F[Flutter Crashlytics plugin](https://firebase.google.com/docs/crashlytics) 的更新，你可以实时跟踪致命错误，提供给你其他 iOS 和 Android 开发者同样的功能。这包括重要的警告和指标，如“无崩溃用户”，帮助您保持您的应用程序的稳定性。Crashlytics 分析管道已经升级，以改善 Flutter 崩溃的集群，使其更快地进行分类、优先排序和修复问题。最后，我们简化了插件的设置过程，这样它只需要几个步骤就可以启动并运行 Crashlytics，直接从你的 Dart 代码开始。

### Flutter 休闲游戏工具包

对于大多数开发者来说，Flutter 是一个应用程序框架。但是，随着 Flutter 提供的硬件加速图形支持以及 [Flame](https://flame-engine.org/) 等开源游戏引擎的使用，围绕休闲游戏开发的社区也在不断壮大。我们希望让休闲游戏开发者更容易入门，所以在今天的 i/o 大会上 [Casual Games Toolkit](https://flutter.dev/games) ，我们将宣布休闲游戏工具包，它提供了一个初学者工具包的模板和最佳实践，以及广告和云服务的信用。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/eacba1086ac57a12a451b5a038c2c3b50565a2380a29adff3183a292b04e52ea.com/max/1400/0*wK4YI3N-Hh2vtDQ2)

尽管 Flutter 并不是为高强度的 3 d 动作游戏设计的，但即使是这些游戏中的一些也已经转向 Flutter 的非游戏用户界面，包括像 [PUBG Mobile](https://play.google.com/store/apps/details?id=com.tencent.ig) 这样拥有数亿用户的流行游戏。对于 i/o，我们认为我们应该看看我们的技术能走多远，所以我们创造了一个有趣的弹球游戏，由 Firebase 和 Flutter 的网络支持提供动力。I/o 弹球提供了一个自定义表格，它是围绕谷歌最喜欢的四个吉祥物设计的: Flutter’s Dash、 Firebase’s Sparky、 Android 机器人和 Chrome 恐龙，并允许你与其他人竞争最高分。我们认为这是一个有趣的方式来展示 Flutter 的多功能性。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/d441ff7dca1589457be1f9f5f9c7d7b6202f67a72d45a12bf8bbda098f414a23.com/max/1400/0*87xQ1AYdEF2YrmQ1)

## 由 Google 赞助，社区支持

我们喜欢 Flutter 的一点是，它不仅仅是一款谷歌产品，而是一款“人人”产品。开源意味着我们都可以参与其中，并且与它的成功有利害关系，无论是通过贡献新的代码或文档，创建赋予核心框架新的超能力的包，编写教授他人的书籍和培训课程，或者帮助组织活动和用户团体。

为了展示社区最好的一面，我们最近与 DevPost 合作赞助了一个益智黑客挑战，让开发者有机会通过 Flutter 重新想象经典的滑动益智游戏来展示他们的技能。这证明了网络、桌面和移动设备是如何结合在一起的: 现在我们都可以在线或通过商店玩游戏。

我们把这个视频放在一起，展示一些我们最喜欢的提交作品和获奖作品; 我们认为你会喜欢它:

Thank you for your support of Flutter, and welcome to Flutter 3\!

感谢您对 Flutter 的支持，欢迎来到 Flutter 3！

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c16497394dc78c1b4955492a22c7c781d18e3413ae9c95d50b3c2385f67d3383.png)

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
