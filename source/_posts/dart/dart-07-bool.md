---
title: Dart语言学习 - 07 布尔
date: 2018-10-17 17:37:31
tags: dart
categories: Dart语言学习
---

# 本节目标

- 布尔 声明、比较、默认值
- 断言、asset、isEmpty、isNaN
- 逻辑操作符 &&、||、!
- 关系运算符 == != > >= < <=

# 环境

- Dart 2.0.0

# 声明

为了代表布尔值，Dart 有一个名字为 bool 的类型。 只有两个对象是布尔类型的：true 和 false 所创建的对象， 这两个对象也都是编译时常量。

[bool](https://api.dartlang.org/stable/2.0.0/dart-core/bool-class.html)

```dart
bool a;
print(a);
```

只有 true 对象才被认为是 true。 所有其他的值都是 flase。

```dart
String name = 'ducafecat';
if(name) {
  print('this is name');
}
```

# assert 断言

```dart
var a = true;
assert(a);

var name = '';
assert(name.isEmpty);
assert(name.isNotEmpty);

var num = 0 / 0;
assert(num.isNaN);
```

# 逻辑运算符

## `&&` 逻辑与

```dart
bool a = true;
bool b = true;
assert(a && b);
```

## `||` 逻辑或

```dart
bool a = true;
bool b = false;
assert(a || b);
```

## `!` 逻辑非

```dart
bool a = true;
bool b = !a;
print(b);
```

# 关系运算符

## `==` 等于

```dart
if(a == b) {}
```

## `!=` 不等于

```dart
if(a != b) {}
```

## `>` 大于

```dart
if(a > b) {}
```

## `>=` 大于或等于

```dart
if(a >= b) {}
```

## `<` 小于

```dart
if(a < b) {}
```

## `<=` 小于或等于

```dart
if(a <= b) {}
```

# 代码

- [bool.dart](https://github.com/ducafecat/dart-learn/blob/master/07-%E5%B8%83%E5%B0%94/bool.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [bool](https://api.dartlang.org/stable/2.0.0/dart-core/bool-class.html)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
