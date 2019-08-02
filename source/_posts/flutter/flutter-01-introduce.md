---
title: Flutter 零基础入门中文教学 - 01 前言
date: 2019-06-17 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

![](2019-01-22-16-13-18.png)

## 本节目标

- 介绍 Flutter
- 课程计划
- 如果获取课程资料、代码、视频

## 适合人群

- 泛移动开发人员
- 原生移动开发人员
- 前端开发人员

## 跨平台: 移动、Web、桌面、嵌入

![跨平台](image-20190522135901818.png)

## Flutter 框架结构

![](image-20190523091712106.png)

- Flutter Framework

Framework 这一层使用 Dart 语言开发，它实现了一套基础库。

Foundation、Animation、Painting、Gestures 为 Dart 实现的 UI 层，提供动画、手势及绘制。

Rendering 渲染层，依赖 UI 层，在运行时 Rendering 层会构建一个 Widget 树，当有变化时，会更具一定的算法计算出有变化的部分，然后更新 Widget 树。

Widgets 层是 Flutter 提供的的一套基础组件库，在基础组件库之上，Flutter 还提供了 Material 和 Cupertino 两种视觉风格的组件库。

- Flutter Engine

Skia 是一个开源的二维图形库，提供各种常用的 API，并可在多种软硬件平台上运行。谷歌 Chrome 浏览器、Chrome OS、安卓、火狐浏览器、火狐操作系统以及其它许多产品都使用它作为图形引擎。

Skia 由谷歌出资管理，任何人都可基于 BSD 免费软件许可证使用 Skia。Skia 开发团队致力于开发其核心部分， 并广泛采纳各方对于 Skia 的开源贡献。

因为没有使用原生的 UI 和绘制框架，所以才保证了 Flutter 的高性能体验。



## Skia

[官网](https://skia.org/)

![image-20190626154959148](image-20190626154959148.png)

Skia是一个开源的二维图形库，提供各种常用的API，并可在多种软硬件平台上运行。谷歌Chrome浏览器、Chrome OS、安卓、火狐浏览器、火狐操作系统以及其它许多产品都使用它作为图形引擎。

Skia由谷歌出资管理，任何人都可基于BSD免费软件许可证使用Skia。Skia开发团队致力于开发其核心部分， 并广泛采纳各方对于Skia的开源贡献。

## Flutter for Web

[https://flutter.dev/web](https://flutter.dev/web)

![](image-20190523092032825.png)

通过对比，可以发现，web框架层和mobile的几乎一模一样。因此只需要重新实现一下引擎和嵌入层，不用变动Flutter API就可以完全可以将UI代码从Android / IOS Flutter App移植到Web。Dart能够使用Dart2Js编译器把Dart代码编译成Js代码。大多数原生App元素能够通过DOM实现，DOM实现不了的元素可以通过Canvas来实现。

## Fuchsia OS

- [许中兴博士演讲：Fuchsia OS 简介及幻灯片下载](https://xuzhongxing.github.io/201806fuchsia.pdf) 

- 桌面系统

![](image-20190626153104569.png)

- 手机OS

![](image-20190626153252442.png)

* 平板

![](image-20190626153929267.png)

- 华为荣耀Play

![](image-20190626154621682.png)

![](image-20190626154700792.png)

## Flutter 特点

- 多平台支持 iOS Android Linux 未来 Fuchsia OS

- 原生用户界面

- 120fps 超高性能

- 两套成熟 UI 库 Material Design 和 Cupertino

- 响应式的框架 Redux、RxDart、BloC 业务和 UI 分离

- Flutter 支持 Hot Reload

- 国内阿里咸鱼、腾讯、京东、国外的谷歌等公司都已经有上线产品在使用 Flutter 开发
  - [showcase](https://flutter.io/showcase)

## Flutter 横向对比

- Cordova

  基于 WebView 渲染，遇到动画、大列表 性能慢

- React Native、Weex

  基于虚拟 DOM 生成原生组件，比 Cordova 这类的性能好，但是遇到负责项目会有叠加 view 过多性能瓶颈

- Flutter

  自己封装的组件和渲染引擎，在设计上肯定会比 RN 这类的性能好，用的自家 Dart 语言深度编译，不需要像 RN 桥接 JavaScript 进行通讯，也会在性能上有优势

## Flutter 生态资源

- [Flutter应用展示](https://itsallwidgets.com/)
- [官方包管理平台](https://pub.dev/flutter)
- [awesome](https://github.com/Solido/awesome-flutter)

## 课程计划

```md
- 开篇写在最前
  - Flutter 介绍
  - 学习方法推荐
  - 课程计划

- 开发环境搭建和工具配置
  - Flutter 环境配置 MacOS
  - IOS 环境配置 MacOS
  - Android 环境配置 MacOS
  - Flutter 环境配置 Windows10
  - Android 环境配置 Windows10
  - 开发工具的选择 VSCode、IDEA、AndroidStudio

- 实战应用
  - 新闻应用程序结构分析
  - 使用布局组件搭建新闻列表界面
  - 采用第三方 Http Dio 程序包读取数据
  - 解析 Json 到 Modle
  - 使用自定义组件展示新闻行信息
  - 点击新闻条路由导航到详情页
  - 采用Web容器展示新闻内容
  - 上拉刷新、下拉加载新闻列表
  - 定制 Loading 效果
  - 采用矢量图标库
  - 采用 Sqlite 实现新闻列表首页缓存
  - Redux 管理主题样式
  - 加入应用启动画面
  - 打包 Android APK 文件
  - 打包 IOS IPA 文件

- 基础知识
  - Route 路由导航
  - 包管理
  - 资源管理
  - 调试工具
  - 线程模型和异常处理
  - 生命周期

- 基础组件
  - Widget 与 Element
  - StatelessWidget
  - StatefulWidget
  - Text
  - Image
  - Button
  - AppBar
  - AlertDialog
  - Icon
  - TextField
  - Form
  - Switch
  - Checkbox

- 布局组件
  - 线性 Row
  - 线性 Column
  - 弹性 Flex
  - 弹性 Expanded
  - 层叠 Stack
  - 层叠 IndexedStack
  - 层叠 Positioned
  - 流式 Flow
  - 流式 Wrap

- 容器组件
  - Scaffold
  - Container
  - Center
  - Padding
  - ConstrainedBox
  - SizedBox
  - DecoratedBox
  - Transform

- 导航组件
  - TabBar
  - NavigationBar
  - PageView

- 可滚动组件
  - CustomScrollView
  - ListView
  - GridView
  - ScrollView
  - ExpansionPanel
  - ScrollController

- 表格组件
  - Table
  - DataTables

- 功能型组件
  - WillPopScope
  - InheritedWidget
  - 主题 Theme

- 事件处理与通知
  - 事件处理
  - 手势识别
  - 全局事件总线
  - 通知消息

- 自定义 Widget
  - 组合其它 Widget
  - 自绘 CustomPaint、Canvas

- 进阶
  - 文件操作
  - Http 请求
  - WebSocket 连接
  - Json 解析
  - 包与插件
  - 国际化
  - 数据库缓存
  - Redux

```

## 参考

- [flutter.io](https://flutter.io)
- [skia](https://skia.org/index_zh)
- [showcase](https://flutter.io/showcase)
- [An open list of apps built with Flutter](https://itsallwidgets.com/)
- [[Flutter: a Portable UI Framework for Mobile, Web, Embedded, and Desktop](https://developers.googleblog.com/2019/05/Flutter-io19.html)](https://developers.googleblog.com/2019/05/Flutter-io19.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

