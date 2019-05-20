---
title: Flutter移动开发 - 01 前言
date: 2019-01-17 10:25:02
tags: flutter
categories: Flutter移动开发
---

![](2019-01-22-16-13-18.png)

## 本节目标

- 介绍 Flutter
- 课程计划
- 如果获取课程资料、代码、视频

## 环境

- Flutter 1.0 stable

## 适合人群

- 泛移动开发人员
- 原生移动开发人员
- 前端开发人员

## Flutter 框架结构

![](2019-01-22-16-21-03.png)

- Flutter Framework

Framework 这一层使用 Dart 语言开发，它实现了一套基础库。

Foundation、Animation、Painting、Gestures 为 Dart 实现的 UI 层，提供动画、手势及绘制。

Rendering 渲染层，依赖 UI 层，在运行时 Rendering 层会构建一个 Widget 树，当有变化时，会更具一定的算法计算出有变化的部分，然后更新 Widget 树。

Widgets 层是 Flutter 提供的的一套基础组件库，在基础组件库之上，Flutter 还提供了 Material 和 Cupertino 两种视觉风格的组件库。

- Flutter Engine

Skia 是一个开源的二维图形库，提供各种常用的 API，并可在多种软硬件平台上运行。谷歌 Chrome 浏览器、Chrome OS、安卓、火狐浏览器、火狐操作系统以及其它许多产品都使用它作为图形引擎。

Skia 由谷歌出资管理，任何人都可基于 BSD 免费软件许可证使用 Skia。Skia 开发团队致力于开发其核心部分， 并广泛采纳各方对于 Skia 的开源贡献。

因为没有使用原生的 UI 和绘制框架，所以才保证了 Flutter 的高性能体验。

## Flutter 特点

- 多平台支持 iOS Android Linux 未来 windows Fuchsia

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

- [官方仓库](https://pub.dartlang.org/flutter/)
- [awesome](https://github.com/Solido/awesome-flutter)

## 课程计划

```md
- 前言
- 环境配置
  - MacOS
  - Windows
- 快速感受
  - Hello Word!
  - 组件 Widgets
  - 路由 Route
  - 第三方包 pub
  - 资源 assets
  - 调试 debug
- 基础组件
  - 组件总览
  - 文本、字体、样式
  - 按钮
  - 图片
  - Icon
  - 输入框
  - 单选、复选框
  - 表单
- 布局组件
- 容器组件
- 滚动组件
- 功能组件
- 事件通知
- 动画
- 自定义组件
- 文件操作
- 网络操作
- 插件开发
- 国际化
```

## 参考

- [flutter.io](https://flutter.io)
- [skia](https://skia.org/index_zh)
- [showcase](https://flutter.io/showcase)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
