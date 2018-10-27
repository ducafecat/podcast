---
title: Dart语言学习 14 symbol、enum、comments
date: 2018-10-27 11:05:15
tags: dart
categories: Dart语言学习
---

# 本节目标

- symbol
- enum
- comments

# 环境

- Dart 2.0.0

# 符号 Symbol

Dart语言的标识符，在反射中用的很普及，特别是很多发布包都是混淆后的。

```dart
import 'dart:mirrors';

Symbol libraryName = new Symbol('dart.core');
MirrorSystem mirrorSystem = currentMirrorSystem();
LibraryMirror libMirror = mirrorSystem.findLibrary(libraryName);
libMirror.declarations.forEach((s, d) => print('$s - $d'));
```

# 枚举 Enum

适合用在常量定义，类型比较很方便。

```dart
enum Status { none, running, stopped, paused }

Status.values.forEach((it) => print('$it - index: ${it.index}'));
```

# 注释 Comments

## 单行注释

```dart
// Symbol libraryName = new Symbol('dart.core');
```

## 多行注释

```dart
  /*
   * Symbol
   * 
  Symbol libraryName = new Symbol('dart.core');
  MirrorSystem mirrorSystem = currentMirrorSystem();
  LibraryMirror libMirror = mirrorSystem.findLibrary(libraryName);
  libMirror.declarations.forEach((s, d) => print('$s - $d')); 
  */
```

## 文档注释

```dart
/// `main` 函数
///
/// 符号
/// 枚举
///
void main() {
  ...
}
```

可参考 `String` 类中的注释使用

# 代码

- [symbol-enum-comments.dart](https://github.com/ducafecat/dart-learn/tree/master/14-symbol-enum-comments)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [Symbol](https://api.dartlang.org/stable/2.0.0/dart-core/Symbol-class.html)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
