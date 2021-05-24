---
title: 使用 Flutter WEB 实现桌面 GUI 第1部分 介绍
tags: flutter
categories: 译文
date: 2021-05-24 00:00:00
---

![](2021-05-24-10-17-04.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://achraf-feydi.medium.com/desktop-gui-implementation-using-flutter-web-part-1-introduction-42d21a6e7937

## 演示

https://www.fluttergui.com/

## 代码

https://github.com/achreffaidi/FlutterGUI

## 参考

- https://pub.dev/packages/flutter_treeview
- https://pub.dev/packages/reorderables
- https://pub.dev/packages/video_player
- https://pub.dev/packages/flutter_simple_calculator
- https://pub.dev/packages/photo_view
- https://pub.dev/packages/maze
- https://pub.dev/packages/flutter_widget_from_html_core
- https://pub.dev/packages/native_pdf_view
- https://pub.dev/packages/painter

## 正文

### Why FlutterGUI?

作为 flutter2 的一部分，flutter 已经宣布，flutter 的网络支持已达到稳定的里程碑。

这不仅意味着我终于可以停止编写 HTML 和 CSS 代码，而且我现在可以拥有一个可以在几乎所有流行平台上运行的应用程序。

我对 Flutter Web 的稳定版本并没有太多期待，因为我已经尝试了 Beta 版，而且大多数插件都没有足够的支持。但是回到两周前，我对目前支持 WEB 的插件的数量感到惊讶。

这促使我去尝试一些有挑战性的东西，创建一个桌面图形用户界面使用颤振网络作为我的新的投资组合网站。

坦率地说，这个项目本身并不是什么有用的东西，它不是在解决问题，而且很可能也不是我下一个十亿美元的想法。但是，这是最好的方式来发现的优势和局限性颤振网络使用在一个网络项目。说实话，我想我至少要花两个月的时间来发布第一个版本。

在这个项目上花了两个星期的时间，每晚工作两个小时左右，我最终得到了一些真正值得出版的东西。

尽管我没有阅读任何文档，也没有像平时那样频繁地用谷歌搜索东西，但我还是对这种简单流畅的体验感到惊讶。从使用 Flutter 手机开发到网络的转变真的很简单，而且我在这个项目中学到的关于网络开发的知识并不需要使用任何东西。

### Technical overview

- Project 现在有 8 个应用程序运行在它里面，正如你已经猜到的: 它是所有的小部件。

- 大多数应用程序是现有的颤振插件，包装在我创建的通用应用部件内部。

- 这个码头是从零开始建造的，因为我找不到一个能满足我需要的现有码头。

- 项目没有后端，Flutter Web 应用程序托管在 Github 页面上。

- 我正在使用 Firebase 分析跟踪用户的互动与应用程序。

### Apps

- 文件管理器: 用 flutter_treeview 颤振树景 and 及 reorderables 可调整的 plugins. 插件

https://pub.dev/packages/flutter_treeview

https://pub.dev/packages/reorderables

![](2021-05-24-10-08-28.png)

- 视频播放器: 使用 video_player 视频播放器 plugin. 插件

https://pub.dev/packages/video_player

![](2021-05-24-10-08-55.png)

- 计算器: 使用 flutter_simple_calculator 简单计算器 plugin. 插件

https://pub.dev/packages/flutter_simple_calculator

![](2021-05-24-10-09-18.png)

- 照片: 使用 photo_view 照片查看 plugin. 插件

https://pub.dev/packages/photo_view

![](2021-05-24-10-09-43.png)

- 游戏: 使用 Maze 梅兹 plugin. 插件

https://pub.dev/packages/maze

![](2021-05-24-10-10-03.png)

- 阅读器: 使用 flutter_widget_from_html_core 从 html 核心 plugin. 插件

https://pub.dev/packages/flutter_widget_from_html_core

![](2021-05-24-10-10-36.png)

- PDF 阅读器: 使用 native_pdf_view 本地 pdf 查看 plugin. 插件

https://pub.dev/packages/native_pdf_view

![](2021-05-24-10-11-00.png)

- 画家: 使用 painter 画家 plugin. 插件

https://pub.dev/packages/painter

![](2021-05-24-10-11-31.png)

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
