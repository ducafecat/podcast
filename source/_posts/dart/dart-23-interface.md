---
title: Dart语言学习 - 23 interface 接口
date: 2018-11-28 17:23:46
tags: dart
categories: Dart语言学习
---

# 本节目标

- 实现接口
- implements 多接口

# 环境

- Dart 2.0.0

# Dart 中没有 interface 关键字

# 实现接口

```dart
void main() {
  var p = AndroidPhone();
  p.startup();
  p.shutdown();
}

abstract class IPhone {
  void startup();
  void shutdown();
}

class AndroidPhone implements IPhone {
  void startup() {
    print('AndroidPhone 开机');
  }
  void shutdown() {
    print('AndroidPhone 关机');
  }
}
```

# 从一个普通类履行接口

```dart
void main() {
  var p = AndroidPhone();
  p.startup();
  p.shutdown();
}

class Phone {
  void startup() {
    print('开机');
  }
  void shutdown() {
    print('关机');
  }
}

class AndroidPhone implements Phone {
  void startup() {
    print('AndroidPhone 开机');
  }
  void shutdown() {
    print('AndroidPhone 关机');
  }
}
```

# 履行多接口

```dart
void main() {
  var p = AndroidPhone();
  p.startup();
  p.shutdown();
}

class Phone {
  void startup() {
    print('开机');
  }
  void shutdown() {
    print('关机');
  }
}

class Mobile {
  int signal;
}

class AndroidPhone implements Phone, Mobile {
  int signal;
  void startup() {
    print('AndroidPhone 开机');
  }
  void shutdown() {
    print('AndroidPhone 关机');
  }
}
```

# 代码

- [interface](https://github.com/ducafecat/dart-learn/blob/master/19-%E7%B1%BB/interface.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
