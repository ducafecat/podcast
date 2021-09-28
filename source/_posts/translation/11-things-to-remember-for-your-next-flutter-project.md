---
title: 下一个 Flutter 项目要记住的 11 件事
tags: flutter
categories: 译文
date: 2021-09-29 00:00:00
---

![](2021-09-28-22-24-07.png)

## 原文

> https://medium.com/flutter-community/11-things-to-remember-for-your-next-flutter-project-1c7c380895ca

## 正文

创建一个新的 Flutter 项目是一件好事ーー新鲜的代码库、没有遗留代码(还没有)、空安全性、您最喜欢的软件包的最新版本等等。但是与此同时，您应该在项目开始时做出关键的决策，这些决策将为未来奠定基础，例如工具、包、文件结构、状态管理解决方案、测试计划。否则，这个项目最终会变成另一碗意大利面和肉丸。为了避免这种情况，我准备了一个列表，在我看来，这个项目中最重要的元素应该尽早决定。我希望它能帮助你，因此ーー快乐的阅读！

### 1. 静态程序分析

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bd704092466e4573308d6e471fb4afeb8ac8c7aad8c018e7cba7bd90eb37dea0.com/max/1314/0*6hBqp3qp2ZMwks8t)

I will not write messy code ( 我不会写乱七八糟的代码([source 来源](https://i2.wp.com/www.thecuriousdev.org/wp-content/uploads/2017/12/messy-code.gif?fit=657%2C352&ssl=1))

Linter 是一个静态分析工具，用于识别和标记代码中的编程错误、警告和样式缺陷，以便您修复它们。在 Flutter 上下文中，这是最容易实现的东西之一，也是保持代码干净的最有用的东西之一。

有很多不同的规则可以让你的代码遵循，但是我建议你使用一个预先定义的规则，它已经遵循了基于 Dart 样式指南的最佳实践:

https://dart-lang.github.io/linter/lints/index.html

https://dart.dev/guides/language/effective-dart/style

- [lint](https://pub.dev/packages/lint);
- [flutter_lints Flutter](https://pub.dev/packages/flutter_lints);
- [very_good_analysis](https://pub.dev/packages/very_good_analysis).

无论选择哪个包，都可以在 analysisservices \_ options. yaml 文件中添加或删除任何特定的静态分析规则。

### 2. 本地化(l10n)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/811f04d4990c001086c653aa6ff67bafe7ee361b2a7083357ba5b1c04f0bd02f.jpg)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/811f04d4990c001086c653aa6ff67bafe7ee361b2a7083357ba5b1c04f0bd02f.jpg)

本地化 [来源](https://marcosantadev.com/wp-content/uploads/Localization_Tips.jpg

什么是本地化(简称 l10n) ？

> 本地化是对产品或服务的调整，以满足特定语言、文化或期望人群的“外观和感觉”的需要ー [TechTarget](https://searchcio.techtarget.com/definition/localization)

建立一个用户感觉自然的应用程序是必要的，例如使用正确的翻译、日期和货币格式、文本方向。因此，本地化是一个基本的工具。即使您正在构建单个区域/语言应用程序，我仍然建议您尽早实现本地化，从而将文本与 UI 代码分离开来。因此，可以在不影响代码的情况下对它们进行重用和调整。

[Flutter documentation](https://flutter.dev/docs/development/accessibility-and-localization/internationalization) 精巧地解释了国际化应用程序的过程。如果缺省方式看起来太复杂，或者您需要一些有用的扩展和帮助器方法，那么有一些流行的第三方包，比如 [easy_localization](https://pub.dev/packages/easy_localization) ，可以帮助您完成本地化过程。

### 3. 环境(有一些味道)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/3c4d6600a7e7c3f3b52d8786d5e90b563f5bf451fbf9100bed67762dbe7b9818.png)

Programming environments [来源](https://jaxenter.com/wp-content/uploads/2020/07/production1.png)

我敢打赌，当有人在生产中损坏数据或删除整个用户表时，您至少已经从您的环境中听说过一个案例(没有双关意思)。相信我，这一点也不好玩。因此，为你的项目创建不同的环境是一个很好的实践:

(本地)环境ー用来让你抓狂: 在代码中做实验，直接在数据库中更改数据，使用快捷键并硬编码认证标记或提供模拟数据。玩得开心，并提供这些功能

- 帮助您验证代码中的更改，用“真实”数据(通常在这种环境中使用生产数据快照)测试特性，并在将应用程序发布到生产环境之前验证它。如果你的团队中有质量保证工程师，这就是他们发光发热的地方

- 环境ー由真实用户使用的环境，其中数据损坏是不可接受的(请始终进行备份)

拥有这样的环境可以帮助您在这些更改到达用户手中之前安全地实验和验证特性。

现在，另一个部分——味道。不，不，我们不是在讨论甜的、咸的或酸的东西ーー这只是在编程中用来描述应用程序的不同构建变体的另一个术语。例如，您希望使图标和标题、 API 端点或任何其他配置对于每个特定环境都不同。为此，您需要定义一种不同的“味道”，这种味道在为特定环境构建应用程序时使用。这里有一些关于如何为 [create flavours for Flutter](https://flutter.dev/docs/deployment/flavors) 。

### 4. 持续集成和持续交付

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/743197c473ae4d8361bf6582ed935c01e76feb8fdf2578799edaa9ac76894c09.png)

持续集成阶段(CI)和持续交付集成阶段(CD)[source 来源](https://www.parasoft.com/wp-content/uploads/2021/04/CICD_CICD.png)

在引入不同的环境之后，自然而然的下一步是自动化构建、测试和发布应用程序的过程。CI/CD 本身就是一个相当复杂的主题，无论如何我都不是这个领域的专家，因此我建议搜索一些关于如何使应用程序开发的不同阶段自动化的其他资源。

然而，有很多 [NoOps](https://www.cio.com/article/3407714/what-is-noops-the-quest-for-fully-automated-it-operations.html) 解决方案是与 Flutter 兼容的，所以你可以轻松自动化你的开发过程:

- [Appcircle](https://appcircle.io/);
- [Codemagic 代码魔法](https://codemagic.io/);
- [Bitrise](https://www.bitrise.io/);
- [VS App Center](https://appcenter.ms/) (还没有 Flutter 集成，但是有一些资源可以帮助你设置所有的东西)

这些解决方案中的任何一个都可以解决问题ーー简单地说，选择一个符合你的需要和预算的方案。

### 5. 后端代码

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/9b1621f1e675f665d9f0487fba9bef324bb02be3766afc5fa788baef8af6fe42.jpeg)

( 另一个关于后端的迷因([source 来源](https://i.imgur.com/G7hDEvr.jpeg))

您是否已经用任何特殊的或者不那么花哨的编程语言实现了后端？很好，您可以跳过这一步，但我仍然建议您查看一些云解决方案，以供将来参考。

在简化版本中，应用程序的后端部分有两个选项:

1.  使用您喜欢的任何编程语言和框架实现自定义后端解决方案，但是稍后将处理所有[DevOps](https://en.wikipedia.org/wiki/DevOps) 让你的代码和数据可以从应用程序中访问
2.  使用任何云解决方案来加速开发过程，并将大部分 DevOps 留给云供应商

如果你觉得第二个选择很有吸引力，可以从支持 Flutter 的云平台中选择一些:

- [Google Firebase 谷歌 Firebase](https://firebase.google.com/);
- [AWS Amplify](https://aws.amazon.com/amplify/);
- [Supabase 女名女子名](https://supabase.io/);

云平台为您的应用程序提供身份验证、数据库、存储、 API 选项和许多其他特性。如果您只需要验证这个想法并快速构建 MVP，而不需要花费大量时间在成熟的后端解决方案上，那么上述任何一个都是一个很好的选择。

### 6. 日志记录，崩溃数据和分析

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/0573b48428be13b9963d7702019654887a0a75d4420a5778a9191a4b144e52f7.com/max/1276/0*P3_7XCAmphsbs09H)

( 哎呀，出问题了[source 来源](https://image.slidesharecdn.com/googleiofirebase-160628162912/95/google-io-extended-vientiane-2016-introducing-firebase-17-638.jpg?cb=1467131395))

伐木被低估了ーー在这里，我说过了！一切都很好，直到出现问题，你需要了解这方面的信息。当我们讨论什么应该记录，什么不应该记录时，总有一个灰色地带。但有一件事情始终是明确的: 您必须知道应用程序何时崩溃以及问题的原因。你收集的关于这个事件的数据越多，就越容易发现和解决这个问题。

像 [Sentry](https://sentry.io/), [Firebase Crashlytics](https://firebase.google.com/products/crashlytics), [Datadog](https://www.datadoghq.com/) 、 Datadog 这样的服务可以帮助你记录最重要的数据、崩溃报告，甚至在你的应用程序或相关服务出现故障时设置通知。

另一种类型的日志记录是收集用户数据用于分析目的。当您构建一个全新的、可能是一流的产品时，了解用户的需求、他们的行为以及他们如何使用应用程序是至关重要的。为此，各种分析工具可以集成到您的 Flutter 应用程序，如 [Firebase Analytics](https://firebase.google.com/products/analytics) 分析，[App Center Analytics](https://docs.microsoft.com/en-us/appcenter/analytics/) 中心分析和许多更多。

### 7. 应用程序 branding

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/4b3911aca61e285e931ab1d73320fef8fc4bdb7961f81c5dc828b04045901971.com/max/1400/0*832kkYT17YlRJOQj)

Material theming ( Material 主题化([source 来源](https://9to5google.com/wp-content/uploads/sites/4/2018/05/material-theming1.png?w=1600))

任何应用程序或品牌的主要目标之一就是获得认可。使用正确的颜色调色板，标志，图标，设计元素，内容，字体，有时甚至布局使你的产品脱颖而出。这就是应用程序的品牌化，在开始的时候准备好基本的部分将会在整个项目中节省你很多时间。

如果你已经准备好了你的 UI 原型或者设计组件，现在是时候把它们转移到你的应用程序中并定义主题---- 颜色、字体、形状等等。为了方便起见，一个叫 [Mike Rydstrom](https://twitter.com/RydMike) 的好人为这个 [flex_color_scheme](https://pub.dev/packages/flex_color_scheme) 方案创建了一个出色的包。

### 8. 项目结构和状态管理

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/4526ea77f5b7b8029bc8ead79fdcef91692c0b9aae0a1e9da8ac8c5cf835949b.gif)

( Flutter 状态管理([source 来源](https://flutter.dev/docs/development/data-and-backend/state-mgmt/intro))

是的，有争议的那个。需要澄清的是，根本没有所谓的“最佳国家管理解决方案”或“最佳应用程序架构”——如果有人不这么认为，请记住，他们可能也会先把牛奶倒进碗里，然后再倒麦片。这是最糟糕的部分ーー我不能教你最好的方法。我只能提供几个选项或分享我的偏好。

下一个 Flutter 项目的几个文件结构选项:

- [Clean Architecture 清洁的建筑](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) — ー清晰的关注点分离，是否长久存在。老实说，我不喜欢这样。我觉得这个概念中有太多的抽象，可能会减缓开发进程
- [Layered Architecture 分层建筑](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html) ー依赖于将数据、业务和表示逻辑分为不同层次的想法。这样的文件结构对于中小型项目来说工作得很好，但是我觉得当项目增长时，这些层会变得越来越不堪重负
- Modular Architecture (I have described this concept 模块化体系结构(我已经描述了这个概念[here 这里](https://mkobuolys.medium.com/flutter-shopping-app-prototype-lessons-learned-16d6646bbed7)) ー把程式码分割成不同功能的模组，以便不同的模组互相作用。这是我最喜欢的一个ー它与集团国家管理解决方案(TEAM BLoC，YEAH!)顺利地工作对于大型项目来说，规模很大。然而，它也带来了一些挑战，比如公共逻辑放在哪里，不同的模块应该如何通信等等

关于《 Flutter 》中的国家管理问题，我想我们已经到了可以把整个会议都用来讨论这个问题的时候了，但是事后还没有最后的答案。我只想补充一点，选择一个你觉得最舒服的。你可以在 [here](https://flutter.dev/docs/development/data-and-backend/state-mgmt/options) 找到一个全面的选项列表。

### 9. 代码生成

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2098abe11b01c29609565b598ec606f3c9782f1e1e839fbcbbe1572b275b5742.jpg)

Code generation ( 代码生成([source 来源](https://resqsoft1.files.wordpress.com/2013/05/customizing_instant_code_generation_illu.jpg))

如果您想简化一些步骤并节省一些开发时间，您可以在项目中使用代码生成。少编码，多交付！ [Code less, deliver more](https://youtu.be/GGwTfsPDiO0?t=514)

有一系列不同的工具可以使用，无论是处理本地化、资产、解析 JSON、生成模型类、实现服务定位器、路由，还是处理不可变状态。唯一要做的就是调查可用的工具和包，并选择最好的工具和包来满足您的项目需求。

对于一个快速 Flutter 项目启动，我建议检查出 [Very Good CLI](https://github.com/VeryGoodOpenSource/very_good_cli) 。这将节省您几个小时的配置(不幸的是，我已经学会了艰难的方式)。

此外，上个月我还做了一个关于 [code generation](https://youtu.be/kg60JQJ-tBE?t=13157) 的演讲ーー它可能是 Flutter 代码生成旅程的起点，所以看看吧！

### 10. 测试策略

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/967e5cc34b411cc892cd4688ecbca44e5235b50073c79497494ce804448d2a03.jpg)

Application testing ( 应用程式测试([source 来源](https://codoid.com/wp-content/uploads/2020/08/strategies-for-mobile-app-testing-1.jpg))

用测试来覆盖 100% 的代码是好还是坏？当然，这很棒，但是代价是什么呢？这样想的话，您可能会掉进一个注定要陷入困境的深渊，在那里您花费更多的时间编写测试，而不是开发特性。为了避免这种情况，您需要一个测试策略。

不要误会我的意思ー用测试覆盖代码是一件好事，而且代码中越隐蔽的地方，在实现新特性时就越安全。只是，在我看来，与编写测试所花费的时间相比，您应该找到测试仍然为您带来更多价值的平衡点。例如，这是我的测试策略:

- Business logic (services, repositories, BLoCs) should be covered 85-100% in 业务逻辑(服务、存储库、集群)应该在**unit/integration tests 单元/集成测试** ー这是所有应用程序中最重要的部分，因此我看到了测试的很大价值;
- **Widget tests 小部件测试** 应该包含所有可重用的 UI 组件。当单个组件被正确测试时，您可以开始测试单个屏幕，但是不要太详细
- **End-to-end tests 端到端测试**, 涵盖了主要的应用程序流以及与 UI 的交互。没有深层次的魔法ーー只是经历一些关键的工作流程。它们包含的屏幕越多越好
- 当整个用户界面准备就绪并实现时ー**golden tests 黄金试验** 确保 UI 不会受到以后更改的影响

老实说，我仍然在测试中寻找中庸之道，但是相信我，你会在一个又一个项目中做得更好。

### 11. 自述文件

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c6d6a081c0e8c70e6fb812ce0e074fbc48f07d43525787591da18f9d2b714b51.com/max/1400/0*IFzSuqZAadpUJ7f7)

Make a README ( 自述文件([source 来源](https://www.makeareadme.com/images/open-graph-logo.png?v=20181203))

你没听错，文件。README 文件是项目中最重要的文档，尤其是在团队中工作时。

您是否刚刚引入了一个需要代码生成的新解决方案？您是否刚刚添加了一个有用的 bash 脚本来自动完成这个过程？您是否实现了一个必须在项目中的任何地方使用的全局记录器？我们无法读取你的思想ーー在 README 文件中提到这一点！

没有太多的文档(至少我没有遇到过这种情况) ，只有缺乏关于项目和代码的信息。所有生成、测试和运行代码的命令、各种文件结构决策、图表、外部工具和服务、关于不同环境的信息(没有 SECRET KEYS)都应该放在这里，并保存在一个单独的地方。这是一个无聊的工作，但却是一个非常有价值的工作！

Phew, what a ride… ( 这车真不错... .[source 来源](https://giphy.com/gifs/relief-z23hGvopHu7w4))

就是这样! 谢谢你花时间阅读这篇文章。

我错过了什么吗? 在评论中提及吧! 在构建一个新的 Flutter 应用程序时，你的清单是什么？

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
