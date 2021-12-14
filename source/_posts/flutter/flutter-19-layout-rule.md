---
title: Flutter 零基础入门中文教学 - 19 布局规则 紧约束、松约束、无边界
tags: flutter
categories: Flutter零基础入门中文教学
date: 2021-11-24 00:00:00
---

## 目标

- 掌握 Flutter 布局规则
- 掌握常见布局方式
- 学会调试技巧

## 视频

## 参考

- https://docs.flutter.dev/development/ui/layout/constraints
- https://flutter.cn/docs/development/ui/layout/constraints
- https://github.com/marcglasberg/flutter_layout_article
- https://docs.flutter.dev/development/ui/layout/box-constraints
- https://flutter.cn/docs/development/ui/layout/box-constraints

## 正文

![](layout.png)

![](2021-11-24-23-10-10.png)

### 1. 名称解释

#### 1.1 紧约束 tight 、松约束 loose

> 官方解释
>
> A tight constraint offers a single possibility, an exact size. In other words, a tight constraint has its maximum width equal to its minimum width; and has its maximum height equal to its minimum height.
>
> A loose constraint, on the other hand, sets the maximum width and height, but lets the widget be as small as it wants. In other words, a loose constraint has a minimum width and height both equal to zero:

- 紧约束 tight

它的最大/最小宽度是一致的，高度也一样。

- 松约束 loose

最小宽度/高度为 0

- 同时是紧约束、松约束

如果最大最小都是 0

#### 有边界 bounded、无边界 unbounded

大部分组件都是有界 bounded

有些组件假装自己没有边界 unbounded ，如 listView row column 这种

### 2. 布局原则

#### 2.1 组件大小与位置

- 只有一个组件的时候占满全屏

https://dartpad.dev/926a55893c9284a85831f857b6004ccf

<iframe src="https://dartpad.dev/926a55893c9284a85831f857b6004ccf" height="500px" width="100%"></iframe>

- 子元素不指定位置，填充父元素尺寸

https://dartpad.dev/75eaf5a64d96e35fa887946bb8f6185b

<iframe src="https://dartpad.dev/75eaf5a64d96e35fa887946bb8f6185b" height="500px" width="100%"></iframe>

- Scaffold body 默认左上对齐

https://dartpad.dev/802b6a69388b2c881361be26c9eb2736

<iframe src="https://dartpad.dev/802b6a69388b2c881361be26c9eb2736" height="500px" width="100%"></iframe>

#### 2.2 约束盒子、紧约束、松约束

- 向下传递约束，向上传递尺寸

https://dartpad.dev/a49dd136dedda0f3ba06d59f00901d0d

<iframe src="https://dartpad.dev/a49dd136dedda0f3ba06d59f00901d0d" height="500px" width="100%"></iframe>

- 通过 BoxConstraints 向下传递约束
- BoxConstraints.loosen 松约束
- BoxConstraints.tightFor 紧约束

https://dartpad.dev/5d495e039d84aa4789960f5fd4763700

<iframe src="https://dartpad.dev/5d495e039d84aa4789960f5fd4763700" height="500px" width="100%"></iframe>

- 通过 SizeBox 向下传递约束
- SizedBox.expand 最大尺寸

https://dartpad.dev/9c6a5bd2a751727b4318a1c9db4f417a

<iframe src="https://dartpad.dev/9c6a5bd2a751727b4318a1c9db4f417a" height="500px" width="100%"></iframe>

#### 2.3 Column Row ListView 都是无边界 unbounded 组件

- Row Column 大小由子元素最大尺寸决定, 主轴方向 unbounded 不限制约束

https://dartpad.dev/2ccab9b37a26b91efb87c249ecff81f8

<iframe src="https://dartpad.dev/2ccab9b37a26b91efb87c249ecff81f8" height="500px" width="100%"></iframe>

- Expand Flex 按比例调整尺寸

https://dartpad.dev/2b83cb698dc732d1a60ddd107b2a6152

<iframe src="https://dartpad.dev/2b83cb698dc732d1a60ddd107b2a6152" height="500px" width="100%"></iframe>

#### 2.4 Stack 布局规则

- 最大尺寸匹配最大的子元素

https://dartpad.dev/918da13543e31dc570576cd24be7cbcc

<iframe src="https://dartpad.dev/918da13543e31dc570576cd24be7cbcc" height="500px" width="100%"></iframe>

- 元素分为有位置、无位置
- 最大尺寸优先匹配有位置元素
- 当只有一个 Positioned 时，Stack 将会占满整个父元素
- 通过 fit: StackFit.passthrough 属性可以调整父组件传递过来的约束规则

https://dartpad.dev/1f39b5557f8cd693327de6a1d4efb6fb

<iframe src="https://dartpad.dev/1f39b5557f8cd693327de6a1d4efb6fb" height="500px" width="100%"></iframe>

- 元素溢出裁剪 clipBehavior: Clip.hardEdge
- 元素溢出点击事件无效

https://dartpad.dev/0cc1ab3e157775b4f9633105d9bf32fb

<iframe src="https://dartpad.dev/0cc1ab3e157775b4f9633105d9bf32fb" height="500px" width="100%"></iframe>

## 总结

尽量让你的布局代码精简适配布局原则，而不是写了很繁琐，越复杂约容易出错，当然业务层面还是需要去拆分成业务组件的各种组合。

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
