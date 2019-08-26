---
title: Dart语言学习 - 20 get set
date: 2018-11-17 15:30:21
tags: dart
categories: Dart语言学习
---

# 本节目标

- 定义、使用、简化 get set

# 环境

- Dart 2.0.0

# 定义、使用 get set

getter 和 setter 的好处是，你可以开始使用实例变量，后来 你可以把实例变量用函数包裹起来，而调用你代码的地方不需要修改。

定义

```dart
class People {
  String _name;

  set pName(String value) {
    _name = value;
  }

  String get pName {
    return 'people is ${_name}';
  }
}
```

使用

```dart
  var p = new People();
  p.pName = 'ducafecat';
  print(p.pName);
```

# 简化 get set

```dart
class People {
  String _name;

  set pName(String value) => _name = value;

  String get pName => 'people is ${_name}';
}
```

# 代码

- [getset.dart](https://github.com/ducafecat/dart-learn/blob/master/19-类/getset.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
