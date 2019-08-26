---
title: Dart语言学习 - 25 多继承类 mixin
date: 2018-12-04 11:26:18
tags: dart
categories: Dart语言学习
---

# 本节目标

- 多继承类的实现方式
- 函数重名冲突

# 环境

- Dart 2.1.0

# 类多继承

```dart
void main() {
  var xm = Xiaomi();
  xm.startup();
  xm.shutdown();
  xm.call();
  xm.sms();
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

class AndroidSystem {
  void call() {
    print('android call');
  }
}

class Weixin {
  void sms() {
    print('weixin sms');
  }
}

class Xiaomi extends AndroidPhone with AndroidSystem, Weixin {
  void startup() {
    super.startup();
    print('AndroidPhone 开机');
  }
}
```

> 采用 `with ... , .... , ...` 方式 mixin 入多个类功能

# 函数重名冲突

```dart
void main() {
  var xm = Xiaomi();
  xm.startup();
  xm.shutdown();
  xm.sms();
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

class AndroidSystem {
  void call() {
    print('android call');
  }
}

class Weixin {
  void sms() {
    print('weixin sms');
  }
}

class QQ {
  void sms() {
    print('qq sms');
  }
}

class Xiaomi extends AndroidPhone with AndroidSystem, Weixin, QQ {
  void startup() {
    super.startup();
    print('AndroidPhone 开机');
  }
}
```

> 遇到相同功能的函数，最后载入的会覆盖之前的函数定义

# 代码

- [mixin](https://github.com/ducafecat/dart-learn/tree/master/25-%E5%A4%9A%E7%B1%BB%E7%BB%A7%E6%89%BF)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
