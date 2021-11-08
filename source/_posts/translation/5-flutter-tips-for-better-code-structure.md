---
title: Flutter 6 个建议改善你的代码结构
tags: flutter
categories: 译文
date: 2021-11-08 06:00:00
---

![](2021-11-08-09-08-13.png)

## 原文

> https://itnext.io/5-flutter-tips-for-better-code-structure-fa514845a903

## 正文

### 1. 将 init 操作与 main 函数分离，使其更加清晰

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/17f29923c63d6bc5cafd2fcb4d014f22ac644c1bd7f3a6b33dbde46e2c2df246.jpeg)

### 2. 你可以简单地管理这样的 GetX 路由，而不需要任何麻烦

- 用法

```dart
Get.toNamed(Routes.login);
```

- 怎么做

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/abf3d7c9fa5bae0fc4c126fef40399cb36428e0e2f61f24d77c8f3fc63ba4fb8.jpeg)

### 3. 你也可以在一个地方管理你的样式风格

- 用法

```dart
// S stands for Styles

S.colors.red
S.textStyles.f10Medium
S.shadows.softShadow
```

- 怎么做

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/8052ac6c00fa66bab1792da34f70c368fa04cfb9b47476773ce09095bcf260af.jpeg)

### 4. 像 boss 一样管理你的资源

- 用法

```dart
// R stands for Resources// Animations

R.anims.loading// SVG images
R.icons.logo// `mages
R.images.
```

- 怎么做

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/b3744156525bb5799b83489072f787aeb9af669fd27f37cf9f698d764243e0c1.jpeg)

### 5. 集中管理你的常量

- 用法

```dart
// C stands for Constants

C.title
C.names
C.descp
```

- 怎么做

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/305368bedb914b36fe539700bfb785a81e16c7533bb2216a93bd4c15256a3b51.jpeg)

### 6. 建立你的工具类 utils，我们在一个地方使用所有的时间

- 用法

```dart
Utils.formatDate(date,locale);

Utils.formatters.onlyTwoDecimalDigits;

Utils.show.dialog();
```

- 怎么做

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2a020521789efd0e26693130fc5ae9b8e48a3dfd33f237c594acc16d845b4a33.png)

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
