---
title: Flutter 添加APP启动 Story View
tags: flutter
categories: 译文
date: 2021-05-05 00:00:00
---

![](2021-05-05-08-51-54.png)

## 原文

> https://medium.com/flutterdevs/story-view-in-flutter-7bb4ae98b119

## 前言

![](2021-05-05-08-29-22.png)

在当前的快速市场中，一些社交渠道已经全面爆发，成为各个年龄段聚会的热门话题。漫步在数字环境中，你会注意到新的网络媒体应用程序，比如 Instagram，在过去的一年里热得像火一样。

当你听到“基于网络的媒体应用”这个词时，可能会出现 Facebook、 Instagram、 Twitter 或 Linkedin 等应用程序。然而，你有没有考虑过如何在 Instagram 这样的在线媒体应用程序上显示一个故事？在线媒体应用程序是一个开放的集会，您可以通过一个简单的用户界面与来自世界各地的个人进行联系。

在这个博客中，我们将探 Story View In Flutter 。我们将实现一个故事视图演示程序，以及如何在您的颤动应用程序中使用故事视图包创建类似 WhatsApp 的故事。

## 类库

- https://pub.dev/packages/story_view/install

## 本文源码

https://github.com/flutter-devs/flutter_story_view_demo

## 正文

### Flutter Story View

Story View Flutter 组件工具对 Flutter 开发者很有帮助，通过使用这个类库，你可以显示社交媒体故事页面非常像 WhatsApp 状态故事或 Instagram 状态故事视图。同样可以像 Google 新闻应用程序一样使用内联/内部 ListView 或者 Column。伴随着手势暂停，向前，并进入后面的页面。

![](2021-05-05-08-34-07.png)

这个演示视频显示了如何创建一个 Flutter 的故事视图。它展示了如何在您的 Flutter 应用程序中使用故事视图包来工作。它可以像文本、图片、视频等一样显示你的故事。此外，用户将转发，先前，和手势暂停的情景。它会显示在你的设备上。

### Features 功能

Story View 的一些特性如下:

- 简单的文本状态故事
- 图像、 GIF 图像故事和视频故事(启用缓存)
- 为上一个、下一个和暂停故事做手势
- 每个故事项的标题
- 在每个故事视图的顶部有一个动画的进度指示器

### Properties 属性

Story View 的一些属性是:

- controller: 此属性用于控制 Story 的回放
- onComplete: 此属性用于在显示 Story 的整个周期时进行回调。每当故事完成时，当 repeat 设置为 true 时，就会调用这个函数
- storyItems: 此属性不为空，不显示页
- onVerticalSwipeComplete: 此属性用于检测到垂直滑动手势时的回调。如果您不想收听这样的事件，请不要提供它
- onStoryShow: 此属性用于当前显示故事时的回调
- progressPosition: 此属性用于应放置进度指示符的位置

### 集成步骤

- 第一步: 添加依赖项

将依赖项添加到 pubspec ー yaml 文件。

```yaml
dependencies:
  flutter:
    sdk: flutter
  story_view: ^0.12.3
```

- 第二步: 导入

```dart
import 'package:story_view/story_view.dart';
```

- 第三步: 拉取包

```shell
> flutter packages get
```

### 代码实现

你需要分别在你的代码中实现它:

> 在 lib 文件夹中创建一个名为 `status_screen.dart` 的新 dart 文件。

在这个屏幕上，我们将创建一个类似 WhatsApp 的用户界面。我们将添加一个容器小部件。在内部，我们将向 ListTile 添加网络图像、文本和 onTap 函数自动换行。在这个函数中，我们将导航到 `StoryPageView()` 类。

```dart
Container(
  height: 80,
  padding: const EdgeInsets.all(8.0),
  color: textfieldColor,
  child: ListView(
    children: <Widget>[
      ListTile(
        leading: CircleAvatar(
          radius: 30,
          backgroundImage: NetworkImage(
              "https://images.unsplash.com/photo-1581803118522-7b72a50f7e9f?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8bWFufGVufDB8fDB8fA%3D%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60"),
        ),
        title: Text(
          "Logan Veawer",
          style: TextStyle(fontWeight: FontWeight.bold,color: white ),
        ),
        subtitle: Text("Today, 20:16 PM",style: TextStyle(color:white.withOpacity(0.5)),),
        onTap: () => Navigator.push(
            context,
            MaterialPageRoute(
                builder: (context) => StoryPageView())),
      ),
    ],
  ),
),
```

当用户按下容器时，就会显示一个故事页面。我们将深入讨论下面的代码。当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-05-05-08-43-31.png)

> 在 lib 文件夹中创建一个名为 `story_page_view.dart` 的新 dart 文件。

首先，我们将创建一个与 StoryController() 相等的 final_controller。

```dart
final _controller = StoryController();
```

我们将创建一个 storyItems 列表。首先，我们将添加 StoryItem.text 意味着只添加不同背景颜色的简单文本状态。其次，我们将添加 StoryItem.pageImage 的意思是用控制器添加图像的 URL 来控制故事。最后，我们将使用控制器和图像匹配添加 gif 视频的 URL。

```dart
final List<StoryItem> storyItems = [
  StoryItem.text(title: '''“When you talk, you are only repeating something you know.
   But if you listen, you may learn something new.”
   – Dalai Lama''',
      backgroundColor: Colors.blueGrey),
  StoryItem.pageImage(
      url:
          "https://images.unsplash.com/photo-1553531384-cc64ac80f931?ixid=MnwxMjA3fDF8MHxzZWFyY2h8MXx8bW91bnRhaW58ZW58MHx8MHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60",
      controller: controller),
  StoryItem.pageImage(
      url:
          "https://wp-modula.com/wp-content/uploads/2018/12/gifgif.gif",
      controller: controller,
      imageFit: BoxFit.contain),
];
```

我们将返回一个 Material() 方法。在这个方法中，我们将添加 StoryView()。在内部，我们将添加一个 storyItems、 controller、 inline means 列表，如果您希望将故事显示为整个页面，则将其设置为 false 。但是，如果您要将它作为页面的一部分(如 ListView 或 Column)显示，那么将其设置为 true。我们会添加重复意味着用户应该故事永远重复然后真实，否则，假。

```dart
return Material(
  child: StoryView(
    storyItems: storyItems,
    controller: controller,
    inline: false,
    repeat: true,
  ),
);
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-05-05-08-46-23.png)

### 代码文件

```dart
import 'package:flutter/material.dart';
import 'package:story_view/story_view.dart';

class StoryPageView extends StatefulWidget {
  @override
  _StoryPageViewState createState() => _StoryPageViewState();
}

class _StoryPageViewState extends State<StoryPageView> {
  final controller = StoryController();

  @override
  Widget build(BuildContext context) {
    final List<StoryItem> storyItems = [
      StoryItem.text(title: '''“When you talk, you are only repeating something you know.
       But if you listen, you may learn something new.”
       – Dalai Lama''',
          backgroundColor: Colors.blueGrey),
      StoryItem.pageImage(
          url:
              "https://images.unsplash.com/photo-1553531384-cc64ac80f931?ixid=MnwxMjA3fDF8MHxzZWFyY2h8MXx8bW91bnRhaW58ZW58MHx8MHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60",
          controller: controller),
      StoryItem.pageImage(
          url:
              "https://wp-modula.com/wp-content/uploads/2018/12/gifgif.gif",
          controller: controller,
          imageFit: BoxFit.contain),
    ];
    return Material(
      child: StoryView(
        storyItems: storyItems,
        controller: controller,
        inline: false,
        repeat: true,
      ),
    );
  }
}
```

### 总结

在本文中，我已经解释了 Flutter 的基本结构的 Story View ; 您可以根据自己的选择修改这个代码。这是一个小的介绍 Story View 的用户交互从我这边，它的工作使用 Flutter。

## 老铁记得 点赞、转发 ，我将更有动力呈现 Flutter 好文~~~~

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
