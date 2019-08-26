---
title: Dart语言学习 - 36 - 代码分格 文档 effective-documentation
date: 2019-01-28 15:15:00
tags: dart
categories: Dart语言学习
---

# 本节目标

- 文档分格

# 环境

- Dart 2.1.0

# 按照句子的格式来格式化评论

```dart
// Not if there is nothing before it.
if (_chunks.isEmpty) return false;
```

如果第一个单词不是大小写相关的标识符，则首字母要大写。使用标点符号结尾 （句号、感叹号、问号）。对于所有的注释都是这样要求的：文档注释、 行内注释、甚至 TODO 注释。即使是一句话的一半也需要如此。

# 使用块注释作为解释说明

```dart
greet(name) {
  // Assume we have a valid name.
  print('Hi, $name!');
}
```

# 使用 /// 文档注释来注释成员和类型

```dart
/// The number of characters in this chunk when unsplit.
int get length => ...
```

# 把第一个语句定义为一个段落

```dart
/// Defines a flag.
///
/// Throws an [ArgumentError] if there is already an option named [name] or
/// there is already an option using abbreviation [abbr]. Returns the new flag.
Flag addFlag(String name, String abbr) { ... }
```

注释文档中的第一个段落应该是简洁的、面向用户的注释。例如下面的示例， 通常不是一个完成的语句。

# 用第三人称来开始函数或者方法的文档注释

```dart
/// Returns `true` if every element satisfies the [predicate].
bool all(bool predicate(T element)) { ... }

/// Starts the stopwatch if not already running.
void start() { ... }
```

# 使用名词短语来开始变量、getter、setter 的注释

```dart
/// The current day of the week, where `0` is Sunday.
int weekday;

/// The number of checked buttons on the page.
int get checkedCount { ... }
```

注释文档应该表述这个属性是什么。虽然 getter 函数会做些计算， 但是也要求这样，调用者关心的是其结果而 不是如何计算的

# 使用名词短语来开始库和类型注释

```dart
/// A chunk of non-breaking output text terminated by a hard or soft newline.
///
/// ...
class Chunk { ... }
```

在程序中，类的注释通常是最重要的文档。 类的注释描述了类型的不变性、介绍其使用的术语、 提供类成员使用的上下文信息。为类提供一些注释可以让 其他类成员的注释更易于理解和编写。

# 在文档注释中添加示例代码

```dart
/// Returns the lesser of two numbers.
///
///     min(5, 3); // 3.
num min(num a, num b) { ... }
```

人类非常擅长从示例中抽象出实质内容，所以即使提供 一行最简单的示例代码都可以让 API 更易于理解。

# 而 Dart 把参数、返回值等描述放到文档注释中，并使用方括号来引用 以及高亮这些参数和返回值

```dart
/// Defines a flag.
///
/// Throws an [ArgumentError] if there is already an option named [name] or
/// there is already an option using abbreviation [abbr]. Returns the new flag.
Flag addFlag(String name, String abbr) { ... }
```

# 把注释文档放到注解之前

```dart
/// _Deprecated: Use [newMethod] instead._
@deprecated
oldMethod();
```

# 使用 “this” 而不是 “the” 来引用实例成员

```dart
class Box {
  /// The value this wraps.
  var _value;

  /// True if this box contains a value.
  bool get hasValue => _value != null;
}
```

# 参考

- [documentation](https://www.dartlang.org/guides/language/effective-dart/documentation)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
