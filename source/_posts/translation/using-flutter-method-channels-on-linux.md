---
title: 基于 Linux 的 Flutter 方法通道 Channels
tags: flutter
categories: 译文
date: 2021-09-24 00:00:00
---

![](2021-09-24-09-05-26.png)

## 原文

> https://medium.com/flutter-community/using-flutter-method-channels-on-linux-2f6ae3e99653

## 代码

https://github.com/charafau/linux_method_channel

## 参考

- https://flutter.dev/docs/development/platform-integration/platform-channels
- https://engine.chinmaygarde.com/fl__value_8cc.html
- https://engine.chinmaygarde.com/fl__value_8h.html#a1b2a2f35eb78370fbb2d1af774f6d245
- https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools
- https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools

## 正文

# Flutter Method Channels on Linux

# 基于 Linux 的 Flutter 方法通道

又见面了！今天我将继续我的 Flutter Linux 之旅，我们将再次接触 Linux 集成。上次我们设法为插件开发设置了 Visual Studio Code，今天我们将进一步研究 Method Channels。

如果你想创建一个带有方法通道支持的插件，非常简单，只需要使用 `flutter create \-t plugin \--platforms=linux <name of your project>` 从模板生成一个 Flutter 项目。但是，如果你不想创建一个单独的插件，只是添加一些自定义代码到 Flutter Linux 应用程序中呢？我发现这并不简单，所以我想我会写这篇文章，这样你就不用自己解决了。

为了不再拖延下去，让我们开始吧。

# 创建方法通道

首先，我们需要创建编解码器 `codec` ，`binary messenger` 二进制信使和信道。接下来，我们将向自定义方法分配一个方法调用，我们将在下一步中创建这个方法。

为此，让我们打开我的 `my_application.cc` 。在 `linux` 文件夹中抄送并导航到我的 `my_application_activate` 函数。接下来，我们在插件初始化之后实现上面描述的对象。

在下面的示例中，`name_of_our_channel` 的名称是我们从 Dart 代码调用的方法。

# 回调函数

现在我们来创建一个回调函数:

非常简单，我们传递一个通道、 methodcall 和一些用户数据。

To check for the channel’s method name we need to use the `fl_method_call_get_name` function on `method_call` object. And compare it with `strcmp` like so:

为了检查通道的方法名，我们需要在 `method_call` 对象上使用 `fl_method_call_get_name` 函数。然后把它和 `strcmp` 比较，就像这样:

# 方法未实现响应

如果传递给 channel 的方法不存在，我们需要返回未实现的结果来做这件事，我们需要调用 `fl_method_call_respond`

# 错误处理

在开始讨论参数和自定义结果之前，让我们先快速了解一下错误处理。

幸运的是，它与未实现的方法非常相似:

我们可以看到它几乎是相同的，我们只是用 `fl_method_error_response_new` 创建结果，而不是用 `fl_method_not_implemented_response_new`。

# 获取 Dart 参数

好了，现在是时候从头开始写我们想写的东西了。让我们假设我们想要从 Dart 发送数据到 c + + ，为了做到这一点，我们只需要从 Dart 端发送一个地图，但是如何获取呢？

为此，我们需要调用 `fl_method_call_get_args(FlMethodCall)` ，它返回一个指向 `FlValue` 的指针。

接下来我们检查返回的值是否是正确的类型:

在上面的例子中，我们查找字符串，但是还有其他的，比如 int、 float、 bool、 map。对于完整的清单检验 [Flutter Engine Documentation](https://engine.chinmaygarde.com/fl__value_8cc.html)

类型检查也是一样，检查枚举的整个列表到 [Documentation](https://engine.chinmaygarde.com/fl__value_8h.html#ae514203eea95fec354c1648957f77a4d)

# 返回一些值

我们几乎完成了，我们设法处理了未实现的方法、错误和从方法中获取参数。剩下的是返回 Dart 的值。

对我们来说幸运的是，这与我们已经完成的工作非常相似，我们只需要创建 `FlMethodResponse` 并将 `FlValue` 放入其中。下面是一个例子:

像前面一样，这里有更多价值创造函数的文档链接 [link to documentation for more value creation function](https://engine.chinmaygarde.com/fl__value_8h.html#a1b2a2f35eb78370fbb2d1af774f6d245)

# 代码完成 + 调试

我想我应该给你一些奖励，因为你来到这里，所以我决定在 Visual Studio Code 中编写代码完成和调试的设置程序。

在开始之前，必须安装 [C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) 和 [Cmake](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools) 插件。

首先让我们设置代码完成。创建名为 `c_cpp_properties.json` 的文件。Json 在里面。在你的项目的根目录下放一个 `.vscode` 文件夹，然后把这个配置文件放进去:

检查编译器路径\(Flutter 使用 Clang\)并根据需要调整 c/c + + 标准。

为了设置调试，我们需要在 `launch.json` 中创建启动配置 `.vscode` 文件夹。让我们来看一下配置:

非常简单，但是需要更改二进制名称。此外，要知道，使它的工作，你需要建立您的 Flutter 项目与 `flutter run`。

# 本文结束

谢谢你的阅读，希望你会发现它很有用。

编程愉快！

完整的例子可以在这里找到。

https://github.com/charafau/linux_method_channel

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
