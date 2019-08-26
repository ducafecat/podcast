---
title: Dart语言学习 - 37 代码分格 最佳实践 effective-usage
date: 2019-01-28 15:30:46
tags: dart
categories: Dart语言学习
---

# 本节目标

- 最佳实践

# 环境

- Dart 2.1.0

# 使用相邻的字符串字面量定义来链接字符串

```dart
raiseAlarm(
    'ERROR: Parts of the spaceship are on fire. Other '
    'parts are overrun by martians. Unclear which are which.');
```

# 使用插值的形式来组合字符串和值

```dart
'Hello, $name! You are ${year - birth} years old.';
```

# 避免在字符串插值中使用多余的大括号

```dart
'Hi, $name!'
"Wear your wildest $decade's outfit."
'Wear your wildest ${decade}s outfit.'
```

# 尽可能的使用集合字面量来定义集合

```dart
var points = [];
var addresses = {};
```

# 如果有必要还可以提供泛型类型

```dart
var points = <Point>[];
var addresses = <String, Address>{};
```

# 不要 使用 .length 来判断集合是否为空

```dart
if (lunchBox.isEmpty) return 'so hungry...';
if (words.isNotEmpty) return words.join(' ');
```

# 使用高阶（higher-order）函数来转换集合数据

```dart
var aquaticNames = animals
    .where((animal) => animal.isAquatic)
    .map((animal) => animal.name);
```

# 避免 在 Iterable.forEach() 中使用函数声明形式

```dart
for (var person in people) {
  ...
}
```

forEach() 方法通常在 JavaScript 中使用，原因是系统内置的 for-in 循环并不能提供期望的结果。 相反，在 Dart 中如果需要遍历一个集合，通常使用循环语句

# 如果你只想在每个集合元素上调用一个已经定义好的函数，则可以使用 forEach() 函数

```dart
people.forEach(print);
```

# 要 用方法声明的形式来给方法起个名字

- 正确

```dart
void main() {
  localFunction() {
    ...
  }
}
```

- 错误示范

```dart
void main() {
  var localFunction = () {
    ...
  };
}
```

# 不要 显式的把变量初始化为 null

```dart
int _nextId;

class LazyId {
  int _id;

  int get id {
    if (_nextId == null) _nextId = 0;
    if (_id == null) _id = _nextId++;

    return _id;
  }
}
```

在 Dart 中没有初始化的变量和域会自动的 初始化为 null。在语言基本就保证了该行为的可靠性。 在 Dart 中没有 “未初始化的内存”这个概念。所以添加 = null 是多余的。

# 不要 创建没必要的 getter 和 setter

```dart
```

# 段落 1

- 正确

```dart
class Box {
  var contents;
}
```

- 错误

```dart
class Box {
  var _contents;
  get contents => _contents;
  set contents(value) {
    _contents = value;
  }
}
```

# 推荐 使用 final 关键字来限定只读属性

```dart
class Box {
  final contents = [];
}
```

# 考虑 用 => 来实现只有一个单一返回语句的函数

```dart
get width => right - left;
bool ready(num time) => minTime == null || minTime <= time;
containsValue(String value) => getValues().contains(value);
```

# 要 尽可能的在定义变量的时候初始化其值

```dart
class Folder {
  final String name;
  final List<Document> contents = [];

  Folder(this.name);
  Folder.temp() : name = 'temporary';
}
```

# 要 尽可能的使用初始化形式

```dart
class Point {
  num x, y;
  Point(this.x, this.y);
}
```

# 要 把 super() 调用放到构造函数初始化列表之后调用

```dart
View(Style style, List children)
    : _children = children,
      super(style) {
```

# 要 使用 rethrow 来重新抛出捕获的异常

```dart
try {
  somethingRisky();
} catch(e) {
  if (!canHandle(e)) rethrow;
  handle(e);
}
```

# 推荐 使用 async/await 而不是直接使用底层的特性

```dart
Future<bool> doAsyncComputation() async {
  try {
    var result = await longRunningCalculation();
    return verifyResult(result.summary);
  } catch(e) {
    log.error(e);
    return false;
  }
}
```

# 不要 在没有有用效果的情况下使用 async

```dart
Future afterTwoThings(Future first, second) {
  return Future.wait([first, second]);
}
```

# 参考

- [usage](https://www.dartlang.org/guides/language/effective-dart/usage)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
