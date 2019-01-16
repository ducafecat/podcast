---
title: Dart语言学习 - 31 可调用类 callable
date: 2019-01-16 11:37:26
tags: dart
categories: Dart语言学习
---

# 本节目标

- 定义并执行可定义类

# 环境

- Dart 2.1.0

# callable

```dart
main(List<String> args) {
  var phone = IOSPhone();
  phone('911');

  // IOSPhone()('911');
}

class IOSPhone {
  call(String num) {
    print('phone number is $num');
  }
}
```

# 代码

- [可调用类](https://github.com/ducafecat/dart-learn/blob/master/31-%E5%8F%AF%E8%B0%83%E7%94%A8%E7%B1%BB/callable.dart)

# 参考

- [callable-classes](https://www.dartlang.org/guides/language/language-tour#callable-classes)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
