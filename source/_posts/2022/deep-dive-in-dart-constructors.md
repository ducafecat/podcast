---
title: Dart Constructors 构造函数使用技巧整理
tags: flutter
categories: 译文
date: 2022-04-26 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220426094828.png)

## 原文

> https://itnext.io/deep-dive-in-dart-constructors-51e4c006fb8f

## 参考

- https://dart.dev/guides/language/language-tour#factory-constructors
- https://www.freecodecamp.org/news/constructors-in-dart
- https://stackoverflow.com/questions/52299304/dart-advantage-of-a-factory-constructor-identifier
- https://dash-overflow.net/articles/factory
- https://flutterigniter.com/deconstructing-dart-constructors
- https://dart.dev/guides/language/language-tour

## 正文

这篇文章是探讨下 Dart 构造函数的一些使用技巧

**首先**

**什么是构造函数？**

> 构造函数是用于初始化对象的特殊方法。在创建类的对象时调用构造函数。

**默认情况**

```dart
final ehe = MyClass(); // Creates an instanceclass MyClass {
  MyClass(); // Fires immediately when created (this guy is cons.)
}
```

**在构造函数中只有一个规则**

也就是说;

> 与它的类名一样命名它! ！

**好的，我们知道了! 但是..**

**我们具体有哪些类型的构造函数类型？**

**缺省构造函数 ー `Class()`**

```dart
// Default Constructor
// 默认什么都不做
class User {
  String name = 'ehe';
  User();
}

///////////////////
// Constructor with parameters
// 构造时初始变量
class User {
  String name;
  User(this.name);
}

///////////////////
// Constructor with the initial method
// 构造函数内写入你的逻辑
class User {
  String name;
  User(this.name) {
    // do some magic
  }
}

/////////////////
// Constructor with assertion
// 使用 Asserts 去检查你的规则
class User {
  String name;
  User(this.name) : assert(name.length > 3);
}

////////////////
// Constructor with initializer
// 初始化你的变量
class User {
  static String uppercase(String e) => e.toUpperCase();
  String name;
  User(name) : name = yell(name);
  static String yell(String e) => e.toUpperCase();
}

/////////////////////
// Constructor with super()
// override 变量
class Person {
  String id;
  Person(this.id);
}
class User extends Person {
  String name;
  User(this.name, String id) : super(id);
}

/////////////////////
// Constructor with this()
// 命名构造函数
class User {
  String name;
  int salary;
  User(this.name, this.salary);
  User.worker(String name) : this(name, 10);
  User.boss(String name) : this(name, 9999999);
}
```

**私有构造函数ー `Class._()`**

您可以使用 `_` 创建私有构造函数，但是它的好处是什么呢？

让我们来看一个例子！

```dart
class Print {
  static void log(String message) => print(message);
}

Print.log('ehe');

// 你想写一个像这样的util，但有一个问题，因为你也可以创建一个我们不想要的实例。

Print(); // 在这种情况下，这是绝对不必要的

// 如何防止这种情况？答案是私有构造函数！

class Print {
  Print._(); // 这将阻止创建实例
  static void log(String message) => print(message);
}

Print(); // 这将给出现在的编译时错误

Your instance is safe now!

```

所以基本上你可以阻止创建一个实例！

**命名构造函数ー `class.Named()`**

您可以在一个 `class` 中创建不同类型的实例

For example;

例如:

```dart
class User {
  String name;
  int salary;
  User.worker(this.name) : salary = 10;
  User.boss(this.name) : salary = 99999999;
}
```

**私有命名构造函数ー `class._Named ()`**

您可以很容易地清洗您的实例！

```dart
class User {
  String name;
  int salary;
  User.worker(this.name) : salary = 10;
  User.boss(this.name) : salary = 99999999;
  User._mafia(this.name) : salary = 9999999999999;
}
```

除了玩笑之外，这是非常有帮助的！

例如，您可以使用私有构造函数创建单例对象！

```dart
class User {
  User._privateConstructor();
  static final User instance = User._privateConstructor();
}
```

**注意**

你可以在一些项目中看到 `_internal` 内部关键字。没什么特别的。**\_internal** construction 只是一个 **.\_internal** 通常给类私有的构造函数的名称(不需要这个名称)。可以使用任何 **Class.\_someName** 结构创建一个私有构造函数)。

Const Constructor ー `const Class()`

您可以使用 `const constructor!` 构造函数使类变为不可变的！

> 常量构造函数是一种优化！编译器使对象成为不可变的，为所有 `Text('Hi!')` 对象。ー Frank Treacy

```dart
const user = User('ehe');

class User {
  final String name;
  const User(this.name);
}
```

**工厂构造函数ー `factory class Class()`**

我们说过施工人员不允许回来，你猜怎么着？

工厂建造者可以！

工厂建造者还能做什么？

您根本不需要创建一个新实例！您可以调用另一个构造函数或子类，甚至可以从缓存返回一个实例！

最后，对工厂的小小警告！

无法调用超类构造函数 (`super()`)

**简单的例子**

```dart
class User {
  final String name;
  User(this.name);

  factory User.fromJson(Map<String, dynamic> json) {
    return User(json["name"]);
  }
}

// Singleton Example
class User {
  User._internal();
  static final User _singleton = Singleton._internal();

  factory User() => _singleton;
}
```

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
