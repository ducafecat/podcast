---
title: Dart 语言的7个很酷的特点
tags: flutter
categories: 译文
date: 2022-05-08 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220508094228.png)

## 原文

> https://betterprogramming.pub/7-cool-features-of-dart-language-809608371e28

## 参考

- https://dart.dev/guides/language/language-tour

## 正文

今天的文章简短地揭示了 Dart 语言所提供的很酷的特性。更多时候，这些选项对于简单的应用程序是不必要的，但是当你想要通过简单、清晰和简洁来改进你的代码时，这些选项是一个救命稻草。

考虑到这一点，我们走吧。

### Cascade 级联

Cascades (`..`, `?..`) 允许你对同一个对象进行一系列操作。这通常节省了创建临时变量的步骤，并允许您编写更多流畅的代码。

```dart
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;

//above block of code when optimized
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
```

### Abstract 抽象类

使用 `abstract` 修饰符定义一个 `_abstract` 抽象类(无法实例化的类)。抽象类对于定义接口非常有用，通常带有一些实现。

```dart
// This class is declared abstract and thus
// can't be instantiated.
abstract class AbstractContainer {
  // Define constructors, fields, methods...

  void updateChildren(); // Abstract method.
}
```

### Factory constructors 工厂建造者

在实现不总是创建类的新实例的构造函数时使用 `factory` 关键字。

```dart
class Logger {
  String name;
  Logger(this.name);
  factory Logger.fromJson(Map<String, Object> json) {
    return Logger(json['name'].toString());
  }
}
```

### Named 命名构造函数

使用命名构造函数为一个类实现多个构造函数或者提供额外的清晰度:

```dart
class Points {
  final double x;
  final double y;

  //unnamed constructor
  Points(this.x, this.y);

  // Named constructor
  Points.origin(double x,double y)
      : x = x,
        y = y;

  // Named constructor
  Points.destination(double x,double y)
      : x = x,
        y = y;
}
```

### Mixins 混合物

Mixin 是在多个类层次结构中重用类代码的一种方法。

要实现 _implement_ mixin，创建一个声明没有构造函数的类。除非您希望 `mixin` 可以作为常规类使用，否则请使用 `mixin` 关键字而不是类。

若要使用 mixin，请使用后跟一个或多个 mixin 名称的 with 关键字。

若要限制可以使用 mixin 的类型，请使用 on 关键字指定所需的超类。

```dart
class Musician {}

//creating a mixin
mixin Feedback {
  void boo() {
    print('boooing');
  }

  void clap() {
    print('clapping');
  }
}

//only classes that extend or implement the Musician class
//can use the mixin Song
mixin Song on Musician {
  void play() {
    print('-------playing------');
  }

  void stop() {
    print('....stopping.....');
  }
}

//To use a mixin, use the with keyword followed by one or more mixin names
class PerformSong extends Musician with Feedback, Song {
  //Because PerformSong extends Musician,
  //PerformSong can mix in Song
  void awesomeSong() {
    play();
    clap();
  }

  void badSong() {
    play();
    boo();
  }
}

void main() {
  PerformSong().awesomeSong();
  PerformSong().stop();
  PerformSong().badSong();
}
```

### Typedefs

类型别名ー是指代类型的一种简明方式。通常用于创建在项目中经常使用的自定义类型。

```dart
typedef IntList = List<int>;
List<int> i1=[1,2,3]; // normal way.
IntList i2 = [1, 2, 3]; // Same thing but shorter and clearer.

//type alias can have type parameters
typedef ListMapper<X> = Map<X, List<X>>;
Map<String, List<String>> m1 = {}; // normal way.
ListMapper<String> m2 = {}; // Same thing but shorter and clearer.
```

### Extension 扩展方法

在 Dart 2.7 中引入的扩展方法是一种向现有库和代码中添加功能的方法。

```dart
//extension to convert a string to a number
extension NumberParsing on String {
  int customParseInt() {
    return int.parse(this);
  }

  double customParseDouble() {
    return double.parse(this);
  }
}

void main() {
  //various ways to use the extension

  var d = '21'.customParseDouble();
  print(d);

  var i = NumberParsing('20').customParseInt();
  print(i);
}
```

### 可选的位置参数

通过将位置参数包装在方括号中，可以使位置参数成为可选参数。可选的位置参数在函数的参数列表中总是最后一个。除非您提供另一个默认值，否则它们的默认值为 null。

```dart
String joinWithCommas(int a, [int? b, int? c, int? d, int e = 100]) {
  var total = '$a';
  if (b != null) total = '$total,$b';
  if (c != null) total = '$total,$c';
  if (d != null) total = '$total,$d';
  total = '$total,$e';
  return total;
}

void main() {
  var result = joinWithCommas(1, 2);
  print(result);
}
```

### unawaited_futures

当您想要启动一个 `Future` 时，建议的方法是使用 `unawaited`

否则你不加 async 就不会执行了

```dart
import 'dart:async';

Future doSomething() {
  return Future.delayed(Duration(seconds: 5));
}

void main() async {
  //the function is fired and awaited till completion
  await doSomething();

  // Explicitly-ignored
  //The function is fired and forgotten
  unawaited(doSomething());
}
```

end.

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
