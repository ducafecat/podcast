---
title: Dart语言学习 - 26 工厂函数
date: 2018-12-04 14:33:01
tags: dart
categories: Dart语言学习
---

# 本节目标

- 工厂函数
- 工厂构造函数

# 环境

- Dart 2.1.0

# 工厂函数

简化类型实例化

```dart
void main() {
  var xm = phoneFactory('ios');
  xm.startup();
  xm.shutdown();
}

class Phone {
  void startup() {
    print('开机');
  }
  void shutdown() {
    print('关机');
  }
}

Phone phoneFactory(String name) {
  switch (name) {
    case 'android':
      return new AndroidPhone();
      break;
    default:
      return new IOSPhone();
  }
}

class AndroidPhone extends Phone {
  void startup() {
    super.startup();
    print('Android Phone 开机');
  }
}

class IOSPhone extends Phone {
  void startup() {
    super.startup();
    print('IOS Phone 开机');
  }
}

```

# 工厂构造函数

```dart
void main() {
  var xm = Phone('android');
  xm.startup();
  xm.shutdown();
}

abstract class Phone {
  factory Phone(String name) {
    switch (name) {
      case 'android':
        return new AndroidPhone();
        break;
      default:
        return new IOSPhone();
    }
  }
  void startup();
  void shutdown();
}

class AndroidPhone implements Phone {
  void startup() {
    print('Android Phone 开机');
  }
  void shutdown() {
    print('Android 关机');
  }
}

class IOSPhone implements Phone {
  void startup() {
    print('IOS Phone 开机');
  }
  void shutdown() {
    print('IOS 关机');
  }
}

```

# 代码

- [factory](https://github.com/ducafecat/dart-learn/tree/master/26-%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
