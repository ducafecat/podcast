---
title: Dart语言学习 - 35 - 代码分格 effective style
date: 2019-01-28 14:43:12
tags: dart
categories: Dart语言学习
---

# 本节目标

- 代码分格

# 环境

- Dart 2.1.0

# 使用 UpperCamelCase 风格来命名类型名称

```dart
class SliderMenu { ... }

class HttpRequest { ... }

typedef bool Predicate<T>(T value);
```

# 使用 lowercase_with_underscores 风格来命名库和文件名名字

```dart
library peg_parser.source_scanner;

import 'file_system.dart';
import 'slider_menu.dart';
```

# 使用 lowercase_with_underscores 风格命名导入的前缀

```dart
import 'dart:json' as json;
import 'dart:math' as math;
import 'package:javascript_utils/javascript_utils.dart' as js_utils;
import 'package:js/js.dart' as js;
```

# 使用 lowerCamelCase 风格来命名其他的标识符

```dart
var item;

HttpRequest httpRequest;

align(clearItems) {
  // ...
}
```

# 使用 lowerCamelCase 来命名常量

```dart
const pi = 3.14;
const defaultTimeout = 1000;
final urlScheme = new RegExp('^([a-z]+):');

class Dice {
  static final numberGenerator = new Random();
}
```

# 把 “dart:” 导入语句放到其他导入语句之前

```dart
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
```

# 把 “package:” 导入语句放到相对导入语句之前

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'a.dart';
```

# 把”第三方” “package:” 导入语句放到其他语句之前。

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'package:myapp/io.dart';
import 'package:myapp/util.dart';
```

# 把导出（export）语句放到所有导入语句之后的部分

```dart
import 'src/error.dart';
import 'src/string_source.dart';

export 'src/error.dart';
```

# 按照字母顺序来排序每个部分中的语句

```dart
import 'package:bar/bar.dart';
import 'package:foo/bar.dart';

import 'a.dart';
import 'a/b.dart';
```

# 在所有的控制结构上使用大括号

```dart
if (true) {
  print('sanity');
} else {
  print('opposite day!');
}
```

# 当只有 if 语句没有 else 语句并且 所有语句可以放到一行的时候，可以省略大括号

```dart
if (arg == null) return defaultValue;
```

# 通常用于当条件满足的时候就跳出 if 或者 返回的情况。 但是对于其他表达式，如果可以放到一行中， 也可以这样使用

```dart
if (parameter == null) parameter = defaultValue;
```

# 在每个语句或者声明后面添加一个空行

```dart
main() {
  first(statement);
  second(statement);
}

anotherDeclaration() { ... }
```

# 在关键字 operator 后面添加一个空格

```dart
bool operator ==(other) => ...;
```

# 在二元和三元操作符之间添加空格

```dart
average = (a + b) / 2;
largest = a > b ? a : b;
if (obj is! SomeType) print('not SomeType');
optional([parameter = defaultValue]) { ... }
```

# 不要 在一元操作符前后添加空格

```dart
!condition
index++
```

# 把开始的大括号 ({) 放到同一行上

```dart
class Foo {
  method() {
    if (true) {
      // ...
    } else {
      // ...
    }
  }
}
```

# 在函数和方法体的 { 之前添加一个空格

```dart
getEmptyFn(a) {
  return () {};
}
```

# 把三元操作符放到多个表达式的下一行开始位置

```dart
return someCondition
    ? whenTrue
    : whenFalse;
```

# 把 . 放到下一行开头当表达式换行的时候

```dart
someVeryLongVariable.withAVeryLongProperty
    .aMethodOnThatObject();
```

# 把构造函数初始化列表中的每个参数和值都放到同一行

```dart
MyClass()
    : firstField = 'some value',
      secondField = 'another',
      thirdField = 'last' {
  // ...
}
```

# 当无法在一行写完集合的时候，把每个元素都用集合定义的方式来表达

```dart
mapInsideList([
  {
    'a': 'b',
    'c': 'd'
  },
  {
    'a': 'b',
    'c': 'd'
  },
]);
```

# 用两个空格来缩进代码块和集合体

```dart
if (condition) {
  print('hi');

  [
    long,
    list,
    literal
  ];
}
```

# 缩进 switch case 两个空格， case 体四个空格

```dart
switch (fruit) {
  case 'apple':
    print('delish');
    break;

  case 'durian':
    print('stinky');
    break;
}
```

# 只少使用两个空格来缩进多行函数级联调用

```dart
buffer
  ..write('Hello, ')
  ..write(name)
  ..write('!');
```

# 使用四个空格来缩进同一行的换行

```dart
someLongObject.aReallyLongMethodName(longArg, anotherLongArg,
    wrappedToNextLine);

bobLikes() =>
    isDeepFried || (hasPieCrust && !vegan) || containsBacon;
```

# 当表达式包含多行函数或者 集合声明定义的时候除外

```dart
new Future.delayed(const Duration(seconds: 1), () {
  print('I am a callback');
});

args.addAll([
  '--mode',
  'release',
  '--checked'
]);
```

# 参考

- [style](https://www.dartlang.org/guides/language/effective-dart/style)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
