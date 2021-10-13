---
title: flutter theme 主题样式生成工具
tags: flutter
categories: 译文
date: 2021-10-13 00:00:00
---

![](2021-10-13-09-08-14.png)

## 原文

> https://medium.com/@sheikhg1900/using-flutter-theming-tools-generated-dart-material-theme-in-your-application-f190f3919e88

## 参考

- https://medium.com/@sheikhg1900/using-flutter-theming-tools-generated-dart-material-theme-in-your-application-f190f3919e88

## 正文

在你的 android 手机上打开 [Flutter 主题工具应用程序](https://play.google.com/store/apps/details?id=com.tamata.soft.flutter.theming.tool) 。按照指南为你的应用程序准备一个很棒的 Dart 主题。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/5b9aae4782b6617fcc4783b5c12e1d106b8a346fd25dab058fe544f65ba7e178.jpeg)

将生成的 Dart 主题代码复制到剪贴板中。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a87494bb83a049dbfef268640a156b66565308e322a7681ce6fe6af0a3020c92.jpeg)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/b2dc345a80425c8d0edd41678da3d060585aa91b587cfb958c584b22636bb420.jpeg)

要在您的计算机上获取主题，请在 IDE 中，_\(例如 Visual Studio Code\)_。将其粘贴到您手机上的 _slack_ 聊天中，以便您可以从计算机上的 _slack_ 获取代码。在移动设备 _slack_ 上，输入 \`\`\`。将出现一个框。将剪贴板内容粘贴到该框中。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/3c2867a86ad4b1aa5c91eac1889bd4a78c5adcaaf74cb99f4fb8594c1e8991b8.jpeg)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/13dca8a32f82844a26004d1d5058e7df537d66dbdd5709beeda5f74614b661d0.jpeg)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/b0dcaf0a5758c766f65093dfac35798867e5fe720c0a785c2c02c46b7520e3e2.jpeg)

可选：按照相同的步骤为黑暗模式生成另一个 _Dart_ 主题。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/efb0b4a1b8be826a3685da0fbee91f9c394aadf020052dc9d4e01ad37e560d74.jpeg)

打开您现有的 _flutter_ 项目。使用以下内容创建 `generated_theme.dart` 文件。

```dart
import 'package:flutter/material.dart';ThemeData get mylightTheme {// TODO: Copy Generated Light Theme Here.
return theme;
}ThemeData get myDarkTheme {// TODO: Copy Generated Dark Theme Here.
return theme;
}
```

用生成的代码替换 TODO 注释。

```dart
ThemeData get mylightTheme {
// Flutter Theming Tool 1.0.0+10, developed by Tamata Soft
// Initialize ThemeData.
  var theme = ThemeData(
    primarySwatch: Colors.blue,
    brightness: Brightness.light,
  );// Main Setting.
  theme = theme.copyWith(
    colorScheme: theme.colorScheme.copyWith(
      onPrimary: const Color(0xffffffff),
      secondary: Colors.deepOrange,
    ),
  );// ElevatedButton Setting.
  theme = theme.copyWith(
    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ButtonStyle(
        shape: MaterialStateProperty.all(
          const RoundedRectangleBorder(
            borderRadius: BorderRadius.only(
              topLeft: Radius.circular(16.0),
              topRight: Radius.circular(16.0),
            ),
          ),
        ),
      ),
    ),
  );// OutlinedButton Setting.
  theme = theme.copyWith(
    outlinedButtonTheme: OutlinedButtonThemeData(
      style: ButtonStyle(
        shape: MaterialStateProperty.all(
          const RoundedRectangleBorder(
            borderRadius: BorderRadius.only(
              topLeft: Radius.circular(16.0),
              topRight: Radius.circular(16.0),
            ),
          ),
        ),
      ),
    ),
  );// Chip Setting.
  theme = theme.copyWith(
    chipTheme: theme.chipTheme.copyWith(
      shape: const RoundedRectangleBorder(
        borderRadius: BorderRadius.only(
          topLeft: Radius.circular(16.0),
          bottomRight: Radius.circular(16.0),
        ),
      ),
      labelStyle: (theme.chipTheme.labelStyle).copyWith(
        color: Colors.deepOrange,
        shadows: [
          const Shadow(
            blurRadius: 2.0,
            color: Colors.grey,
          )
        ],
      ),
      secondaryLabelStyle: (theme.chipTheme.labelStyle).copyWith(
        shadows: [
          const Shadow(
            blurRadius: 2.0,
          )
        ],
      ),
    ),
  );
  return theme;
}
```

打开 `main.dart` 文件。在 `MaterialApp` 小部件中添加 `theme` 属性。

```dart
MaterialApp(
 title: 'Flutter Demo',
 theme: mylightTheme,
 ----
 ----
)
```

# **所需的包.**

`google_fonts`

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
