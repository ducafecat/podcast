---
title: Dart语言学习 - 05 常量
date: 2018-10-10 14:49:26
tags: dart
categories: Dart语言学习
---

# 本节目标

- 常量的定义方式
- `final` `const` 的区别

# 环境

- Dart 2.0.0

# 定义

## 类型声明可以省略

```dart
final String a = 'ducafecat';
final a = 'ducafecat';

const String a = 'ducafecat';
const a = 'ducafecat';
```

## 初始后不能再赋值

```dart
final a = 'ducafecat';
a = 'abc';

const a = 'ducafecat';
a = 'abc';
```

## 不能和 var 同时使用

```dart
final var a = 'ducafecat';
const var a = 'ducafecat';
```

## const 赋值 申明可省略

```dart
const List ls = const [11, 22, 33];
const List ls = [11, 22, 33];
```

# 区别

## 需要确定的值

```dart
final dt = DateTime.now();

const dt = const DateTime.now();
```

## 不可变性可传递

```dart
final List ls = [11, 22, 33];
ls[1] = 44;

const List ls = [11, 22, 33];
ls[1] = 44;
```

## 内存中重复创建

```dart
final a1 = [11 , 22];
final a2 = [11 , 22];
print(identical(a1, a2));

const a1 = [11 , 22];
const a2 = [11 , 22];
print(identical(a1, a2));
```

# 代码

- [constant.dart](https://github.com/ducafecat/dart-learn/blob/master/05-%E5%B8%B8%E9%87%8F/constant.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
