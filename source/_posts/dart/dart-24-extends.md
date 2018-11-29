---
title: Dart语言学习 - 24 extends 继承
date: 2018-11-28 17:45:11
tags: dart
categories: Dart语言学习
---

# 本节目标

- 实现继承
- 继承抽象类的问题
- 不可多继承
- 父类调用
- 调用父类构造
- 重写超类函数

# 环境

- Dart 2.0.0

# 实现继承

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

class AndroidPhone extends Phone {
}
```

# 继承抽象类的问题

```dart
void main() {
  var p = AndroidPhone();
  p.startup();
  p.shutdown();
}

abstract class Phone {
  void startup();
  void shutdown();
}

class AndroidPhone extends Phone {
}
```

> 抽象类中只定义抽象函数，实例化访问会报错

# 不可多继承

```dart
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

class AndroidPhone extends Phone, Mobile {
}
```

# 父类调用

```dart
void main() {
  var p = AndroidPhone();
  p.startup();
}

class Phone {
  void startup() {
    print('开机');
  }
  void shutdown() {
    print('关机');
  }
}

class AndroidPhone extends Phone {
  void startup() {
    super.startup();
    print('AndroidPhone 开机');
  }
}
```

> super 对象可以访问父类

# 调用父类构造

```dart
void main() {
  var p = AndroidPhone(12345678);
  p.showNumber();
}

class Mobile {
  int number;
  int signal;
  Mobile(this.number);
  void showNumber() {
    print('010-${number}');
  }
}

class AndroidPhone extends Mobile {
  AndroidPhone(int number) : super(number);
}
```

> 可调用父类的 构造函数

# 重写超类函数

```dart
void main() {
  dynamic p = AndroidPhone(12345678);
  p.showNumber111();
}

class Mobile {
  int number;
  int signal;
  Mobile(this.number);
  void showNumber() {
    print('010-${number}');
  }
}

class AndroidPhone extends Mobile {
  AndroidPhone(int number) : super(number);

  @override
  void noSuchMethod(Invocation mirror) {
    print('被重写 noSuchMethod');
  }
}
```

> 在重写的函数上加修饰符 `@override`

# 代码

- [extends](https://github.com/ducafecat/dart-learn/blob/master/19-%E7%B1%BB/extends.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
