---
title: Dart语言学习 - 28 泛型
date: 2018-12-05 11:30:26
tags: dart
categories: Dart语言学习
---

# 本节目标

- 使用泛型
- 定义泛型
- 限制泛型

# 环境

- Dart 2.1.0

# 泛型使用

```dart
main(List<String> args) {
  var l = List<String>();
  l.add('aaa');
  l.add('bbb');
  l.add('ccc');
  print(l);

  var m = Map<int, String>();
  m[1] = 'aaaa';
  m[2] = 'bbbb';
  m[3] = 'cccc';
  print(m);
}
```

> 很多的容器对象，在创建对象时都可以定义泛型类型。

# 泛型函数

```dart
main(List<String> args) {
  var key = addCache('a00001', 'val.....');
  print(key);
}

K addCache<K, V>(K key, V val) {
  print('${key} ${val}');
  return key;
}
```

> 泛型可以用在一个函数的定义

# 构造函数泛型

```dart
main(List<String> args) {
  var p = Phone<String>('abc00000011111');
  print(p.mobileNumber);
}

class Phone<T> {
  final T mobileNumber;
  Phone(this.mobileNumber);
}
```

> 这是大多数情况下使用泛型的场景，在一个类的构造函数中

# 泛型限制

```dart
main(List<String> args) {
  var ad = AndroidPhone();
  var p = Phone<AndroidPhone>(ad);
  p.mobile.startup();
}

class Phone<T extends AndroidPhone > {
  final T mobile;
  Phone(this.mobile);
}

class AndroidPhone {
  void startup() {
    print('Android Phone 开机');
  }
}
```

> 通过 extends 关键字 可以限定你可以泛型使用的类型

# 代码

- [泛型](https://github.com/ducafecat/dart-learn/tree/master/28-%E6%B3%9B%E5%9E%8B)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
