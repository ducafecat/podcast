---
title: 在 Flutter 中探索 StreamBuilder
tags: flutter
categories: 译文
date: 2021-09-01 00:00:00
---

![](2021-09-01-17-37-55.png)

## 原文

> https://medium.com/flutterdevs/exploring-streambuilder-in-flutter-5958381bca67

## 正文

异步交互可能需要一个理想的机会来进行总结。偶尔，在周期结束之前可能会发出一些值。在 Dart 中，您可以创建一个返回 Stream 的容量，该容量可以在异步进程处于活动状态时发射一些值。假设您需要根据一个 Stream 的快照在 Flutter 中构造一个小部件，那么有一个名为 StreamBuilder 的小部件。

在这个博客中，我们将探索 Flutter 中的 StreamBuilder。我们还将实现一个演示程序，并向您展示如何在您的 Flutter 应用程序中使用 StreamBuilder。

# 介绍:

StreamBuilder 可以监听公开的流，并返回小部件和捕获获得的流信息的快照。造溪者提出了两个论点。

> _A stream_
>
> 构建器，它可以将流中的多个组件更改为小部件

Stream 像一条线。当您从一端输入值而从另一端输入侦听器时，侦听器将获得该值。一个流可以有多个侦听器，这些侦听器的负载可以获得流水线，流水线将获得等价值。如何在流上放置值是通过使用流控制器实现的。流构建器是一个小部件，它可以将用户定义的对象更改为流。

# 建造者:

> 要使用 StreamBuilder，需要调用下面的构造函数:

```dart
const StreamBuilder({
Key? key,
Stream<T>? stream,
T? initialData,
required AsyncWidgetBuilder<T> builder,
})
```

实际上，您需要创建一个 Stream 并将其作为流争用传递。然后，在这一点上，您需要传递一个 AsyncWidgetBuilder，该 AsyncWidgetBuilder 可用于构造依赖于 Stream 快照的小部件。

# 参数:

> 下面是 StreamBuilderare 的一些参数:

- `Key? key`: 小部件的键，用于控制小部件如何被另一个小部件取代
- `Stream<T>? stream`: 一个流，其快照可以通过生成器函数获得
- `T? initialData`: 将利用这些数据制作初始快照
- `required AsyncWidgetBuilder<T> builder`: 生成过程由此生成器使用

# 如何实现 dart 文件中的代码:

你需要分别在你的代码中实现它:

> 让我们创建一个流:

下面的函数返回一个每秒生成一个数字的 Stream。你需要使用 async \* 关键字来创建一个流。若要发出值，可以使用 yield 关键字后跟要发出的值。

```dart
Stream<int> generateNumbers = (() async* {
  await Future<void>.delayed(Duration(seconds: 2));

  for (int i = 1; i <= 10; i++) {
    await Future<void>.delayed(Duration(seconds: 1));
    yield i;
  }
})();
```

> _From that point onward, pass it as the stream argument_
>
> 从那一点开始，把它作为流参数传递下去

```dart
StreamBuilder<int>(
stream: generateNumbers,
// other arguments
)
```

> 让我们创建一个 AsyncWidgetBuilder

构造函数期望您传递一个类型为 AsyncWidgetBuilder 的命名争用构建器。这是一个有两个参数的函数，它们的类型都是 BuildContext 和 AsyncSnapshot \< t > 。后续的边界\(包含当前快照\)可以用来确定应该呈现的内容。

要创建这个函数，首先需要了解 AsyncSnapshot。AsyncSnapshot 是使用异步计算的最新通信的不变描述。在这种独特的情况下，它解决了与 Stream 的最新通信。可以通过 AsyncSnapshot 属性获取流的最新快照。您可能需要使用的属性之一是 connectionState，这个枚举将当前关联状态转换为异步计算，在这种特殊情况下，这种异步计算就是 Steam。

```dart
StreamBuilder<int>(
  stream: generateNumbers,
  builder: (
      BuildContext context,
      AsyncSnapshot<int> snapshot,
      ) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return CircularProgressIndicator();
    } else if (snapshot.connectionState == ConnectionState.active
        || snapshot.connectionState == ConnectionState.done) {
      if (snapshot.hasError) {
        return const Text('Error');
      } else if (snapshot.hasData) {
        return Text(
            snapshot.data.toString(),
            style: const TextStyle(color: Colors._red_, fontSize: 40)
        );
      } else {
        return const Text('Empty data');
      }
    } else {
      return Text('State: ${snapshot.connectionState}');
    }
  },
),
```

AsyncSnapshot 还有一个名为 hasError 的属性，可用于检查快照是否包含非空错误值。如果异步活动的最新结果失败，hasError 值将有效。为了获取信息，首先，您可以通过获取其 hasData 属性来检查快照是否包含信息，如果 Stream 有效地释放了任何非空值，那么 hasData 属性将是有效的。然后，在这一点上，您可以从 AsyncSnapshot 的数据属性获取信息。

由于上面属性的值，您可以计算出应该在屏幕上呈现什么。在下面的代码中，当 connectionState 值正在等待时，将显示一个 CircularProgressIndicator。当 connectionState 更改为 active 或 done 时，可以检查快照是否有错误或信息。建造函数称为 Flutter 管道的检测。因此，它将获得一个与时间相关的快照子组。这意味着，如果在实际上相似的时间里，Stream 发出了一些值，那么一部分值可能没有传递给构建器。

> 枚举有一些可能的值:

- \> none: 无: 不与任何异步计算关联。如果流为空，则可能发生
- \> waiting: 等待: 与异步计算关联并等待协作。在这个上下文中，它暗示流还没有完成
- \> active: 活跃的: 与活动的异步计算相关联。例如，如果一个 Stream 已经返回了任何值，但此时还没有结束
- \> done: > 完成: 与结束的异步计算相关联。在这个上下文中，它暗示流已经完成

> 设置初始数据:

您可以选择传递一个 worth 作为 initialData 参数，这个参数将被利用，直到 Stream 发出 a。如果传递的值不为空，那么当 connectionState 在等待时，hasData 属性在任何事件中首先都将为 true

```dart
StreamBuilder<int>(
initialData: 0,
// other arguments
)
```

要在 connectionState 等待时显示初始数据，应该调整 if snapshot.connectionState = = connectionState.waiting，然后调整上面代码中的块。

```dart
if (snapshot.connectionState == ConnectionState.waiting) {
  return Column(
    crossAxisAlignment: CrossAxisAlignment.center,
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      CircularProgressIndicator(),
      Visibility(
        visible: snapshot.hasData,
        child: Text(
          snapshot.data.toString(),
          style: const TextStyle(color: Colors._black_, fontSize: 24),
        ),
      ),
    ],
  );
}
```

当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](demo.gif)

# Code File:

# 密码档案:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_steambuilder_demo/splash_screen.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Splash(),
      debugShowCheckedModeBanner: false,
    );
  }
}

Stream<int> generateNumbers = (() async* {
  await Future<void>.delayed(Duration(seconds: 2));

  for (int i = 1; i <= 10; i++) {
    await Future<void>.delayed(Duration(seconds: 1));
    yield i;
  }
})();

class StreamBuilderDemo extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return _StreamBuilderDemoState ();
  }
}

class _StreamBuilderDemoState extends State<StreamBuilderDemo> {

  @override
  initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        automaticallyImplyLeading: false,
        title: const Text('Flutter StreamBuilder Demo'),
      ),
      body: SizedBox(
        width: double._infinity_,
        child: Center(
          child: StreamBuilder<int>(
            stream: generateNumbers,
            initialData: 0,
            builder: (
                BuildContext context,
                AsyncSnapshot<int> snapshot,
                ) {
              if (snapshot.connectionState == ConnectionState.waiting) {
                return Column(
                  crossAxisAlignment: CrossAxisAlignment.center,
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    CircularProgressIndicator(),
                    Visibility(
                      visible: snapshot.hasData,
                      child: Text(
                        snapshot.data.toString(),
                        style: const TextStyle(color: Colors._black_, fontSize: 24),
                      ),
                    ),
                  ],
                );
              } else if (snapshot.connectionState == ConnectionState.active
                  || snapshot.connectionState == ConnectionState.done) {
                if (snapshot.hasError) {
                  return const Text('Error');
                } else if (snapshot.hasData) {
                  return Text(
                      snapshot.data.toString(),
                      style: const TextStyle(color: Colors._red_, fontSize: 40)
                  );
                } else {
                  return const Text('Empty data');
                }
              } else {
                return Text('State: ${snapshot.connectionState}');
              }
            },
          ),
        ),
      ),
    );
  }
}
```

# 结语:

在本文中，我已经简单介绍了 StreamBuilder 的基本结构; 您可以根据自己的选择修改这段代码。这是我对 StreamBuilder On User Interaction 的一个小小介绍，它正在使用 Flutter 工作。

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
