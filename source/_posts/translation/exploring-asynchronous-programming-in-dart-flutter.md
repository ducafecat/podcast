---
title: 在 Dart 和 Flutter 中探索异步编程
tags: flutter
categories: 译文
date: 2021-05-21 00:00:00
---

![](2021-05-21-09-00-00.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://medium.com/flutterdevs/exploring-asynchronous-programming-in-dart-flutter-25f341af32f

## 正文

![](2021-05-21-08-37-45.png)

异步编程是一种相等的编程类型，它允许一个工作单元独立于基本的应用程序线程运行。当工作完成时，它告诉主线程。在 Flutter 结构中可访问的 UI 小部件利用 Dart 的异步编程亮点来达到非同寻常的效果，帮助保持代码的协调性，并防止 UI 在客户端上的安全性。

在这个博客中，我们将探索在 Dart & Flutter 中的异步编程。我们将看看异步代码模式如何帮助准备用户交互和从网络恢复数据，并在您的 Flutter 应用程序中看到几个异步 Flutter 小部件的行动。

### 异步编程

它是一种在编程周期中附加在事件链上的相等执行类型。对于那些刚刚接触异步编程的人来说，这是另一种加速改进周期的技术。尽管如此，我们不能在所有实例上使用异步编程。当你在寻找直率而不是生产力的时候，它是有效的。为了处理基本的、自治的信息，异步编程是一个非同寻常的决定。

![](2021-05-21-08-41-53.png)

various perspectives 是理想的对应物 Flutter 从各种角度，在任何事件，为异步编程。即使 Dart 是单线程的，它也可以与不同的代码相关联，这些代码运行在不连续的线程中。使用 Dart 中的同步代码会造成延迟并阻碍整个程序的执行。尽管如此，异步编程解决了这个问题。此外，这提示改进了应用程序的执行和应用程序的响应性。

### 为什么我们应该使用异步编程

面是异步编程的一些应用:

- 改善工作表现及提高工作效率 反应性. 你的应用程序，特别是当你有长期运行的活动，不需要阻止执行。对于这种情况，您可以在等待长期任务的结果的同时执行其他工作

- 以一种完美的、易于理解的方式组装代码，从根本上比传统线程创建的标准代码更好，并且更好地处理代码，你编写更少的代码，你的代码将比利用过去的异步编程策略，如利用纯赋值更可行

- 您可以使用语言高亮部分的最新升级，如 async / await was presented in a flutter. 在一阵颤动中显现出来

- 对元素进行了一些改进，比如对每个 async 进行了改进，对 async 类型进行了总结，比如 Value.

### Future

未来的工作方式基本上与 Javascript 中的 Promise 相同。它有两个表达是未完成和已完成。完成的未来要么有价值，要么有错误。未完成的未来是等待函数的异步活动完成或抛出错误。

这个类有几个构造函数:

- Future. 意味着将 Duration 对象作为展示延迟后执行的时间跨度和函数的竞争对象

- 编码内存缓存意味着在内存中以原始状态存储压缩图片

- Future. 意味着使未来以错误结束

### 使用 Await/Async

Dart 中的异步和等待方法基本上与不同的方言相同，但是，不管您是否对异步编程利用异步/等待有所了解，您都应该认为在这里很容易理解。

=> Async 函数: 函数构成了异步编程的基础。这些异步函数的主体中有异步修饰符。下面是一个关于综合异步工作的例子:

```dart
void flutter() async {
  print('Flutter Devs');
}
```

=> Await 表达式: 它使您编写异步代码就像它是同步的一样。总而言之，等待发音具有下面给出的结构:

```dart
void main() async {
  await flutter();
  print('flutter Devs done');
}
```

### 用户交互事件

也许异步处理用户输入的最简单模型是用回调来响应按钮小部件上的连接事件:

```dart
FlatButton(
  child: Text("Data"),
  onPressed: () {
    print("pressed button");
  },
)
```

和大多数 Flutter 组件一样，FlatButton 组件提供了一个舒适的界面，称为 onPressed，用于响应速度快的按钮。在这里，我们已经将一个神秘的回调容量传递给了边界，它除了向控制台打印消息之外什么也不做。当客户端按下 catch 时，onPressed 事件被关闭，当偶然循环可以到达时，将执行未知函数。

在后台，有一个事件流，每次向其添加另一个事件时，都会使用任何相关信息调用回调工作。对于这种情况，基本的 catch 按钮没有相关信息，因此回调没有边界。

### 使用回调的异步网络调用

也许异步编程最著名的例子包括通过网络获取信息，例如，通过 HTTP 上的 REST 服务:

```dart
import 'package:http/http.dart' as http;final future = http.get("https://flutterdevs.com");future.then((response) {
  if (response.statusCode == 200) {
    print("Response received.");
  }
});
```

Http 包是 Dart 的包回购中最著名的一个，Pub。我在这里合并了一个导入断言，以引起对使用作为关键字的 http 名称命名导入的平均例子的注意。这些助手保持包的许多高级函数、常量和因子不与您的代码发生冲突，就像澄清像 get ()这样的函数从何而来一样。

代码模型展示了吞噬未来的经典例子。当调用 http.get ()时，对它的调用立即返回一个不适当的 Future 示例。回想一下，通过 HTTP 获得结果需要很大的能量，我们不需要在旁观时让应用程序保持惰性。这就是我们立即将未来移回来并继续执行以下代码行的原因。这些下一行使用了 Future 示例的 at that then ()策略来获得一个回调函数，这个回调函数迟早会在 REST 反应出现时执行。万一不可避免的响应 HTTP 状态码为 200(成功) ，我们可以向调试控制台打印一条直截了当的消息。

我们把这个例子改进一下怎么样。这个模型将未来存储在最后一个变量中，然后得到 then () ，但是除非你有一个有效的理由来保留这个未来案例，否则跳过这个部分是平均的，就像在相应的模型中一样:

```dart
http.get("https://flutterdevs.com").then((response) {
  if (response.statusCode == 200) {
    print("Response received.");
  }
  else {
    print("Response not received");
  }
});
```

由于对 get ()的调用会对 Future 采取步骤，因此您可以直接调用它的 then ()策略，而不需要在变量中保存将来的引用。这些代码在这些行上稍微简化了一点，但同时又可以破译。将一些有价值的回收注册链接到我们的未来是可行的，例如:

```dart
http.get("https://flutterdevs.com").then((response) {
  if (response.statusCode == 200) {
    print("Response received.");
  }
  else {
    print("Response not received");
  }
}).catchError(() {
  print("Error!");
}).whenComplete(() {
  print("Future complete.");
});
```

目前我们已经设置了一个回调函数，当 HTTP 调用关闭时将执行一个错误，而不是使用 catchError 的响应，另一个回调函数将运行时不会考虑未来如何利用 whenComplete ()完成。这种搭售的技巧是可以想象的，因为每一种策略都会给我们的未来带来参考。

### 没有回调的异步网络调用

Dart 提供了一个解决异步调用的替代例子，这个例子看起来更像习惯的同步代码，它可以使仔细阅读和推理变得更加简单。异步/等待标点符号为你处理了大量的未来物流:

```dart
Future<String> getData() async {
  final response = await http.get("https://flutterdevs.com");
  return response.body;
}
```

当您意识到将在函数内部执行异步调用时，例如 http.get () ，您可以使用 async 关键字对函数进行戳记。异步工作一致地返回未来，您可以利用其主体中的 await 关键字。对于这种情况，我们认识到 REST 调用将返回字符串信息，因此我们在返回类型上使用泛型来表示: Future < string > 。

你可以等待任何返回未来的函数。getData ()函数将在等待清晰运行之后挂起执行，并将未来返回给调用者。代码紧密地等待反应; 它等待网络调用的未来完成。然后，当反应出现荒谬时，执行继续，并将 Response 对象指定为最后一个因子，然后 getData ()返回 Response.body，这是一个字符串。您不必从 getData ()中明确返回 future，因为这样做的结果是返回了 await 的主要用途。当你有了字符串信息，你返回它，然后达特用价值结束未来。

> 为了在使用 await 时捕捉错误，你可以使用 Dart 的标准 try/catch 包括:

```dart
Future<String> getData() async {
  try {
    final response = await http.get("https://flutterdevs.com");
    return response.body;
  } catch (excute) {
    print("Error: $excute");
  }
}
```

在这个版本中，我们放置的代码，可以抛出豁免到 try 块。在一切都很容易的情况下，我们将得到一个响应并返回字符串信息，类似于在早期模型中。如果出现错误，catch 块将执行所有考虑过的内容，并向我们传递一个对豁免的引用。由于我们没有向 getData ()的最大限度添加明确的返回公告，Dart 将在那里添加一个特定的返回 null 语句，这将以 null 值结束未来。

> 请注意，如果网络调用成功，则在 try 块中会发生返回，因此不会调用隐含的返回。

回调函数有它们的用途，并且它们可以成为处理简单情况下的异步通信的一种特殊方法，例如，响应一个客户机挤压捕获。对于更多混乱的情况，例如，当你需要安排一些异步调用，每个异步调用都依赖于先前调用的结果，Dart 的异步/等待语法结构可以帮助你尽量不去安排回调，这里或那里暗指回调地狱之火。

### FutureBuilder

一个 FutureBuilder 根据给定的未来条件来组装自己。对于这个模型，我们应该期望您有一个名为 getData ()的函数，它返回 Future < string > 。

```dart
class MyStatelessWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return FutureBuilder(
      future: getData(),
      builder: (BuildContext context, AsyncSnapshot snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return CircularProgressIndicator();
        }        if (snapshot.hasData) {
          return Text(snapshot.data);
        }        return Container();
      },
    );
  }
}
```

这个定制的无状态小部件返回一个 FutureBuilder，如果 getData ()返回的 future 还没有结束，它将显示一个 advancement 指针，如果 future 已经结束，它将显示这个信息。万一这两样东西都不正确，一个空的容器就会被全面考虑。你建议 FutureBuilder 关注未来的边界，这时给它一个每次修改都需要的构建工作。构建回调获得典型的 BuildContext 争用，这是所有 Flutter 构建活动的正常情况，它同样获得 AsyncSnapshot 的出现，您可以使用 AsyncSnapshot 检查未来的状态并恢复任何信息。

这种方法有一个问题。根据 FutureBuilder 的官方文档，在构建步骤之前已经准备好了未来的必需品。

> 为了解决这个问题，我们需要使用一个有状态小部件来代替:

```dart
class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  Future<String> _dataFuture;  @override
  void initState() {
    super.initState();    _dataFuture = getData();
  }  @override
  Widget build(BuildContext context) {
    return FutureBuilder(
      future: _dataFuture,
      builder: (BuildContext context, AsyncSnapshot snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return CircularProgressIndicator();
        }        if (snapshot.hasData) {
          return Text(snapshot.data);
        }        return Container();
      },
    );
  }
}
```

这个小部件的改编在 initState ()期间获得了未来的信息。创建小部件的状态对象时，将精确地调用 initState ()技术一次。

### StreamBuilder

流类似于事件管道。信息或错误事件朝向一边，它们被传递给另一边的侦听器。当您为 StreamBuilder 提供对当前流的引用时，它会因此订阅和提取，以刷新至关重要的内容，并且它依赖于需要成为管道的任何信息来组装自己。

```dart
class MyStatelessWidget extends StatelessWidget {
  final Stream<String> dataStream;  const MyStatelessWidget({Key key, this.dataStream}) : super(key: key);  @override
  Widget build(BuildContext context) {
    return StreamBuilder<ConnectionState>(
      stream: dataStream,
      builder: (BuildContext context, AsyncSnapshot<ConnectionState> snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return CircularProgressIndicator();
        }        if (snapshot.hasData) {
          return Text(snapshot.data);
        }        return Container();
      },
    );
  }
}
```

### 总结

本文介绍了 Flutter 基本结构的“dart 与 Flutter 中的异步编程”，您可以根据自己的选择修改该代码。这是一个小的介绍在 dart 和 flutter 异步编程用户交互从我这边，它的工作使用 flutter。

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
