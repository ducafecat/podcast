---
title: 在 dart fluter 中使用 typedef
tags: flutter
categories: 译文
date: 2021-07-20 00:00:00
---

![](2021-07-20-08-05-07.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/explore-typedef-in-dart-fluter-6dd102fdf5f9

## 参考

- https://dart.dev/guides/language/language-tour#typedefs

## 正文

![](2021-07-20-06-13-37.png)

在这个博客中，我们将探索 TypeDef In Dart & Fluter。它告诉你在 Dart 中使用 typedef 的最好方法。它同样工程在 Flutter 和有一个利用例子在您的 Flutter 应用程序。

在 Dart 中，您可以使用 typedef 关键字创建类型别名来使用某种类型。本文介绍了如何制作函数型和非函数型的 typedef，以及如何利用所制作的 typedef。

### 如何为函数使用 typedef

Typedef 关键字最初是在 Dart 1 中使用的，用来暗示函数。在 Dart 1 中，如果需要将函数用作变量、字段或边界，则需要首先使用 typedef。

要使用类型别名，只需将函数标记降级为 typedef。从那时起，您可以使用 typedef 作为变量、字段或边界，如下面的模型所示。

```dart
typedef IntOperation<int> = int Function(int a, int b);

int processTwoInts (IntOperation<int> intOperation, int a, int b) {
  return intOperation(a, b);
}

class MyClass {

  IntOperation<int> intOperation;

  MyClass(this.intOperation);

  int doIntOperation(int a, int b) {
    return this.intOperation(a, b);
  }
}

void main() {
  IntOperation<int> sumTwoNumbers = (int a, int b) => a + b;
  print(sumTwoNumbers(2, 2));

  print(processTwoInts(sumTwoNumbers, 2, 1));

  MyClass myClass = MyClass(sumTwoNumbers);
  print(myClass.doIntOperation(4, 4));
}
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像屏幕下方的最终输出一样:

```sh
4
3
8
```

> 下面是函数具有泛型参数类型的另一个模型。

```dart
typedef Compare<T> = bool Function(T a, T b);
bool compareAsc(int a, int b) => a < b;
int compareAsc2(int a, int b) => a - b;

bool doComparison<T>(Compare<T> compare, T a, T b) {
  assert(compare is Compare<T>);
  return compare(a, b);
}

void main() {
  print(compareAsc is Compare<int>);
  print(compareAsc2 is Compare<int>);

  doComparison(compareAsc, 1, 2);
  doComparison(compareAsc2, 1, 2);
}
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像屏幕下方的最终输出一样:

```sh
true
false
true
```

自从 Dart 2 之后，你可以在任何地方使用函数类型的标点符号。因此，再使用 typedef 并不重要。另外还表示喜欢内联函数类型。这是因为阅读代码的个人可以直接看到函数类型。下面是可以与没有 typedef 的主体模型进行比较的内容。

```dart
int processTwoInts (int Function(int a, int b) intOperation, int a, int b) {
  return intOperation(a, b);
}

class MyClass {

  int Function(int a, int b) intOperation;

  MyClass(this.intOperation);

  int doIntOperation(int a, int b) {
    return this.intOperation(a, b);
  }
}

void main() {
  int Function(int a, int b) sumTwoNumbers = (int a, int b) => a + b;
  print(sumTwoNumbers(2, 2));

  print(processTwoInts(sumTwoNumbers, 2, 1));

  MyClass myClass = MyClass(sumTwoNumbers);
  print(myClass.doIntOperation(4, 4));
}
```

尽管如此，如果函数很长而且大部分时间被利用，那么使用 typedef 很有价值。

### 对 Non-Functions 使用 typedef:

在 Dart 2.13 之前，你可以使用 typedef 来处理函数类型。自从 Dart 2.13 以来，你同样可以使用 typedefs 来创建暗示非函数的类型别名。使用基本上是相同的，你只需要允许类型作为一个 typedef。

首先，你的 Dart 表格应该是 2.13 或以上版本。为 Flutter，你需要利用版本 2.2 或以上。此外，您还需要在 pubspec 中刷新基本 SDK 表单。Yaml to 2.13.0 and run bar get (for Dart)或 Flutter pub get (for Flutter)。

```yaml
environment:
  sdk: ">=2.13.0 <3.0.0"
```

例如，您需要描述存储整数数据列表的类型。由于这个原因，您可以创建一个 typedef，其类型是 `List <int>` 。之后，如果需要描述用于放置信息显示的变量，可以使用 typedef 作为类型。在下面的模型中，我们刻画了一个类型为 `List <int>` 的被认为是 DataList 的 typedef。正如您可以在下面的代码中找到的，利用 typedef 可以给您提供与利用实际类型相似的操作。您可以直接降级列表值，并访问 List 的技术和属性。如果你检查 runtimeType，你会得到 `List <int>` 作为结果。

```dart
typedef DataList = List<int>;

void main() {
  DataList data = [50, 60];
  data.add(100);
  print('length: ${data.length}');
  print('values: $data');
  print('type: ${data.runtimeType}');
}
```

> 当我们运行应用程序时，我们应该得到屏幕的输出，就像屏幕下方的最终输出一样:

```sh
length: 3
values: [50,60,100]
type: List<int>
```

与变量不同，类型别名同样可以用作技术的字段、参数和返回值。

```dart
typedef DataList = List<int>;

class MyClass {

  DataList currentData;

  MyClass({required this.currentData});

  set data(DataList currentData) {
    this.currentData = currentData;
  }

  ScoreList getMultipliedData(int multiplyFactor) {
    DataList result = [];

    currentData.forEach((element) {
      result.add(element * multiplyFactor);
    });

    return result;
  }
}

void main() {
  MyClass myClass = MyClass(currentData: [50, 60, 70]);

  myClass.data = [60, 70];
  print(myClass.currentData);

  print(myClass.getMultipliedData(3));
}
```

> 当我们运行应用程序时，我们应该得到屏幕的输出，就像屏幕下方的最终输出一样:

```sh
[70, 90]
[180, 210]
```

下面是另一种模式。例如，您需要一个用于存储请求正文的类型，该类型的键和值类型对于每种类型都可能不同。对于这种情况，`Map <String，dynamic> data type` 是合理的。尽管如此，每次您需要声明一个请求主体变量时，您可以为该类型创建 typedef，而不是使用 `Map <String，dynamic>` 。

```dart
typedef RequestBody = Map<String, dynamic>;

void main() {
  final RequestBody requestBody1 = {
    'type': 'BUY',
    'itemId': 2,
    'amount': 200,
  };
  final RequestBody requestBody2 = {
    'type': 'CANCEL_BUY',
    'orderId': '04567835',
  };

  print(requestBody1);
  print(requestBody2);
}
```

> 当我们运行应用程序时，我们应该得到屏幕的输出，就像屏幕下方的最终输出一样:

```json
{type: BUY, itemId: 2, amount: 200}
{type: CANCEL_BUY, orderId: 04567835}
```

还可以定义具有泛型类型参数的类型别名。下面的 ValueList 类型别名有一个泛型类型参数 t。使用类型别名定义变量时，可以传递要使用的泛型类型。

类似地，您可以表示具有泛型类型参数的类型别名。下面的 NumberList 类型别名具有一个非独占类型参数 t。在利用类型别名对变量进行特征化时，可以传递常规类型以进行利用。

```dart
typedef NumberList<T> = List<T>;

void main() {
  NumberList<String> numbers = ['1', '2', '3'];
  numbers.add('4');
  print('length: ${numbers.length}');
  print('numbers: $numbers');
  print('type: ${numbers.runtimeType}');
}
```

> 当我们运行应用程序时，我们应该得到屏幕的输出，就像屏幕下方的最终输出一样:

```sh
length: 4
numbers: [1, 2, 3, 4]
type: List<String>
```

### Usage in Flutter

下面的代码是一个 Flutter 模型，它为 `List <widget>` 类型定义了 typedef。

```dart
import 'package:flutter/material.dart';

typedef WidgetList = List<Widget>;

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: TypedefExample(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class TypedefExample extends StatelessWidget {

  WidgetList buildMethod() {
    return <Widget>[
      const FlutterLogo(size: 60),
      const Text('FlutterDevs.com', style: const TextStyle(color:  Colors.blue, fontSize: 24)),
    ];
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Flutter Demo'),
      ),
      body: SizedBox(
        width: double.infinity,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: buildMethod(),
        ),
      ),
    );
  }
}
```

### Conclusion

在这篇文章中，我解释了在 Dart & Fluter 中 TypeDef 的基本结构，您可以根据自己的选择修改这个代码。这是一个小型介绍 TypeDef 在 Dart 和 Fluter 对用户交互从我这边，它的工作使用 Flutter。

我希望这个博客能够为你提供足够的信息，帮助你在你的项目中尝试使用 TypeDef In Dart & Fluter。这就是如何制作和利用 Dart/Flutter 中的 typedef。您需要允许 typedef 的类型或函数签名。然后，在这一点上，可以将所生成的 typedef 用作策略的变量、字段、参数或返回值。所以请尝试一下。

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
