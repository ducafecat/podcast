---
title: Dart语言学习 - 09 日期时间
date: 2018-10-18 15:02:05
tags: dart
categories: Dart语言学习
---

# 本节目标

- 声明
- UTC 时间
- 公元时间
- 时间戳
- 解析标准时间
- 时间运算

# 环境

- Dart 2.0.0

# 声明

```dart
var now = new DateTime.now();
print(now);
var d = new DateTime(2018, 10, 10, 9, 30);
print(d);
```

# 创建时间 UTC

- [UTC 协调世界时](https://zh.wikipedia.org/wiki/%E5%8D%8F%E8%B0%83%E4%B8%96%E7%95%8C%E6%97%B6)
- [原子时](https://zh.wikipedia.org/wiki/%E5%8E%9F%E5%AD%90%E6%97%B6)
- [原子钟](https://zh.wikipedia.org/wiki/%E5%8E%9F%E5%AD%90%E9%90%98)

```dart
var d = new DateTime.utc(2018, 10, 10, 9, 30);
print(d);
```

# 解析时间 IOS 8601

- [ISO 8601](https://zh.wikipedia.org/wiki/ISO_8601)
- [时区](https://zh.wikipedia.org/wiki/%E6%97%B6%E5%8C%BA)
- [时区列表](https://zh.wikipedia.org/wiki/%E6%97%B6%E5%8C%BA%E5%88%97%E8%A1%A8)

```dart
var d1 = DateTime.parse('2018-10-10 09:30:30Z');
print(d1);
var d2 = DateTime.parse('2018-10-10 09:30:30+0800');
print(d2);
```

# 时间增减量

```dart
var d1 = DateTime.now();
print(d1);
print(d1.add(new Duration(minutes: 5)));
print(d1.add(new Duration(minutes: -5)));
```

# 比较时间

```dart
var d1 = new DateTime(2018, 10, 1);
var d2 = new DateTime(2018, 10, 10);
print(d1.isAfter(d2));
print(d1.isBefore(d2));
var d1 = DateTime.now();
var d2 = d1.add(new Duration(milliseconds: 30));
print(d1.isAtSameMomentAs(d2));
```

# 时间差

```dart
var d1 = new DateTime(2018, 10, 1);
var d2 = new DateTime(2018, 10, 10);
var difference = d1.difference(d2);
print([difference.inDays, difference.inHours]);
```

# 时间戳

- [公元](https://zh.wikipedia.org/wiki/%E5%85%AC%E5%85%83)

```dart
var now = new DateTime.now();
print(now.millisecondsSinceEpoch);
print(now.microsecondsSinceEpoch);
```

# 代码

- [datetime.dart](https://github.com/ducafecat/dart-learn/blob/master/09-%E6%97%A5%E6%9C%9F%E6%97%B6%E9%97%B4/datetime.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [DateTime](https://api.dartlang.org/stable/2.0.0/dart-core/DateTime-class.html)
- [UTC 协调世界时](https://zh.wikipedia.org/wiki/%E5%8D%8F%E8%B0%83%E4%B8%96%E7%95%8C%E6%97%B6)
- [原子时](https://zh.wikipedia.org/wiki/%E5%8E%9F%E5%AD%90%E6%97%B6)
- [原子钟](https://zh.wikipedia.org/wiki/%E5%8E%9F%E5%AD%90%E9%90%98)
- [ISO 8601](https://zh.wikipedia.org/wiki/ISO_8601)
- [时区](https://zh.wikipedia.org/wiki/%E6%97%B6%E5%8C%BA)
- [时区列表](https://zh.wikipedia.org/wiki/%E6%97%B6%E5%8C%BA%E5%88%97%E8%A1%A8)
- [公元](https://zh.wikipedia.org/wiki/%E5%85%AC%E5%85%83)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
