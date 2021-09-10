---
title: Flutter 2.5 的新功能
tags: flutter
categories: 译文
date: 2021-09-10 00:00:00
---

![](2021-09-10-08-26-51.png)

## 原文

> https://medium.com/flutter/whats-new-in-flutter-2-5-6f080c3f3dc

## 正文

您好，欢迎来到 Flutter 2.5！这是一个大版本，在 Flutter 版本历史上排名第二：关闭了 4600 个问题，从 252 个贡献者和 216 个审阅者合并了 3932 个 PR。如果我们回顾过去一年，我们会看到 1337 位贡献者创建了 21,072 个巨大的 PR，其中 15,172 个被合并。虽然“Flutter 中的新功能”博客文章重点介绍了新功能，但我们在 Flutter 方面的第一项工作始终是确保您拥有所需的功能，并达到最高的质量水平。 事实上，此版本继续了许多重要的性能和工具改进，以跟踪您自己的应用程序中的性能问题。同时，还有许多新功能，包括对 Android 的全屏支持、更多 Material You（也称为 v3）支持、更新的文本编辑以支持可切换的键盘快捷键、在 Widget Inspector，对在 Visual Studio Code 项目中添加依赖项的新支持，对从 IntelliJ/Android Studio 中的测试运行获取覆盖率信息的新支持，以及一个全新的应用程序模板，为您的真实 Flutter 应用程序提供更好的基础。此版本充满了令人兴奋的新更新，让我们开始吧。

### 性能：iOS 着色器预热、异步任务、GC 和消息传递

此版本带来了多项性能改进。此列表中的第一个 PR 用于从离线训练运行 ( [#25644](https://github.com/flutter/engine/pull/25644 "https://github.com/flutter/engine/pull/25644") ) 中连接 Metal 着色器预编译，它（如我们的基准测试所示）将最坏情况的帧光栅化时间减少了 2/3 秒，将第 99 个百分位帧减少了一半。我们继续在减少 iOS 卡顿方面取得进展，这是沿着这条道路迈出的又一步。然而，着色器预热只是卡顿的来源之一。以前，处理来自网络、文件系统、插件或其他隔离的异步事件可能会中断动画，这是另一个卡顿的来源。以下改进调度策略 ( [#25789](https://github.com/flutter/engine/pull/25789 "https://github.com/flutter/engine/pull/25789")) 在此版本的 UI 隔离的事件循环中，帧处理现在优先于处理其他异步事件，从而在我们的测试中消除了此源的卡顿。

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/588cf9e40b6f8e751f638b25b8d38ae7d5a47fe24040f8df88e91bbbcced09e3.png)

_由于前后处理异步事件结果导致的帧滞后_

另一个导致卡顿的原因是垃圾收集器 (GC) 暂停 UI 线程以回收内存。以前，某些图像的内存只会延迟回收以响应 Dart VM 执行的 GC。作为早期版本中的解决方法，Flutter 引擎会向 Dart VM 暗示图像内存可以通过 GC 回收，这在理论上可以导致更及时的内存回收。不幸的是，在实践中，这导致了太多的主要 GC，并且有时仍然无法足够快地回收内存以避免内存受限设备上的低内存情况。在这个版本中，未使用的图像的内存被急切地回收（[#26219](https://github.com/flutter/engine/pull/26219 "https://github.com/flutter/engine/pull/26219")、[#82883](https://github.com/flutter/flutter/pull/82883 "https://github.com/flutter/flutter/pull/82883")、[#84740](https://github.com/flutter/flutter/pull/84740 "https://github.com/flutter/flutter/pull/84740")），大大减少了 GC。

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a4e0292ebdf7787351e7e3c5834092d2361d1b75774556656a79aadfafa382fa.png)

_添加修复程序之前和之后的 GC 以急切地回收未使用的大图像内存_

例如，在我们的一项测试中，播放 20 秒动画 GIF 从需要 400 多次 GC 变为只需要 4 次。更少的主要 GC 意味着涉及图像出现和消失的动画将减少卡顿，并消耗更少的 CPU 和功率。 Flutter 2.5 的另一个性能改进是在 Dart 和 Objective-C/Swift (iOS) 或 Dart 和 Java/Kotlin (Android) 之间发送消息时的延迟。通常作为[调整](https://docs.google.com/document/d/1oNLxJr_ZqjENVhF94-PqxsGPx0qGXx-pRJxXL6LSagc/edit%23heading%3Dh.9gabvat7tlxf "https://docs.google.com/document/d/1oNLxJr_ZqjENVhF94-PqxsGPx0qGXx-pRJxXL6LSagc/edit#heading=h.9gabvat7tlxf")消息频道的一部分，从消息编解码器中删除不必要的副本可将延迟减少多达 50%，具体取决于消息大小和设备（[#25988](https://github.com/flutter/engine/pull/25988 "https://github.com/flutter/engine/pull/25988")，[#26331](https://github.com/flutter/engine/pull/26331 "https://github.com/flutter/engine/pull/26331")）。

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2468f9052db2e005813c30039c146011dbfd6f6555bee1d8844c1d8e12333986.png)

_之前和之后的 iOS 消息延迟_

您可以[在](https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af "https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af")Aaron Clarke[撰写](https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af "https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af")的[提高 Flutter](https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af "https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af")中的[平台通道性能](https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af "https://medium.com/flutter/improving-platform-channel-performance-in-flutter-e5b4e5df04af")博客文章中阅读有关这项工作的详细信息。 如果您的目标是 iOS，那么最后一个性能更新：在此版本中，在 Apple Silicon M1 Mac 上构建的 Flutter 应用程序在 ARM iOS 模拟器 ( [#pull/85642](https://github.com/flutter/flutter/pull/85642 "https://github.com/flutter/flutter/pull/85642") )上本地运行。这意味着 Intel x86_64 指令和 ARM 之间没有 Rosetta 转换，这会提高您的 iOS 应用程序测试期间的性能，并允许您避免一些微妙的 Rosetta 问题（[#74970](https://github.com/flutter/flutter/issues/74970%23issuecomment-858170914 "https://github.com/flutter/flutter/issues/74970#issuecomment-858170914")、[#79641](https://github.com/flutter/flutter/issues/79641 "https://github.com/flutter/flutter/issues/79641")）。这是全面支持 Flutter for Apple Silicon 的又一步。请继续关注更多。

### Dart 2.14：格式、语言特性、发布和 linting 开箱即用

当然，如果没有 Dart 语言和构建它的运行时，Flutter 就不是 Flutter。此版本的 Flutter 随 Dart 2.14 一起发布。[新版本的 Dart](https://medium.com/%40mit.mit/announcing-dart-2-14-b48b9bb2fb67 "https://medium.com/@mit.mit/announcing-dart-2-14-b48b9bb2fb67")带有新的格式，使[级联](https://dart.dev/guides/language/language-tour%23cascade-notation "https://dart.dev/guides/language/language-tour#cascade-notation")更加清晰，新的 pub 支持忽略文件，以及新的语言功能，包括传奇的三重移位运算符的回归。此外，也是 Dart 2.14 最好的事情之一，是此版本创建了一组标准的 lint，在新的 Dart 和 Flutter 项目之间共享，开箱即用。

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/990b950eea59097fb5a2f3e5ab6b69dbcf4d8dda59310c3de672eddf64fcfd42.png)

_`flutter create` 开箱即用，带有一个 analysis_options.yaml 文件，其中预先填充了推荐的 Flutter lints_ 您不仅会在创建新的 Dart 或 Flutter 项目时获得这些 lint，而且[只需几个步骤](https://flutter.dev/docs/release/breaking-changes/flutter-lints-package%23migration-guide "https://flutter.dev/docs/release/breaking-changes/flutter-lints-package#migration-guide")，您也可以将相同的分析添加到现有应用程序中。有关这些 lint 的详细信息、新语言功能等，请查看[Dart 2.14 的发布公告](https://medium.com/dartlang/announcing-dart-2-13-c6d547b57067 "https://medium.com/dartlang/announcing-dart-2-13-c6d547b57067")。

### 框架：Android 全屏、Material You & 文本编辑快捷方式

Flutter 2.5 版本包括对该框架的许多修复和改进。从[Android](https://github.com/flutter/flutter/pull/81303 "https://github.com/flutter/flutter/pull/81303")开始[，我们修复了一系列与全屏模式相关的问题](https://github.com/flutter/flutter/pull/81303 "https://github.com/flutter/flutter/pull/81303")，在它们之间有近 100 次竖起大拇指。模式本身的名称使其成为我们最喜欢的新功能之一：_向后倾斜、粘性、粘性沉浸和边缘到边缘_。此更改还添加了一种在其他模式下收听全屏更改的方法。例如，如果用户与应用互动，当系统 UI 返回时，开发人员现在可以编写代码以返回全屏或执行其他操作。

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/36da53110e6f729b70ca46c2513c4d02756dce0c66e0620f11fd5b5c3cf699c8.png)

_新的 Android 边到边模式：普通模式（左）、边到边模式（中）、带有自定义 SystemUIOverlayStyle 的边到边（右）_ 在此版本中，我们继续构建对新 Material You（又名 v3）规范的支持，包括对浮动操作按钮大小和主题的更新（[#86441](https://github.com/flutter/flutter/pull/86441 "https://github.com/flutter/flutter/pull/86441")），以及 MaterialState.scrolledUnder 您可以使用示例代码中的示例代码查看的新状态公关 ( [#79999](https://github.com/flutter/flutter/pull/79999 "https://github.com/flutter/flutter/pull/79999") )。

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/94c3bc9574bd46421d51e81e8c1ebb24e4be13baf46b203b9b6f8efbedbb11b7.png)

_新材料您的 FAB 尺寸_

![01.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e14616e736c0c38bd2bead41927e4eb99b27dba52b339eb7aff0f11d9d28fdd6.png)

_新的 MaterialState.scrolledUnder 状态在起作用_ 当我们谈论滚动时，另一个改进是添加了滚动指标通知（[#85221](https://github.com/flutter/flutter/pull/85221 "https://github.com/flutter/flutter/pull/85221")、[#85499](https://github.com/flutter/flutter/pull/85499 "https://github.com/flutter/flutter/pull/85499")），即使用户没有滚动，它[也会](https://github.com/flutter/flutter/pull/85499 "https://github.com/flutter/flutter/pull/85499")提供可滚动区域的通知。例如，下面显示了滚动条根据 的基础大小适当地出现或消失 ListView：

![02.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/d33eecb82565e969cf7c23132dd5e4021260939f9da890ba195e943ee91b9bc3.png)

_新的滚动指标通知使滚动条无需滚动即可自动出现和消失_ 在这种情况下，您不必编写任何代码，但如果您想捕获[ScrollMetricNotification](https://master-api.flutter.dev/flutter/widgets/ScrollMetricsNotification-class.html "https://master-api.flutter.dev/flutter/widgets/ScrollMetricsNotification-class.html")更改，则可以。特别感谢社区贡献者[xu-baoolin](https://github.com/xu-baolin "https://github.com/xu-baolin")，他为此付出了努力并提出了一个很好的解决方案。 社区的另一个出色贡献是为 ScaffoldMessenger. 你可能还记得 ScaffoldMessenger 从 [Flutter 2.0 发布公告](https://medium.com/flutter/whats-new-in-flutter-2-0-fe8e95ecc65 "https://medium.com/flutter/whats-new-in-flutter-2-0-fe8e95ecc65")作为一个更强大的方式来显示 SnackBars 在屏幕的底部为用户提供通知。在 Flutter 2.5 中，您现在可以在脚手架的顶部添加一个 banner ，该 banner 会一直保持到用户关闭它为止。

![03.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2bb567197fc06a010901f197ec49ff419bc3e549f895591c6c1d3346fb1ae4ae.png)

您的应用程序可以通过调用以下 showMaterialBanner 方法来获得此行为 ScaffoldMessenger：

[banner](https://material.io/components/banners%23usage "https://material.io/components/banners#usage") 的[Material 指南规定](https://material.io/components/banners%23usage "https://material.io/components/banners#usage")您的应用一次只能显示一个，因此如果您的应用调用 showMaterialBanner 多次，ScaffoldMessenger 它将维护一个队列，显示每个新 banner ，因为前一个 banner 已被关闭。感谢[Calamity210](https://github.com/Calamity210 "https://github.com/Calamity210")对 Flutter 中的 Material 支持做出了如此出色的补充！ 在 Flutter 2.0 及其新的文本编辑功能的基础上进一步构建，例如文本选择枢轴点以及能够在处理后停止键盘事件的传播，在此版本中，我们添加了使文本编辑键盘快捷键可覆盖的功能（[#85381](https://github.com/flutter/flutter/pull/85381 "https://github.com/flutter/flutter/pull/85381")）。如果您希望**Ctrl-A**执行一些自定义操作而不是选择所有文本，您可以这样做。本[DefaultTextEditingShortcuts](https://github.com/flutter/flutter/blob/b524270af147847f64fa296cf6eed3633ffe683d/packages/flutter/lib/src/widgets/default_text_editing_shortcuts.dart%23L163 "https://github.com/flutter/flutter/blob/b524270af147847f64fa296cf6eed3633ffe683d/packages/flutter/lib/src/widgets/default_text_editing_shortcuts.dart#L163")类包含每个平台上受支持扑每键盘快捷键列表。如果您想覆盖任何内容，请使用 Flutter 的现有[Shortcuts](https://api.flutter.dev/flutter/widgets/Shortcuts-class.html "https://api.flutter.dev/flutter/widgets/Shortcuts-class.html")小部件将任何快捷方式重新映射到现有或自定义意图。您可以将该小部件放置在小部件树中要应用覆盖的任何位置。查看[API 参考](https://api.flutter.dev/flutter/widgets/DefaultTextEditingShortcuts-class.html "https://api.flutter.dev/flutter/widgets/DefaultTextEditingShortcuts-class.html")中的一些示例。

### 插件：相机、图像选择器和插件

另一个有很多改进[的插件](https://pub.dev/packages/camera "https://pub.dev/packages/camera")是[相机插件](https://pub.dev/packages/camera "https://pub.dev/packages/camera")：

- [3795](https://github.com/flutter/plugins/pull/3795 "https://github.com/flutter/plugins/pull/3795") [相机] android-rework 第 1 部分：支持 Android 相机功能的基类
- [3796](https://github.com/flutter/plugins/pull/3796 "https://github.com/flutter/plugins/pull/3796") [相机] android-rework 第 2 部分：Android 自动对焦功能
- [3797](https://github.com/flutter/plugins/pull/3797 "https://github.com/flutter/plugins/pull/3797") [camera] android-rework part 3：Android 曝光相关功能
- [3798](https://github.com/flutter/plugins/pull/3798 "https://github.com/flutter/plugins/pull/3798") [相机] android-rework 第 4 部分：Android 闪光和变焦功能
- [3799](https://github.com/flutter/plugins/pull/3799 "https://github.com/flutter/plugins/pull/3799") [相机] android-rework 第 5 部分：Android FPS 范围、分辨率和传感器方向功能
- [4039](https://github.com/flutter/plugins/pull/4039 "https://github.com/flutter/plugins/pull/4039") [相机] android-rework 第 6 部分：Android 曝光和焦点功能
- [4052](https://github.com/flutter/plugins/pull/4052 "https://github.com/flutter/plugins/pull/4052") [camera] android-rework part 7：Android 降噪功能
- [4054](https://github.com/flutter/plugins/pull/4054 "https://github.com/flutter/plugins/pull/4054") [相机] android-rework 第 8 部分：最终实现的支持模块
- [4010](https://github.com/flutter/plugins/pull/4010 "https://github.com/flutter/plugins/pull/4010") [camera] 在 iOS 上不触发平面设备方向
- [4158](https://github.com/flutter/plugins/pull/4158 "https://github.com/flutter/plugins/pull/4158") [相机] 修复坐标旋转以在 iOS 上设置焦点和曝光点
- [4197](https://github.com/flutter/plugins/pull/4197 "https://github.com/flutter/plugins/pull/4197") [相机] 修复相机预览并不总是在方向改变时重建
- [3992](https://github.com/flutter/plugins/pull/3992 "https://github.com/flutter/plugins/pull/3992") [camera] 设置不受支持的 FocusMode 时防止崩溃
- [4151](https://github.com/flutter/plugins/pull/4151 "https://github.com/flutter/plugins/pull/4151") [camera] 引入 camera_web 包

[image_picker 插件](https://pub.dev/packages/image_picker "https://pub.dev/packages/image_picker")也做了很多工作，专注于端到端的相机体验：

- [3898](https://github.com/flutter/plugins/pull/3898 "https://github.com/flutter/plugins/pull/3898") [image_picker] 图像选择器修复相机设备
- [3956](https://github.com/flutter/plugins/pull/3956 "https://github.com/flutter/plugins/pull/3956") [image_picker] 将相机捕获的存储位置更改为 Android 上的内部缓存，以符合新的 Google Play 存储要求
- [4001](https://github.com/flutter/plugins/pull/4001 "https://github.com/flutter/plugins/pull/4001") [image_picker] 删除了对相机权限的冗余请求
- [4019](https://github.com/flutter/plugins/pull/4019 "https://github.com/flutter/plugins/pull/4019") [image_picker] 当相机是源时修复旋转

这项工作改进了适用于 Android 的相机和 image_picker 插件的功能和稳健性。此外，您会注意到[摄像头插件](https://pub.dev/packages/camera_web "https://pub.dev/packages/camera_web")的早期版本可用于网络支持 ( [#4151](https://github.com/flutter/plugins/pull/4151 "https://github.com/flutter/plugins/pull/4151") )。此预览为在 Web 上查看相机预览、拍照、使用闪光灯和缩放控件提供基本支持。它目前不是[认可的插件](https://flutter.dev/docs/development/packages-and-plugins/developing-packages%23endorsed-federated-plugin "https://flutter.dev/docs/development/packages-and-plugins/developing-packages#endorsed-federated-plugin")，因此您需要[明确添加它](https://pub.dev/packages/camera_web/install "https://pub.dev/packages/camera_web/install")以在您的网络应用程序中使用。 最初的 Android 相机重写工作由[acoutts](https://github.com/acoutts "https://github.com/acoutts")贡献。相机和 image_picker 工作是由降落[基流](https://www.baseflow.com/open-source/flutter "https://www.baseflow.com/open-source/flutter")，一家咨询公司，专门从事 Flutter 和知名的[上 pub.dev 自己的包](https://pub.dev/publishers/baseflow.com/packages "https://pub.dev/publishers/baseflow.com/packages")。camera_web 的工作主要由总部位于美国的 Flutter 咨询公司[Very Good Ventures](https://verygood.ventures/ "https://verygood.ventures/")完成。非常感谢大家对 Flutter 社区的贡献！ 另一个有价值的社区贡献是 Flutter 社区组织，以[“plus”插件](https://plus.fluttercommunity.dev/ "https://plus.fluttercommunity.dev/")而闻名。在此版本的 Flutter 中，Flutter 团队的每个相应插件现在都带有一个类似[电池](https://pub.dev/packages/battery "https://pub.dev/packages/battery")的建议：

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/99ea75ff9f1090884b4a11d7112455e69bec96389d5ba6c9558a4699f9bde6ae.png)

此外，由于这些插件不再被积极维护，它们不再被标记为 Flutter 最喜欢的插件。如果您还没有这样做，我们建议您使用以下插件的 plus 版本：

![image.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c0f906fb95498f2fbe0627a586be246d842dd242515761adccfb86fbeb1b80ff.png)

### Flutter DevTools：性能、小部件检查器和润色

此版本的 Flutter 对 Flutter DevTools 进行了许多改进。首先也是最重要的是 DevTools 中增加的支持以利用引擎更新（[#26205](https://github.com/flutter/engine/pull/26205 "https://github.com/flutter/engine/pull/26205")、[#26233](https://github.com/flutter/engine/pull/26233 "https://github.com/flutter/engine/pull/26233")、[#26237](https://github.com/flutter/engine/pull/26237 "https://github.com/flutter/engine/pull/26237")、[#26970](https://github.com/flutter/engine/pull/26970 "https://github.com/flutter/engine/pull/26970")、[#27074](https://github.com/flutter/engine/pull/27074 "https://github.com/flutter/engine/pull/27074")、[#26617](https://github.com/flutter/engine/pull/26617 "https://github.com/flutter/engine/pull/26617")）。其中一组更新使 Flutter 能够更好地将跟踪事件与特定框架相关联，这有助于开发人员确定框架可能超出预算的原因。您可以在 DevTools Frames 图表中看到这一点，该图表已被重建为“实时”；框架在您的应用程序中呈现时填充在此图表中。从此图表中选择一个帧导航到该帧的时间线事件：

![04.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/94f16b23503dee50ec875f2e473255d99f0fd5ff629f5364d0f3c8968111992b.png)

Flutter 引擎现在还可以识别时间线中的着色器编译事件。Flutter DevTools 使用这些事件来帮助您诊断应用程序中的着色器编译卡顿。

![05.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/095d7cbe442e8c506b6e8757394d87689465ffb912a511faca9dd74d494d5947.png)

DevTools 检测由于着色器编译而丢失的帧 借助这项新功能，DevTools 会检测您何时因着色器编译丢失帧，以便您可以解决问题。要像第一次一样运行您的应用程序（在填充着色器缓存之前，就像为任何用户一样），请 flutter run 与--purge-persistent-cache 标志一起使用。这会清除缓存以确保您重现用户在“首次运行”或“重新打开”(iOS) 体验中看到的环境。此功能仍在开发中，因此请[提交](https://b.corp.google.com/issues/new%3Fcomponent%3D775375%26template%3D1369639 "https://b.corp.google.com/issues/new?component=775375&template=1369639")您发现的[问题](https://b.corp.google.com/issues/new%3Fcomponent%3D775375%26template%3D1369639 "https://b.corp.google.com/issues/new?component=775375&template=1369639")的问题，或者我们可以做出的任何改进以帮助调试着色器编译卡顿。 此外，当您跟踪应用程序中的 CPU 性能问题时，您可能会被来自 Dart 和 Flutter 库和/或引擎本机代码的分析数据淹没。如果您想关闭其中任何一个以专注于您自己的代码，您可以使用新的 CPU Profiler 功能 ( [#3236](https://github.com/flutter/devtools/pull/3236 "https://github.com/flutter/devtools/pull/3236") ) 来实现，该功能使您能够从任何这些来源中隐藏分析器信息。 ![06.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/18f169bfdc53e483e4979abc3a66fa03e5e4b65f4d7076824ca2db7d99a7d9dc.png) 对于您没有过滤掉的任何类别，它们现在已经进行了颜色编码（[#3310](https://github.com/flutter/devtools/pull/3310 "https://github.com/flutter/devtools/pull/3310")、[#3324](https://github.com/flutter/devtools/pull/3324 "https://github.com/flutter/devtools/pull/3324")），以便您可以轻松查看 CPU 帧图表的哪些部分来自系统的哪些部分。 ![07.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/046e0bb38d9e0e0581ddcf3e34d3e07a19b1b541baf76b7fd4bcb272efb92145.png) 彩色框架图，用于识别应用中的应用、原生、Dart 和 Flutter 代码活动 性能并不是您想要调试的唯一因素。此版本的 DevTools 附带了对 Widget Inspector 的更新，允许您将鼠标悬停在小部件上以评估对象、视图属性、小部件状态等。

![08.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2d23a82bce320f3b09c307bb2b9408f573ea3d67195dde996ae76be17780e5d6.png)

而且，当您选择一个小部件时，它会自动填充在新的小部件检查器控制台中，您可以在其中浏览小部件的属性。

![09.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/f10e8902465e874e4fe8feb4f6f45075a7ac61ffc6e8540081128fdd87c044a5.png)

在断点处暂停时，您还可以从控制台计算表达式。 除了新功能外，Widget Inspector 还进行了翻新。为了让 DevTools 成为了解和调试 Flutter 应用程序的更有用的目的地，我们与芬兰的一家创意技术机构[Codemate](https://codemate.com/ "https://codemate.com/")合作进行了一些更新。

![10.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/42d51a5854af7ab495df09be2a1ba3ec3555634b676786180c660682a136d9d3.png)

Flutter DevTools 优化了 UX 以提高易用性 在此屏幕截图中，您可以看到以下更改：

- **更好地传达调试切换按钮的作用**——这些按钮具有新图标、面向任务的标签，以及描述它们的作用和何时使用它们的丰富工具提示。每个工具提示进一步链接到该功能的详细文档。
- **更容易扫描和定位感兴趣的**小部件——Flutter 框架中常用的小部件现在在检查器左侧的小部件树视图中显示图标。它们根据类别进一步进行颜色编码。例如，布局小部件显示为蓝色，而内容小部件显示为绿色。此外，每个文本小部件现在显示其内容的预览。
- **对齐布局资源管理器和小部件树的配色方案**- 现在可以更轻松地从布局资源管理器和小部件树中识别相同的小部件。例如，下面屏幕截图中的“列”小部件位于布局浏览器中的蓝色背景上，并且在小部件树视图中具有蓝色图标。

我们很想[听听您对](https://github.com/flutter/devtools/issues "https://github.com/flutter/devtools/issues")由这些更新或我们可以做出的任何其他改进引起的任何问题[的想法](https://github.com/flutter/devtools/issues "https://github.com/flutter/devtools/issues")，以确保 DevTools 运行良好。而这些亮点仅仅是个开始。有关此版本 Flutter 的 DevTools 新功能的完整列表，请查看发行说明：

- [Flutter DevTools 2.3.2 发行说明](https://groups.google.com/g/flutter-announce/c/LSNbc2rKIjQ/m/7Y7cWgO2CQAJ "https://groups.google.com/g/flutter-announce/c/LSNbc2rKIjQ/m/7Y7cWgO2CQAJ")
- [Flutter DevTools 2.4.0 发行说明](https://groups.google.com/g/flutter-announce/c/qenYe5LuHb8/m/tkWcBCVaAQAJ "https://groups.google.com/g/flutter-announce/c/qenYe5LuHb8/m/tkWcBCVaAQAJ")
- [Flutter DevTools 2.6.0 发行说明](https://groups.google.com/g/flutter-announce/c/yBEZOWdV9nc/m/KCX3m2BpCAAJ "https://groups.google.com/g/flutter-announce/c/yBEZOWdV9nc/m/KCX3m2BpCAAJ")

### IntelliJ/Android Studio：集成测试、测试覆盖率和图标预览

Flutter 的 IntelliJ/Android Studio 插件在此版本中也进行了许多改进，首先是运行集成测试的能力 ( [#5459](https://github.com/flutter/flutter-intellij/pull/5459 "https://github.com/flutter/flutter-intellij/pull/5459") )。集成测试是在设备上运行的整个应用程序测试，位于 integration_test 目录中，并使用与 testWidgets()小部件单元测试相同的功能。

![11.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/68ef3cb8915b4c7445c3fcfc9830259d8d8fbae64d7761ceec4e7784b019a6c2.png)

在 IntelliJ/Android Studio 中集成测试您的 Flutter 应用程序 要将集成测试添加到您的项目，请[按照 flutter.dev 上的说明进行操作](https://flutter.dev/docs/testing/integration-tests "https://flutter.dev/docs/testing/integration-tests")。要将测试与 IntelliJ 或 Android Studio 连接，请添加启动集成测试的运行配置并连接设备以供测试使用。运行配置可以让你运行测试，包括设置断点、步进等。 此外，Flutter 最新的 IJ/AS 插件允许您查看单元测试和集成测试运行的覆盖率信息。您可以从“调试”按钮旁边的工具栏按钮访问它：

![12.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/363a259339319b0b59d14bd10cc4def785350af1b41cb90c57c478b66f941115.png)

覆盖信息在编辑器的装订线中使用红色和绿色条显示。在这个例子中，第 9-13 行被测试，但第 3 和 4 行没有被测试。

![13.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/931a916c00a58badcb0b79f4cae4ed06bc8761c84699e77dcec43cee2e04a6d3.png)

最新版本还包括预览来自 pub.dev 包中使用的图标的新功能，这些包是围绕 TrueType 字体文件（[#5504](https://github.com/flutter/flutter-intellij/pull/5504 "https://github.com/flutter/flutter-intellij/pull/5504")、[#5595](https://github.com/flutter/flutter-intellij/pull/5595 "https://github.com/flutter/flutter-intellij/pull/5595")、[#5677](https://github.com/flutter/flutter-intellij/pull/5677 "https://github.com/flutter/flutter-intellij/pull/5677")、[#5704](https://github.com/flutter/flutter-intellij/pull/5704 "https://github.com/flutter/flutter-intellij/pull/5704")）构建的，就像 Material 和 Cupertino 图标支持预览一样。

![14.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/d26f41074e494f4f2b2a6ae2995778e7753da146f392873f63c23a6b67a374b2.png)

IntelliJ/Android Studio 中的图标预览 要启用图标预览，您需要告诉插件您正在使用哪些软件包。插件设置/首选项页面中有一个新的文本字段：

![15.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/fc9e115b1ace04efb542703c28a16d48e6603ba3019c84b17e870d3d41922d30.png)

请注意，这适用于在类中定义为静态常量的图标，如屏幕截图中的示例代码所示。它不适用于表达式，例如 LineIcons.addressBook()or LineIcons.values['code']。如果您是不使用此功能的图标包的作者，请创建一个[问题](https://github.com/flutter/flutter-intellij/issues "https://github.com/flutter/flutter-intellij/issues")。 这是 Flutter 的 IntelliJ/Android Studio 插件的更多更新，您可以在发行说明中阅读：

- [Flutter IntelliJ 插件 M57 发布](https://groups.google.com/g/flutter-announce/c/nZPj0uIW3h4/m/2Xnx8KQtAwAJ "https://groups.google.com/g/flutter-announce/c/nZPj0uIW3h4/m/2Xnx8KQtAwAJ")
- [Flutter IntelliJ 插件 M58 发布](https://groups.google.com/g/flutter-announce/c/WJUH0m6cu-U/m/_n0RltLFAAAJ "https://groups.google.com/g/flutter-announce/c/WJUH0m6cu-U/m/_n0RltLFAAAJ")
- [Flutter IntelliJ 插件 M59 发布](https://groups.google.com/g/flutter-announce/c/CNzqxtybpBA/m/nSu7QabMAQAJ "https://groups.google.com/g/flutter-announce/c/CNzqxtybpBA/m/nSu7QabMAQAJ")
- [Flutter IntelliJ 插件 M60 发布](https://groups.google.com/g/flutter-announce/c/qc40yulxvAg/m/B_1HuGmoBQAJ "https://groups.google.com/g/flutter-announce/c/qc40yulxvAg/m/B_1HuGmoBQAJ")

### Visual Studio Code：依赖项、Fix All 和 Test Runner

Flutter 的 Visual Studio Code 插件也在此版本中得到了改进，从两个新命令“Dart：添加依赖项”和“Dart：添加开发依赖项”（[#3306](https://github.com/Dart-Code/Dart-Code/issues/3306 "https://github.com/Dart-Code/Dart-Code/issues/3306")，[#3474](https://github.com/Dart-Code/Dart-Code/issues/3474 "https://github.com/Dart-Code/Dart-Code/issues/3474")）开始。

![16.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c7c3d562c2181881140ae82dfecb8c2e2193d36f50b81188e46ffcc05a8bba35.png)

在 Visual Studio Code 中添加 Dart 依赖项 这些命令提供的功能类似于[Jeroen Meijer 的 Pubspec Assist 插件](https://marketplace.visualstudio.com/items%3FitemName%3Djeroen-meijer.pubspec-assist "https://marketplace.visualstudio.com/items?itemName=jeroen-meijer.pubspec-assist")已经提供了一段时间。这些新命令开箱即用，并提供定期从 pub.dev 获取的包类型过滤列表。 您可能还对适用于 Dart 文件的“全部修复”命令（[#3445](https://github.com/Dart-Code/Dart-Code/issues/3445 "https://github.com/Dart-Code/Dart-Code/issues/3445")、[#3469](https://github.com/Dart-Code/Dart-Code/issues/3469 "https://github.com/Dart-Code/Dart-Code/issues/3469")）感兴趣，并且可以一步修复所有与[dart fix](https://dart.dev/tools/dart-fix "https://dart.dev/tools/dart-fix")相同的问题。

![17.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/d9110d9bc4f2d74715a4f40eac5f6a2244059d246dfd6e15e8d21048741655e7.png)

使用 Flutter Fix 规则修复代码中的所有已知问题 这也可以通过添加 source.fixAll 到 editor.codeActionsOnSaveVS Code 设置来设置为在保存时运行。 或者，如果您想尝试预览功能，您可以启用该 dart.previewVsCodeTestRunner 设置并查看通过新的 Visual Studio Code 测试运行程序运行的 Dart 和 Flutter 测试。

![18.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/6d804b6919e9cdb3e635587d2886793723c36f9fd6d71f95e96d04b57afb9ea9.png)

使用新的 Visual Studio Code 测试运行程序测试您的 Dart 和 Flutter 代码 Visual Studio Code 测试运行器看起来与当前的 Dart 和 Flutter 测试运行器略有不同，它将跨会话保留结果。Visual Studio Code 测试运行器还添加了新的装订线图标，显示测试的最后状态，可以单击以运行测试（或右键单击以获取上下文菜单）。

![19.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/30e066bfd5b36f3c59a1ab8e115c2c45b83e1cc9310bb885cb0875c3bd2f8368.png)

在即将发布的版本中，现有的 Dart 和 Flutter 测试运行器将被移除，以支持新的 Visual Studio Code 测试运行器。 这只是 Visual Studio Code 新功能和修复的冰山一角。有关所有详细信息，请查看发行说明：

- [v3.26](https://dartcode.org/releases/v3-26/ "https://dartcode.org/releases/v3-26/") VS Code Test Runner 集成，Flutter 创建设置，...
- [v3.25](https://dartcode.org/releases/v3-25/ "https://dartcode.org/releases/v3-25/")额外的依赖管理改进，修复所有文件/保存时，......
- [v3.24](https://dartcode.org/releases/v3-24/ "https://dartcode.org/releases/v3-24/")依赖树改进，更容易启动配置，编辑器改进
- [v3.23](https://dartcode.org/releases/v3-23/ "https://dartcode.org/releases/v3-23/") Profile Mode 改进，改进的依赖关系树，LSP 改进

### 工具：例外、新应用模板和 Pigeon 1.0

在之前的 Flutter 版本中，您可能对期望未处理的异常感到沮丧，因此您可以触发调试器并找出它们的来源，结果却发现 Flutter 框架没有让异常通过以触发“未处理的调试器中的 expectation”处理程序。在此版本中，调试器现在可以在未处理的异常上正确中断，而这些异常以前刚刚被框架捕获 ( [#17007](https://github.com/flutter/flutter/issues/17007 "https://github.com/flutter/flutter/issues/17007") )。这改善了调试体验，因为您的调试器现在可以将您直接指向他们代码中的抛出行，而不是指向框架深处的随机行。一个相关的新功能使您能够决定 FutureBuilder 是否应该重新抛出或吞下错误 (# [84308](https://github.com/flutter/flutter/pull/84308 "https://github.com/flutter/flutter/pull/84308")）。这应该会为您提供大量额外的例外情况，以帮助您追踪 Flutter 应用程序中的问题。 自 Flutter 诞生以来，就出现了 Counter 应用模板，它具有许多优点：它展示了 Dart 语言的许多特性，展示了几个关键的 Flutter 概念，并且它足够小，可以放入单个文件中，即使有很多的解释性评论。然而，它没有为现实世界的 Flutter 应用程序提供一个特别好的起点。在此版本中，通过以下命令提供了一个新模板 ( [#83530](https://github.com/flutter/flutter/pull/83530 "https://github.com/flutter/flutter/pull/83530") )： $ flutter create -t skeleton my*app ![20.gif](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/5ca62ed8f16e6168c93f5c21ecbd49f811d7882a99eba2f318069996e42e6887.png) *新的 Flutter 骨架模板在起作用\_ 骨架模板生成一个遵循社区最佳实践的两页列表视图 Flutter 应用程序（带有详细信息视图）。它的开发经过大量内部和外部审查，为构建生产质量应用程序提供了更好的基础，并支持以下功能：

- 用于 ChangeNotifier 协调多个小部件
- 默认情况下使用 arb 文件生成本地化
- 包括示例图像并为图像资产建立 1x、2x 和 3x 文件夹
- 使用“功能优先”的文件夹组织
- 支持共享首选项
- 支持明暗主题
- 支持多页面间导航

随着时间的推移，随着 Flutter 最佳实践的发展，预计这个新模板也会随之发展。 另一方面，如果您正在开发插件而不是应用程序，那么您可能会对 Pigeon 的 1.0 版本感兴趣。Pigeon 是一个代码生成工具，用于在 Flutter 及其主机平台之间生成类型安全的互操作代码。它允许您定义插件 API 的描述，并为 Dart、Java 和 Objective-C（分别可用于 Kotlin 和 Swift）生成框架代码。

![21.png](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/616578393e56c2f5bf2d9fdf68aa5ca0c5e292dc1602a142247fd1acadfcd4be.png)

示例生成的 Pigeon 代码 Flutter 团队的一些插件中已经使用了 Pigeon。在此版本中，它提供了更多有用的错误消息，增加了对泛型、原始数据类型作为参数和返回类型以及多个参数的支持，预计将来会更频繁地使用它。如果您想在自己的插件或添加到应用程序项目中利用 Pigeon，可以在[Pigeon 插件页面](https://pub.dev/packages/pigeon "https://pub.dev/packages/pigeon")找到更多信息。

### 重大更改和弃用

以下是 Flutter 2.5 版本中的重大变化：

- [默认拖动滚动设备](https://flutter.dev/docs/release/breaking-changes/default-scroll-behavior-drag "https://flutter.dev/docs/release/breaking-changes/default-scroll-behavior-drag")
- [在 v2.2 之后删除了弃用的 API](https://flutter.dev/docs/release/breaking-changes/2-2-deprecations "https://flutter.dev/docs/release/breaking-changes/2-2-deprecations")
- [引入包：flutter_lints](https://flutter.dev/docs/release/breaking-changes/flutter-lints-package "https://flutter.dev/docs/release/breaking-changes/flutter-lints-package")
- [ThemeData 的重音属性已被弃用](https://flutter.dev/docs/release/breaking-changes/theme-data-accent-properties "https://flutter.dev/docs/release/breaking-changes/theme-data-accent-properties")
- [手势识别器清理](https://flutter.dev/docs/release/breaking-changes/gesture-recognizer-add-allowed-pointer "https://flutter.dev/docs/release/breaking-changes/gesture-recognizer-add-allowed-pointer")
- [用 collate 替换 AnimationSheetBuilder.display](https://flutter.dev/docs/release/breaking-changes/animation-sheet-builder-display "https://flutter.dev/docs/release/breaking-changes/animation-sheet-builder-display")
- [使用 HTML 插槽在 Web 中呈现平台视图](https://flutter.dev/docs/release/breaking-changes/platform-views-using-html-slots-web "https://flutter.dev/docs/release/breaking-changes/platform-views-using-html-slots-web")
- [将 LogicalKeySet 迁移到 SingleActivator](https://github.com/flutter/flutter/pull/80756 "https://github.com/flutter/flutter/pull/80756")

有关自 1.17 版本以来重大更改的完整列表，[请参阅 flutter.dev](https://flutter.dev/docs/release/breaking-changes "https://flutter.dev/docs/release/breaking-changes")。 随着我们继续更新 Flutter Fix（在您的 IDE 中和通过 dart fix 命令可用），我们总共有 157 条规则来自动迁移受这些或过去的重大更改以及任何弃用影响的代码。一如既往，非常感谢社区[贡献测试](https://github.com/flutter/tests/blob/master/README.md "https://github.com/flutter/tests/blob/master/README.md")，他们帮助我们识别这些重大变化。要了解更多信息，请查看[我们的重大变更政策](https://github.com/flutter/flutter/wiki/Tree-hygiene%23handling-breaking-changes "https://github.com/flutter/flutter/wiki/Tree-hygiene#handling-breaking-changes")。 此外，随着 Flutter 2.5 的发布，我们将弃用[2020 年 9 月宣布的](http%3A//flutter.dev/go/rfc-ios8-deprecation "http://flutter.dev/go/rfc-ios8-deprecation")对 iOS 8 的支持。放弃对市场份额不到 1% 的 iOS 8 的支持，使 Flutter 团队能够专注于更广泛使用的新平台。弃用意味着这些平台可以工作，但我们不会在这些平台上测试 Flutter 的新版本或插件。您可以[在 flutter.dev 上查看当前支持的 Flutter 平台列表](https://flutter.dev/docs/development/tools/sdk/release-notes/supported-platforms "https://flutter.dev/docs/development/tools/sdk/release-notes/supported-platforms")。

### 概括

最后，一如既往地感谢世界各地的 Flutter 社区让这一切成为可能。对于在此更新中贡献和审查了 1000 个 PR 的数百名开发人员，这里是您每项努力的成果。我们正在共同努力为世界各地的开发人员转变应用程序开发流程，以便您可以从单个代码库中交付更多、更快、部署到您关心的平台。 请继续关注 Google Flutter 团队的更多更新。今年还没有结束！

> [原文链接： What’s new in Flutter 2.5](https://medium.com/flutter/whats-new-in-flutter-2-5-6f080c3f3dc "https://medium.com/flutter/whats-new-in-flutter-2-5-6f080c3f3dc")

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
