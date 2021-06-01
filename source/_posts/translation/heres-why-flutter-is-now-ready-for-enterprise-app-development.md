---
title: 为什么 Flutter 已经为企业应用程序开发做好了准备
tags: flutter
categories: 译文
date: 2021-06-02 00:00:00
---

![](2021-06-01-22-24-30.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 猫哥说

这篇文章很硬，如果你正在架构一个 APP，或者你正在写 flutter 技术论文，可以参考下。

## 原文

> https://betterprogramming.pub/heres-why-flutter-is-now-ready-for-enterprise-app-development-1986ef2cd3e3

## 正文

为企业应用程序开发做好准备了吗？长期以来，这一直是开发者提出的最多的问题之一。但是在 Beta 发布之后，这个平台展示了很多前景，并且提供了大量的本地化特性，使得本地化应用程序的开发变得更加容易。

虽然移动应用开发市场的确正在向强大的应用开发过程体验转变，但主要障碍之一是 iOS 和安卓应用开发之间的分工。由于这两种操作系统的用户遍布全球，企业在锁定受众时必须注意这一点，以确保自己的品牌不会遗漏任何市场。

本文将帮助您了解为什么颤振是准备授权企业。

当谷歌在 2018 年宣布 Flutter 的第一个稳定版本(1.0)时，人们很想知道它是否是一个很好的企业级移动应用程序开发工具。

快进到今天，我很自豪我决定尝试 Flutter 为企业应用程序开发。

我知道你们很多人都想知道为什么 Flutter 在应用程序开发方面获得了广泛的关注，因为它的定位与其他跨平台开发工具没有什么不同，这些跨平台开发工具提供了原生的 iOS 和 Android 应用程序。建立一次，部署到每个地方！

不像其他人，我避免陷入这些陈述！

在一年的时间里，现在有超过 4000 个插件支持 Flutter 应用程序。媒体，YouTube，Stack Overflow，以及更多的网站都充斥着建议 Flutter 可以帮助你为不同的商业领域创建各种各样的应用程序的内容。

Flutter 是王道ーー至少在企业应用程序开发解决方案方面是如此。这不仅仅是我的观点，也是来自移动应用开发行业的压倒性的声音。

https://www.xicom.ae/services/mobile-app-development/

根据谷歌的统计，每月有 50 万开发者使用谷歌软件开发工具包。

https://venturebeat.com/2020/04/22/google-500000-developers-flutter-release-process-versioning-changes/

另外，Flutter 的 SDK 是 GitHub 上增长第二快的项目，它使得它在业界的竞争对手中脱颖而出。所有这一切都指向一个欣欣向荣的社区，渴望分享，成长，并提高 Flutter！

https://techmonitor.ai/technology/software/top-10-programming-languages

所有这些事实，现在是时候来决定 Flutter 和它的库是否准备好在 2021 年开发企业移动应用程序了。

在我们直接进入你可能需要开发特定的企业安卓应用程序的需求之前，我的简单目标是为每个需求找到至少一个 Flutter 解决方案，让你相信 Flutter 现在已经准备好开发几乎没有混合代码需求的企业应用程序。

当然，业务应用程序的需求因项目而异，因此最终的结果可能会有所不同。

让我们先简单介绍一下 Flutter。

### 框架概述

Flutter 是一个开源的 UI 软件开发工具包，广泛用于跨平台应用程序开发。通过使用单一的代码库，移动应用程序开发公司可以创建各种类型的应用程序，从简单的聊天应用程序到按需购物应用程序。它与其他框架的不同之处在于 Flutter 应用程序是用 Google 的面向对象程序设计语言 Dart 编写的。

谷歌选择了 Dart，同时考虑了以下四点:

- Productivity 生产力
- Faster allocation 更快的分配
- Object orientation 面向对象
- High performance 高性能

有了这些事实，Flutter 可以帮助开发者为 iOS、 Android 和网络平台开发本地应用程序，这些应用程序可以在多个平台上无缝运行。

UI 性能、源代码成熟度、安全测试和功能是开发人员在为不同平台设计应用程序时遇到的主要挑战。颤振应用程序开发可以帮助您解决这样的问题，以极其轻松。

现在，企业应用程序到底是什么，构建它的主要需求是什么，Flutter 是如何对过程做出贡献的？

### 企业级移动应用

首先，企业应用程序是否仅仅意味着领先品牌的发展。

无论你是一个进步的创业公司还是一个领先的组织，企业应用程序都是为所有人服务的。这些应用程序是专门为企业劳动力的有限和受保护的使用而设计和开发的。对于企业应用程序，管理员可以集中处理数据，实现大规模的自动化，并维护流畅的工作流。但是为了使其功能化，企业应用程序需要许多特性、高安全性和具有健壮框架的无缝 UI 设计，以确保高性能。

让我们了解一下构建企业应用程序的具体要求，看看 Flutter 和它的库包生态系统是否已经为这项任务做好了准备。

以下是我选择的基本要求。在每一个需求类别中都有很多需要覆盖的内容，尽管我已经试图简要地假设读者已经熟悉 Flutter 的基本特性。

- Layered architecture 分层结构
- Development environment 发展环境
- User interface 用户界面
- Hardware features 硬件特性
- App security 应用程序安全性
- Miscellaneous requirements 杂项需求

### 分层架构，确保更好的功能性

在开发企业应用程序时，确保它具有分层架构，以确保无缝功能，并通过不同开发团队的各种技能提高生产率。

当 layers 被插入时，开发者必须想办法提供以下功能:

- 更好地访问文档完整的设计模式
- 大量的开发人员同时处理代码库
- 很容易理解各种各样的应用程序功能

通过为网络资源、本地存储、 SQLite 数据库以及通过插件插件访问硬件提供简单而安全的网络，Flutter 在这里大放异彩。

让我们来看看如何:

- State management : 当前位置 Flutter 的应用程序架构的核心，而 Google 最近的建议是使用 Provider 框架，这个框架更容易理解和构建。同时其他状态管理如 Redux、 BLoC、,InheritedWidget 继承的 widget, setState, etc., they coexist within reason. 、 setState 等，它们在合理范围内共存
- RxDart : 如果 Dart 的流和异步包不能满足您的异步编程需求，那么考虑 RxDart 是一个明智的决定。它与 Flutter 和状态管理框架无缝集成
- Background processing 后台处理: 它允许计算密集的工作，以执行在应用程序，同时保持用户界面的响应在同一时间。根据后台处理需求的复杂程度，您可能需要采用纯 Dart 实现之外的本地平台特性
- Dependency injection: 依赖注入: 为了使你的应用程序代码单元独立和可重用，移动应用程序开发人员可以使用 依赖注入.这是一种使代码更容易测试的设计模式。GetIT 定位器是一个简单易用的 DI 库，它与状态管理框架无缝地协同工作，以确保应用程序层的分离
- JSON serialization/deserialization JSON 序列化/反序列化: 对于任何 RESTful 客户机都很重要，在大多数企业应用程序中也很常见
- Deep linking 深度连接: 它提供正确的导航从一个网站或推送通知启动应用程序内的特定领域
- Local storage 本地存储: 提供本地存储的少量键/值数据，然后帮助您的应用程序工作，即使应用程序是背景或停止
- SQLite: 可用于处理大量的结构化数据
- Push notifications 推送式通知 : 对于企业级应用程序，通常需要后端集成，以帮助您向用户通知到期日期、关于服务的提醒等。对于此，firebasmessaging 是一站式解决方案

### 本地安卓和 iOS 应用的开发环境

对于开发环境，开发人员可以在 Android Studio/IntelliJ 和 Visual Studio Code 之间选择他们的 Flutter IDE。所有这些都在 Mac，PC，Linux 和 Chromebook 上得到了很好的支持。所有你需要的是采用 Flutter 应用程序开发与正确的经验。

在这些 ide 中，开发人员可以实现构建、设备部署、调试和性能分析。但是要为本地 iOS 创建一个开发环境/部署，需要在 Mac 上使用 Xcode。

- Scalability 可伸缩性: 颤振应用程序很容易扩展，因为它基于 Dart 生态系统，引入 Dart 包来提供外部库的功能。颤振项目可以重构成颤振飞镖软件包，提供了另一种方式来分割大型团队的工作，使其更容易扩大应用程序
- Testability 可测试性: 是否正在使用 unit tests 单元测试, widget tests 小部件测试, or ，或 integration tests 集成测试, 每一个 Flutter 小工具可以很容易地测试。所有这些测试工具都允许最大的测试覆盖率，并且仅受可用时间和资源的限制
- Continuous integration/continuous delivery 持续集成/持续交付: 使用卓越的安卓和 iOS 工具集将应用程序部署到 Google Play 商店或苹果商店，使它们可以在任何现有的企业移动 CI/CD 设置中使用

与 Flutter 合作的移动应用程序开发公司可以将大部分时间花在 Flutter/Dart 环境中，同时将 Flutter 应用程序部署到安卓和 iOS 设备上。知识如何建立和签署应用程序和供应配置文件，等是必不可少的实施一个成功的扑动应用程序。

### 用户界面

应用程序界面在用户体验中扮演着重要角色。企业移动应用程序努力专注于提供优秀的用户界面。为了满足这个需求，Flutter 提供了一套全面的 Android 和 iOS 的高精度演示。

> 为了让你的用户界面更有吸引力，你可以整合:

- Animations 动画: 很容易开始学习动画，他们可以扩展到任何复杂性。对于广泛使用的 Flutter，你可以包括耀斑，这是一个完整的 2D 矢量动画库。应用程序开发公司正在广泛使用这个工具来定制具有无缝接口的企业应用程序
- Page transitions 页面转换: 他们可以是一个完美的例子，学习如何导航之间的应用程序页面与动画可以实现
- Paging 分页或无限滚动列表视图: 当需要在不占用大量设备内存的情况下向用户显示大量数据时，这是一个常见的需求。这是年的最新趋势移动应用开发服务，因为 Flutter 提供了丰富的内容存储库的无限滚动
- Image loading/caching library 图像加载/缓存库: 它提供了一种快速、简单的方法来处理许多图像，包括在基本图像或 SVG 图像不够好的情况下进行缓存。因此，Flutter 应用程序开发人员可以很容易地通过加载和缓存库管理图像

最后，你可以在 Flutter 移动应用程序上提供谷歌和苹果地图。

### 硬件特性需求

无论你是如何出色地定制了你的应用程序，并提供了一系列广泛的功能，没有一个应用程序是完全可以在没有硬件功能支持的情况下工作的。

因此，当你雇佣应用程序开发人员为企业员工/用户/员工开发企业应用程序时，你需要硬件和软件支持:

https://www.xicom.ae/solutions/hire-developers/

- Camera 相机
- Accelerometer 加速度计
- GPS 全球定位系统
- Biometric authentication, including fingerprint and face ID 生物计量学，包括指纹和脸部识别码
- Microphone 麦克风

### 移动应用安全

安全是一个企业无法破坏的领域ーー无论是基本的企业应用程序还是高级应用程序。保护应用程序数据安全是企业最关心的问题之一。因此，在创建企业应用程序时需要注意各种各样的事情。毫无疑问，这是一个非常广泛的话题，但我将把它缩小到几个具体点，使之易于理解。

假设 Flutter 应用程序是建立在安卓和 iOS 沙箱环境之上的，所以每个 Flutter 应用程序对于本地 iOS 和安卓应用程序都有固有的安全方面。

最基本的要求，如身份验证(生物统计学，拇指指纹，两级密码)很好地迎合了 Flutter 的简单认证。

> 以下是你可以考虑的其他认证提供商:

- Amazon 亚马逊
- Facebook
- GitHub
- Google
- Dropbox
- Azure Active Directory Azure 活动目录
- LinkedIn
- Instagram 图片分享
- Microsoft Live Connect 微软 Live Connect

SSL 证书固定也很重要，因为它减少了由于共享服务器而发生攻击的可能性。它确保 web (HTTPS)请求的安全，并且受到支持。

安全存储提供了一种在设备上安全地存储少量密钥或有价值信息的方法，即使在没有互联网连接的情况下也能让你的应用程序工作。

### 杂项要求

除了以上所有的要求，这里还有一些在开发企业应用程序时需要考虑的多重要求:

- Analytics 分析: 分析库可在 Flutter 上满足这一要求
- Error reporting 错误报告: 开发人员可以使用 Flutter’s Sentry library 插件.
- Third-party 第三方或开放源码库: 这份第三方插件的清单 在你开始在你的应用里随机挑选和使用一个之前
- Generating QR codes: 生成二维码: 无论是为了应用程序的高级功能还是为了安全目的,二维码扫描很重要

考虑一下这个:

- 分享应用程序细节 社交媒体账号
- 访问个人接触名单
- 允许相机或位置，而使用的应用程序
- 发送短信或多媒体信息或接收短信 一次性密码
- 在应用程式内使用应用内支付 SDK.

### Flutter 的跨平台支持超越 iOS 和 Android

我们只讨论了 Flutter 对本地 iOS 和 Android 应用程序的支持，但是 Flutter 正在极大地扩展对 web、 macOS、 Windows 和 Linux 的支持。开发一个可以在所有这些平台上无缝部署和执行的应用程序，只需要使用一段代码，这是您一直以来努力的目标。

与此同时，您必须接受这样一个事实，即并非所有平台都支持所有相同的特性。例如，谷歌地图现在只支持安卓、 iOS 和网络。另一方面，这些都是目前用户操作的主要平台。

通过利用 Flutter 的潜力和它广泛的小部件选择，你可以针对移动设备和网络。此外，它是更好的有响应屏幕与 Flutter 内置的应用程序，看起来不同的设备和适合用户的屏幕。所有这些都可以很容易地通过一个代码库实现。

### 总结

在应用程序开发中，Flutter 已经越来越受欢迎，但随着图书馆的广泛支持，它已经迅速成为企业在短时间内创建企业应用程序的可行选择。

最好的部分是任何行业利基中的企业、科技公司、创业公司和个人开发者都可以通过雇佣合适的移动应用开发公司来发挥其潜力并创建一个应用程序。随着一个健康和成长的颤振库包装生态系统的正确使用，也许是时候让企业闪耀在竞争的市场，并建立自己的立足点在行业的未来十年。

https://www.xicom.ae/services/mobile-app-development/

感谢 Anupam Chugh。

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
