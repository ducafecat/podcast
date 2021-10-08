---
title: Flutter 网络搜索引擎SEO优化友好
tags: flutter
categories: 译文
date: 2021-10-08 00:00:00
---

![](2021-10-08-09-12-55.png)

## 原文

> https://medium.com/mindful-engineering/flutter-web-seo-friendly-317528c29cc6

## 参考

- https://pub.dev/packages/seo_renderer

## 正文

目前，在开发移动和网络应用程序时， Flutter 将成为一种流行趋势。我们都知道桌面版本正处于 beta 测试阶段，并且在 Flutter web 的稳定版本中被淘汰。我们都知道，当互联网出现在图片中时，目标受众就会大得多(世界范围内)。我们的网站不能很容易地到达用户只要输入和搜索到一个搜索引擎，我们得到的结果。当网站是为企业和其他人建立的时候，索引很重要。

当我们希望通过增加搜索索引来建议用户选择我们的应用时，搜索引擎优化是最重要的。

是否可能与 Flutter 网络搜索引擎优化？

这个问题在开发网络应用程序之前或之后出现在我的脑海中。对 web 应用程序更好的 SEO 支持是 Flutter 开发者下一个版本的目标。

### 这里是我的网站后，使其 SEO 友好的结果

> 以前:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/3fc6ee1c8ed974d221da7c420ee72df15fd018b1dc457ae20a02688402cc7231.png)

> 之后:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c3f48665321964fc2853b79ac8d63e26d4c93ccfeab0527517a5fe4e22c046c2.png)

### 搜索引擎优化

> 搜索引擎优化搜索引擎是一个过程，它可以提高网站流量的质量和数量，使网站或网页从搜索引擎获得流量。搜索引擎优化的目标是无偿流量，而不是直接流量或付费流量。

### 什么是 seo 友好型？

建立一个 seo 友好的网站意味着谷歌和其他搜索引擎可以高效地抓取网站上的每个页面，有效地解释内容，并将其索引到数据库中。一旦编入索引，他们就可以根据用户搜索的主题向用户提供最相关、最有价值的网页。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c4cdbd00299df2065d5451175132203e651bdb343ef6e12cde62863df9d7aab6.com/max/1400/0*mypesmxL7_SlPmwa)

> 我遵循的步骤，使网站 SEO 友好

- 标题的长度至少应该是 207 个字符
- 描述长度最少 690 个字符将是有益的
- 需要在 index.html 中添加关键字 meta（关键字应根据页面内容正确，并且需要添加至少 10 个关键字以存档良好的 SEO）
- 需要增加一个移动优化的观点
- 包装每个文本、图像、链接*[\*\*\_seo_render 感谢上帝*\*\*](https://pub.dev/packages/seo_renderer) 包将有助于使网站 SEO 友好。它是用来渲染文本，链接，图像小部件作为 HTML 元素 SEO 的目的。(仍在开发中)\_
- 语义小工具也可以帮助使网站 seo 友好

### 适用于 seo 的 Meta 标签

> 开放图表标签

```dart
og:title - The title of your object as it should appear within the graph, e.g., "The Rock".

og:type - The type of your object, e.g., "video.movie". Depending on the type you specify, other properties may also be required.

og:image - An image URL which should represent your object within the graph

.og:url - The canonical URL of your object that will be used as its permanent ID in the graph, e.g., "https://www.imdb.com/title/tt0117500/".
```

在 meta 标签中有许多可用的属性，你可以在这里找到 [here](https://ogp.me/)

### 使用 seo 渲染库渲染文本小部件作为 HTML 元素

- 首先，我们需要添加一个`RouteObserver` 从导航堆栈中弹出时自动删除 Html 元素`MaterialApp`

```dart
navigatorObservers: <RouteObserver<ModalRoute<void>>[routeObserver],
```

- 将这个依赖项添加到`pubspec.yaml`

`pubspec.yaml`

- 从 Pub 取包裹

```dart
flutter packages get
```

- 所有的文本，图片和链接都应该像这样被扭曲

`home_page.dart`

- 只要传递你的 `Text`/`RichText` 小工具和一个选项 `RenderController()` 可用于在可滚动内容/更改位置的情况下刷新内容(位置)
- 需要通过 `child : Widget`, `anchorText : String`, `link : String` & 可选的 `RenderConroller()`
- 需要通过 `child : Widget`, `link : String`, `alt : String` & 可选的 `RenderConroller()`

### 语义学

这个小部件使用小部件的含义描述对小部件树进行注释。

可访问性工具、搜索引擎和其他语义分析软件使用它来确定应用程序的含义。

这种语义将为移动环境中的可访问性服务提供信息。

- **Semantics 语义学** 当你只想描述一个特定的小部件
- **MergeSemantics** 当你想要描述一组组件的时候。在这个例子中，不同的*Semantics 语义学* 在这个节点的子树中定义的**one single Semantics 一个单一的语义学**。这可能是非常有用的重新组语义，但是，在语义冲突的情况下，结果可能是荒谬的

### 语义的属性:

在语义学中有许多可用的属性，比如,

- `label`: 它提供了一个小部件的文本描述，是基本的语义信息
- `container`: 此节点是否在语义树中引入新的语义节点(SemanticsNode)。它不能用上层语义，即独立语义进行分离或合并
- `explicitChildNodes` : 默认值为 false，表示是否强制显示子窗口小部件的语义信息。它可以理解为分裂语义学
- `scopesRoute`: 如果它不是空的，则无论节点是否对应于子树的根，子树都应该声明路由名称。通常`explicitChildNodes`将其设置为 true，并在路由跳转中使用它，例如`Page jumps`,`Dialog`, `BottomSheet`, `PopupMenu` 弹出部分

这里显示了语义的一些属性,

`semantics_demo.dart`

> 如果希望调试应用程序的语义，可以将 MaterialApp 的 showSemanticsDebugger 属性设置为 true。这将迫使 Flutter 生成一个覆盖可视化的语义树。

### 搜索引擎渲染器 | Flutter Package

https://pub.dev/packages/seo_renderer

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
