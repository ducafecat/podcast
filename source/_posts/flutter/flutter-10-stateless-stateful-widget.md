---
title: Flutter 零基础入门中文教学 - 10 stateless stateful 有状态、无状态组件
date: 2019-08-18 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- stateless、stateless 差别
- 动手封装两个 widget 来体验

![点击切换](2019-09-01-11-25-01.png)

![点击切换](2019-09-01-11-25-15.png)

## 安装插件

Awesome Flutter Snippets

![](2019-09-01-11-27-15.png)

## 第一步：编写 statefull 主程序

```dart
import 'package:flutter/material.dart';

main(List<String> args) {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  MyApp({Key key}) : super(key: key);

  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Text('data'),
      ),
    );
  }
}
```

## 第二步：编写 stateless 图片显示

```dart
import 'package:flutter/material.dart';

class MyPicView extends StatefulWidget {
  final String picName;
  MyPicView({Key key, this.picName}) : super(key: key);

  _MyPicViewState createState() => _MyPicViewState();
}

class _MyPicViewState extends State<MyPicView> {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Image.asset('assets/${widget.picName}'),
    );
  }
}

```

## 第三步：编写切换图片路径状态

```dart
import 'package:flutter/material.dart';
import 'my_pic_view.dart';

main(List<String> args) {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  MyApp({Key key}) : super(key: key);

  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  String fileName = 'p1.jpg';
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          body: Column(
        children: <Widget>[
          MyPicView(
            picName: fileName,
          ),
          RaisedButton(
            onPressed: () {
              String tmpFileName = 'p1.jpg';
              if (fileName == 'p1.jpg') {
                tmpFileName = 'p2.jpg';
              }
              setState(() {
                fileName = tmpFileName;
              });
            },
            child: Text('切换图片'),
          )
        ],
      )),
    );
  }
}

```

## 代码

https://github.com/ducafecat/flutter-learn/tree/master/state_less_ful_app

## 参考

- [插件 Awesome Flutter Snippets](https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets)
- [Flutter Stateless and Stateful Widget](https://medium.com/@paridhi.softinator/flutter-stateless-and-stateful-widget-4f1ef1fb7177)
- [Flutter: Stateful vs Stateless Widget](https://medium.com/flutter-community/flutter-stateful-vs-stateless-db325309deae)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
