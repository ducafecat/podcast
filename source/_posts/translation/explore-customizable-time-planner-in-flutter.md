---
title: Flutter 定制时间简化组件 time_planner
tags: flutter
categories: 译文
date: 2021-08-06 00:00:00
---

![](2021-08-06-06-28-32.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/explore-customizable-time-planner-in-flutter-c8108218b52c

## 参考

- https://pub.dev/packages/time_planner

## 正文

![](2021-08-06-06-05-41.png)

从一开始，Flutter 就是一次伟大的邂逅。建造迷人的用户界面从来没有这么快。不管你是一个业余爱好者还是一个有教养的开发者，要无可救药地迷恋上 Flutter 并不难。所有的软件开发人员都明白，日期是最棘手的事情。同样，时间表也不是特例。

在移动应用程序中，有很多情况下用户需要输入出生日期、预订机票、安排会议等等。

在这个文章，我们将探索定制的时间规划 Flutter。我们还将实现一个演示程序，并创建一个可定制的时间计划使用时间规划器包在您的 Flutter 应用程序。

https://pub.dev/packages/time_planner

### Introduction

一个愉快的，简单的利用，定制的时间规划为 Flutter 移动，桌面和网络。这是一个按时间表向客户显示任务的小部件。每行显示一个小时，每列显示一天，但是您可以更改该部分的标题并显示您需要的任何其他内容。

![](2021-08-06-06-08-07.png)

![](2021-08-06-06-07-08.png)

![](2021-08-06-06-07-33.png)

![](2021-08-06-06-08-23.png)

这个演示视频显示了如何创建一个可定制的时间计划在 Flutter。它展示了如何定制的时间计划将工作，使用您的 Flutter 应用程序的时间计划包。它显示当用户点击任何行和列时，将创建一个随机的时间计划器。动画的。它会显示在你的设备上。

### 属性

时间计划器有以下几个属性:

- startHour: 这属性是用来时间从这个开始，它将从 1 开始
- endHour: 这属性用于此时间结束，最大值为 24
- headers: 这属性用于创建天数，每天是一个 TimePlannerTitle。你应该至少创造一天
- tasks: 这属性用于在时间计划器上列出小部件
- style: 这属性用于时间计划程序的样式
- currentTimeAnimation: 这属性用于小部件加载滚动到动画的当前时间。默认值为 true

### Implementation

- 第一步: 添加依赖项

将依赖项添加到 pubspec ー yaml 文件。

```yaml
time_planner: ^0.0.3
```

- 第二步: 导入

```dart
import 'package:time_planner/time_planner.dart';
```

- 第三步: 在应用程序的根目录中运行 flutter 软件包。

```sh
flutter packages get
```

### 代码

你需要分别在你的代码中实现它:

> 在 lib 文件夹中创建一个名为 main.dart 的新 dart 文件。

首先，我们创建一个名为变量任务的 TimePlannerTask 列表。

```dart
List<TimePlannerTask> tasks = [];
```

我们将创建一个 \_addobject ()方法。

```dart
void _addObject(BuildContext context) {
  List<Color?> colors = [
    Colors.purple,
    Colors.blue,
    Colors.green,
    Colors.orange,
    Colors.cyan
  ];

  setState(() {
    tasks.add(
      TimePlannerTask(
        color: colors[Random().nextInt(colors.length)],
        dateTime: TimePlannerDateTime(
            day: Random().nextInt(10),
            hour: Random().nextInt(14) + 6,
            minutes: Random().nextInt(60)),
        minutesDuration: Random().nextInt(90) + 30,
        daysDuration: Random().nextInt(4) + 1,
        onTap: () {
          ScaffoldMessenger.of(context).showSnackBar(
              SnackBar(content: Text('You click on time planner object')));
        },
        child: Text(
          'this is a demo',
          style: TextStyle(color: Colors.grey[350], fontSize: 12),
        ),
      ),
    );
  });

  ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Random task added to time planner!')));
}
```

在函数中，我们将添加 tasks.add ()方法。在内部，我们将添加 TimePlannerTask ()小部件。在这个小部件中，我们将添加颜色、日期时间、 minutesDuration 和 daysDuration。我们还将在用户点击时间计划器时显示 snackBar 消息。

在正文中，我们将添加 TimePlanner ()小部件。在内部，我们将添加 startHour、 endHour 和 header。在头文件中，我们将添加一些 TimePlannerTitle ()。此外，我们还将添加任务和样式。

```dart
TimePlanner(
  startHour: 2,
  endHour: 24,
  headers: [
    TimePlannerTitle(
      date: "7/20/2021",
      title: "tuesday",
    ),
    TimePlannerTitle(
      date: "7/21/2021",
      title: "wednesday",
    ),
    TimePlannerTitle(
      date: "7/22/2021",
      title: "thursday",
    ),
    TimePlannerTitle(
      date: "7/23/2021",
      title: "friday",
    ),
    TimePlannerTitle(
      date: "7/24/2021",
      title: "saturday",
    ),
    TimePlannerTitle(
      date: "7/25/2021",
      title: "sunday",
    ),
    TimePlannerTitle(
      date: "7/26/2021",
      title: "monday",
    ),
    TimePlannerTitle(
      date: "7/27/2021",
      title: "tuesday",
    ),
    TimePlannerTitle(
      date: "7/28/2021",
      title: "wednesday",
    ),
    TimePlannerTitle(
      date: "7/29/2021",
      title: "thursday",
    ),
    TimePlannerTitle(
      date: "7/30/2021",
      title: "friday",
    ),
    TimePlannerTitle(
      date: "7/31/2021",
      title: "Saturday",
    ),
  ],
  tasks: tasks,
  style: TimePlannerStyle(
      showScrollBar: true
  ),
),
```

> 现在，我们将创建一个漂浮的 actionbutton ()。

```dart
floatingActionButton: FloatingActionButton(
  onPressed: () => _addObject(context),
  tooltip: 'Add random task',
  child: Icon(Icons.add),
),
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-08-06-06-15-06.png)

### Code File

```dart
import 'dart:math';

import 'package:flutter/material.dart';
import 'package:flutter_customizable_time_plan/splash_screen.dart';
import 'package:time_planner/time_planner.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData.dark(),
      home: Splash()
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key? key, required this.title}) : super(key: key);

  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  List<TimePlannerTask> tasks = [];

  void _addObject(BuildContext context) {
    List<Color?> colors = [
      Colors.purple,
      Colors.blue,
      Colors.green,
      Colors.orange,
      Colors.cyan
    ];

    setState(() {
      tasks.add(
        TimePlannerTask(
          color: colors[Random().nextInt(colors.length)],
          dateTime: TimePlannerDateTime(
              day: Random().nextInt(10),
              hour: Random().nextInt(14) + 6,
              minutes: Random().nextInt(60)),
          minutesDuration: Random().nextInt(90) + 30,
          daysDuration: Random().nextInt(4) + 1,
          onTap: () {
            ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(content: Text('You click on time planner object')));
          },
          child: Text(
            'this is a demo',
            style: TextStyle(color: Colors.grey[350], fontSize: 12),
          ),
        ),
      );
    });

    ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Random task added to time planner!')));
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        automaticallyImplyLeading: false,
        title: Text(widget.title),
        centerTitle: true,
      ),
      body: Center(
        child: TimePlanner(
          startHour: 2,
          endHour: 24,
          headers: [
            TimePlannerTitle(
              date: "7/20/2021",
              title: "tuesday",
            ),
            TimePlannerTitle(
              date: "7/21/2021",
              title: "wednesday",
            ),
            TimePlannerTitle(
              date: "7/22/2021",
              title: "thursday",
            ),
            TimePlannerTitle(
              date: "7/23/2021",
              title: "friday",
            ),
            TimePlannerTitle(
              date: "7/24/2021",
              title: "saturday",
            ),
            TimePlannerTitle(
              date: "7/25/2021",
              title: "sunday",
            ),
            TimePlannerTitle(
              date: "7/26/2021",
              title: "monday",
            ),
            TimePlannerTitle(
              date: "7/27/2021",
              title: "tuesday",
            ),
            TimePlannerTitle(
              date: "7/28/2021",
              title: "wednesday",
            ),
            TimePlannerTitle(
              date: "7/29/2021",
              title: "thursday",
            ),
            TimePlannerTitle(
              date: "7/30/2021",
              title: "friday",
            ),
            TimePlannerTitle(
              date: "7/31/2021",
              title: "Saturday",
            ),
          ],
          tasks: tasks,
          style: TimePlannerStyle(
              showScrollBar: true
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => _addObject(context),
        tooltip: 'Add random task',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

### 结语

在这篇文章中，我已经简单地解释了 Customizable Time Planner 的基本结构; 您可以根据自己的选择修改这段代码。这是一个小规模的介绍定制时间计划对用户交互从我这边，它的工作使用 Flutter。

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
