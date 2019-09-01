---
title: Flutter 零基础入门中文教学 - 07 我们的第一个程序 hello word
date: 2019-08-12 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- 程序基础结构
- pubspec.yaml 配置
- 布局，样式使用

![](2019-09-01-10-58-22.png)

## 目录文件结构

| 名称         | 说明             |
| ------------ | ---------------- |
| lib          | Flutter 代码     |
| android      | Android 项目     |
| ios          | IOS 项目         |
| test         | 测试目录         |
| .idea        | IDEA 编辑器配置  |
| pubspec.yaml | Flutter 配置文件 |
| pubspec.lock | 包版本锁定       |
| build        | 编译目录         |

## 一、编写最基础 helloword

- 步骤

```sh
1. 第一步 runApp(...)
2. 第二步 MaterialApp(...)
3. 第三步 指定 widget Text(...)
```

- 代码

```dart
import 'package:flutter/material.dart';

main(List<String> args) {
  // 第一步 runApp(...)
  runApp(
      // 第二步 MaterialApp
      MaterialApp(
    // 第三步 指定 widget
    home: Text('hello word!'),
  ));
}

```

## 二、采用界面脚手架 标题 侧栏 正文

```dart
main(List<String> args) {
  // 第一步 runApp(...)
  runApp(
      // 第二步 MaterialApp
      MaterialApp(
    // 第三步 指定 widget
    home:
        // 第四步 页面脚手架
        Scaffold(
      // 第五步 程序标题
      appBar: AppBar(
        title: Text('我们第一个程序'),
      ),
      // 第六步 侧栏
      drawer: Drawer(
        child: Text('侧栏'),
      ),
      // 正文
      body: Text('hello word!'),
    ),
  ));
}
```

## 三、布局 样式 图片

```dart
main(List<String> args) {
  // 第一步 runApp(...)
  runApp(
      // 第二步 MaterialApp
      MaterialApp(
    // 第三步 指定 widget
    home:
        // 第四步 页面脚手架
        Scaffold(
      // 第五步 程序标题
      appBar: AppBar(
        title: Text('我们第一个程序'),
      ),
      // 第六步 侧栏
      drawer: Drawer(
        child: Text('侧栏'),
      ),
      // 正文
      body:
          // 居中
          Center(
        child: Column(
          children: <Widget>[
            // 载入图片
            Image.asset('assets/p1.jpg'),
            // 文字
            Text(
              '雷神',
              // 样式
              style: TextStyle(fontSize: 28, color: Colors.red),
            ),
          ],
        ),
      ),
    ),
  ));
}
```

## 代码

https://github.com/ducafecat/flutter-learn/tree/master/helloword

## 参考

- [Write your first Flutter app, part 1](https://flutter.dev/docs/get-started/codelab)
- [widgets](https://flutter.dev/docs/development/ui/widgets)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
