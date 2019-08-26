---
title: Dart语言学习 - 19 类
date: 2018-11-17 13:52:30
tags: dart
categories: Dart语言学习
---

# 本节目标

- 定义、使用类
- 构造函数
- 简化构造
- 初始化列表
- 命名构造函数
- 重定向构造函数

# 环境

- Dart 2.0.0

# 定义、使用类

定义

```dart
class Point {
}
```

使用

```dart
var p = new Point();
```

# 构造函数

定义

```dart
class Point {
  num x;
  num y;
  Point(num x, num y){
    this.x = x;
    this.y = y;
  }
}
```

使用

```dart
var p = new Point(1, 2);
print([p.x, p.y]);
```

# 简化构造

定义

```dart
class Point {
  num x;
  num y;
  Point(this.x, this.y);
}
```

使用

```dart
var p = new Point(1, 2);
print([p.x, p.y]);
```

# 初始化列表

定义

```dart
class Point {
  num x;
  num y;
  var origin;
  Point(this.x, this.y): origin = {x:x, y:y};
}
```

使用

```dart
var p = new Point(1, 2);
print([p.x, p.y, p.origin]);
```

# 命名构造函数

定义

```dart
class Point {
  num x;
  num y;
  Point.fromJson(Map json) {
    x = json['x'];
    y = json['y'];
  }
}
```

使用

```dart
var p = new Point.fromJson({"x": 1, "y": 2});
print([p.x, p.y]);
```

# 重定向构造函数

定义

```dart
class Point {
  num x;
  num y;
  Point(this.x, this.y);
  Point.fromJson(Map json) : this(json['x'], json['y']);
}
```

使用

```dart
var p = new Point.fromJson({"x": 1, "y": 2});
print([p.x, p.y]);
```

# 代码

- [class.dart](https://github.com/ducafecat/dart-learn/blob/master/19-类/class.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
