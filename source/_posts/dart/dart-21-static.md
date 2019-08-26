---
title: Dart语言学习 - 21 静态成员
date: 2018-11-17 16:02:07
tags: dart
categories: Dart语言学习
---

# 本节目标

- 静态变量
- 静态方法

# 环境

- Dart 2.0.0

# 静态变量

## static 定义

声明

```dart
class People {
  static String name = 'ducafecat';
}
```

调用

静态变量可以通过外部直接访问,不需要将类实例化

```dart
print(People.name);
```

## 函数内部访问

实例化后的类也可以访问该静态变量

声明

```dart
class People {
  static String name = 'ducafecat';
  void show() {
    print(name);
  }
}
```

调用

```dart
var p = new People();
p.show();
```

## 不能用 this

因为静态变量实际上存在于类中,而不是实例本身

```dart
class People {
  static String name = 'ducafecat';
  void show() {
    print(this.name);
  }
}
```

# 静态方法

静态方法可以通过外部直接访问

声明

```dart
class People {
  static String name = 'ducafecat';
  static void printName() {
    print(name);
  }
}
```

调用

```dart
People.printName();
```

# 总结

- 实例化后将无法通过外部直接调用 static 成员
- 静态成员与实例成员是分开的, 静态成员处于类的定义体中, 实例成员处于类的实例中

# 代码

- [static.dart](https://github.com/ducafecat/dart-learn/blob/master/19-类/static.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
