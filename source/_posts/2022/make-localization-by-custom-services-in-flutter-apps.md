---
title: 在 Flutter 应用程序中通过定制服务进行本地化
tags: flutter
categories: 译文
date: 2022-02-15 00:00:00
---

![](2022-02-15-22-26-00.png)

## 前言

猫哥在项目中也是有这个困惑，如何管理 多语言，如果常量定义方式，还是有点不优雅。

这篇文章的作者就提出了个不错的建议，服务端编辑，本地文件缓存同步。

就是读取数据库啦，后端弄个界面维护维护数据，貌似是一个不错的方案。

## 原文

https://medium.com/flutter-community/make-localization-by-custom-services-in-flutter-apps-519b5de32aae

## 代码

https://github.com/VB10/custom_localization

## 参考

- https://pub.dev/packages/easy_localization

## 正文

许多应用程序在客户端进行本地化。这意味着，如果您的密钥有任何问题，您必须同时更新字符串和应用程序。让我们学习自定义解决方案，当用户将改变语言，我们的应用程序将直接更新每个屏幕的新语言。

正如我所说的，通常我们使用 JSON 文件等的本地化。我写了许多 Flutter 项目，所以我过去使本地化为我们的客户，一般来说，我做了一个土耳其和英语版本的产品由我写的。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/54df0eabf27d91ede48b2972c6c1049752c5fdbe5c48b7f00b432fd5e75ac439.png)

通常我们像这样定义这些键，然后应用程序用键读取值。

Flutter 有许多本地化选项，特别是我在生产项目中多次使用了 [easy_localizaiton](https://pub.dev/packages/easy_localization)。这对我的所有要求都适用，但有一天，客户需要特殊的解决方案。我已经直接要求,

- 你知道我们有一个 location ，你还指望什么？
- 是的，Veli 我们知道这一点，但是我们想要管理本地化文件在我们的 web 服务和客户端可以更改语言到我们的本地化列表响应。

实际上，这个请求并不是什么大问题，因为我们可以使用远程配置、 Firebase 实时配置或者简单的本地化都有自定义的后端解决方案，但是任何人都不能满足这些要求。该做甜甜圈了！

如果我们想要更好的编码时间，我们必须制定计划。首先，两个应用程序的 API 端点是什么，以获得我们支持的语言列表，应用程序将通过查看语言键带来翻译值。当应用程序用户更改应用程序中的语言时，我们必须更改整个字符串值，而不是旧值。

### 我们的步骤:

- 应用程序和应用程序更改语言的后端端点可以是显示语言
- 我们将实现初始语言
- 我们可以根据自动接收到的新语言信息改变整个文本的一个点。
- 用户可以找到当前的语言值。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/0bf31cfbd44cbe0b31d43981dfa1669c0265cd21b5abedc8e10ca125716bab46.png)

我用 Firebase 创建了简单的后端资源，例如:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/f07d061a61297f4d8fc3bbd072d6319e794f45f16d478464e71f14d671b11ac2.png)

通常我有一个定制的后端，比如 node.js，Java 等等，或者我可以选择 Firebase 云函数，但是我只是想展示如何使用 flutter 特性定制本地化，所以我们没有使用这个方法。

让我们开始编码吧！

我们知道如何用后端端点加载语言资源。之后，我想缓存它的后端响应，因为客户端需要每次重新启动的全部数据。因此，我们需要缓存机制，以保持后端响应，而用户不更改语言。

我已经选择了 [Hive](https://pub.dev/packages/hive) 解决方案使用缓存的应用程序，因此 Hive 非常快速，安全，和有用的解决方案为我们。我为我们的许多企业项目选择了 Hive。我将在这个项目中使用这个库:

- Hive - <https://pub.dev/packages/hive> (Cache)
- Vexana - <https://pub.dev/packages/vexana> (Network)
- Provider - <https://pub.dev/packages/provider> (Global State)

- 首先，我们需要示例屏幕来显示初始语言的键值(现在我接受在更改为 tr 之后开始 en while 应用程序)。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/9876ac962608a6523191f566154173405a69235e44f087afb5c45d8644eb3be5.png)

我在下面的图片中添加了 tr 和 en 的语言值。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/ae38fb8c521b61a667d85d5e23a0f0eb61b19e742063a6ef8db3f3e07ad83ca7.png)

现在，我将实现一个服务层来获取列表语言结果和相关键值。我总是倾向于为下面的实例服务调用的每个业务操作提供接口。

```dart
abstract class ILangugaeService {
  final INetworkManager networkManager;
  ILangugaeService(this.networkManager);
  Future<List<String>?> fetchLangageList();
  Future<Map<String, dynamic>?> fetchSpesificResources(String key);
}
```

在我尝试为控件后端响应实现一个特性测试之后，我编写了这个接口，它是正确的，因此我想为我们的业务验证这个响应。

当您调用这个测试服务测试文件时，您将看到每个操作都是正确的。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/662ff037133028c83cf2d5164773f056ae5bbcdd00648e87e8a5394a664a4d2f.png)

- 测试功能结果

是时候实现后端部分了。在包括 to title 和 body widget 之后，我将调用 en 的资源列表。完成后，我们完成了第一个目标

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/32aea7758934048407d71a300889a4a3781cf26a464a8ab78a41fa7c624d79c8.png)

- 初始资源

现在，我们连接到了后端服务，如果更改为语言参数，我们可以自动显示新的资源。现在，我们需要什么？如果我们有太多的页面，我们希望动态地更改整个键。让我们把这个建筑。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/9e7e480474c2e4a3db3854f82febade222802f1a447fee54b2d73f422fb40394.png)

最后，我实现了这个小工具。它由语言列表响应填充。如果您想要添加一种新的语言，您只需在使用完成后在数据库中添加一个资源键即可。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c64b5f3a67728b9ca848f1031fa195fdf5b76b1d7467bd1cd3890b584a137fd9.png)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/29afd097bce847950ac01b59e86946035e06023783a5aab8e5059f4a7fe7e326.png)

### 语言管理

我将创建一个用于管理运营的 manager 类。它需要通知一个应用程序的每一个变化。对钥匙的其他要求。我计划为键字符串设置语言扩展类。这个扩展将能够监听语言的变化。

它改变了我们的本地化资源，然后通过提供程序管理通知整个模块。我更新了每个语言更改请求的初始资源键。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/fb38a8ea828468e92eb7541a401d6763cea0820585037b6b2703ffc1fe99ee3b.png)

这是准备使用，但我如何使用我的钥匙。是的，我们谈到需要一个字符串变化的扩展。首先，我们需要学习如何根据当前视图实现这个管理器。

```dart
Text(context.watch<LanguageManager>().resources?[LocaleKeys.hello.name] ?? '')
```

也就是说，每次更改都会得到字符串响应，但我不使用这个，因为它太长而且不能控制。当然，我们如何才能为此编写一个更好的解决方案？

我将替换 home 视图中的文本小部件，例如:

```dart
ListTile(title: Text(LocaleKeys.ageQuestion.tr(context))),
```

> 您可以选择不使用 context-param 的其他状态管理方法。那是你的选择，我给建议应该用这个。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e1fd87dd965497423eb937c8fe333c5ff6b30547bde1416ed8265ef520ce8894.gif)

来自后端响应的运行时更改

现在，我们已经准备好使用，但是我们需要最后的改进缓存，因此我们将通过缓存获得性能和能力。我们的要求已经够多了。[Hive](https://docs.hivedb.dev/#/) 它既是一个快速又安全的键值数据库。

完成配置单元后，我将向数据库中添加后端响应，以便使用脱机或应用程序启动。通常我们应该获取语言响应，直到用户可以手动更改语言或者获取数据到具有任何数据的应用程序。

我创建了 IHive 抽象类来管理我的 hive 类。我做了一个两个盒子，第一个管理我的钥匙，另一个保存语言列表。

在我更新了我的语言管理器之后。我添加了两个新的参数 IHive `<map>` 和 IHive `<list>` ，用于保存语言环境数据。

```dart
LanguageManager(
  LanguageService(ProductNetworkManager()),
  LanguageHive(),
  LanguageListHive(),
  ),
```

我开始控制那些有缓存数据的应用程序。如果应用程序有任何缓存数据，它将在第一次启动时更新管理器资源。用户在更新语言环境时，我将进入在 Hive 中响应以便再次重用。

> 我在 map 数组中保持选择的语言，但是通常如果你想保持其他语言，你可以为新的键打开一个新的框。它将提供不再下载其他语言，因为您的旧框保留在应用程序，而您删除的 Hive。

还有一点，当应用程序启动时，我将在检查缓存中的数据之后，首先进行初始化。

我们差不多准备好了! 让我们看看结果如何。

> 其他提示，您可以使用流构建器监听配置更改，但是我不使用流创建屏幕，也许您可以创建一个基类并监听配置更改，而不需要任何状态管理。

Thank you for reading 🍀
Have a good hacking, both in real life and coding life

感谢您阅读 🍀

《Have a good hacking, both in real life and coding life》

### GitHub 代码

https://github.com/VB10/custom_localization

end

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
