---
title: Dart语言学习 - 22 abstract 抽象
date: 2018-11-28 16:17:42
tags: dart
categories: Dart语言学习
---

# 本节目标

- 抽象 类、函数
- 接口方式使用
- 继承方式使用

# 环境

- Dart 2.0.0

# abstract 类、函数、成员

- 普通类前加 abstract

```dart
abstract class Person {
  static const String name = 'ducafecat';
  void printName(){
    print(name);
  }
}
```

# 不能直接 new 实例化

```dart
var p = Person();
p.printName();
```

> `Dart 2` 开始 `new` 可以不写，提高阅读体验

# 继承方式使用

定义

```dart
class Teacher extends Person {
}
```

实例

```dart
var user = Teacher();
user.printName();
```

# 接口方式使用

定义

```dart
abstract class Person {
  static const String name = '';
  void printName();
}

class Student implements Person {
  String name = 'this is student';
  void printName() {
    print(name);
  }
}
```

实例

```dart
var user = Student();
user.printName();
```

# 代码

- [abstract](https://github.com/ducafecat/dart-learn/blob/master/19-%E7%B1%BB/abstract.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
