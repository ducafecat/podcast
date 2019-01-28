---
title: Dart语言学习 - 38 代码分格 API 设计 effective-design
date: 2019-01-28 16:44:13
tags: dart
categories: Dart语言学习
---

# 本节目标

- API 设计

# 环境

- Dart 2.1.0

# 要 使用一致的术语

```dart
pageCount         // 一个成员变量
updatePageCount() // 和 pageCount 名字一致。
toSomething()     // 和 Iterable 的 toList() 一致。
asSomething()     // 和 List 的 asMap() 一致。
Point             // 广为人知的概念。
```

# 避免 缩写

```dart
pageCount
buildRectangles
IOStream
HttpRequest
```

# 推荐 把最具描述性的名词放到最后

```dart
pageCount             // A count (of pages).
ConversionSink        // A sink for doing conversions.
ChunkedConversionSink // A ConversionSink that's chunked.
CssFontFaceRule       // A rule for font faces in CSS.
```

# 考虑 尽量让代码看起来像普通的句子

```dart
// "If errors is empty..."
if (errors.isEmpty) ...

// "Hey, _subscription, cancel!"
_subscription.cancel();

// "Get the monsters where the monster has claws."
monsters.where((monster) => monster.hasClaws);
```

# 推荐 使用非命令式动词短语命名布尔类型的变量和属性

```dart
isEmpty
hasElements
canClose
closesWindow
canShowPopup
hasShownPopup
```

# 考虑 省略命名布尔参数的动词

```dart
Isolate.spawn(entryPoint, message, paused: false)
new List.from(elements, growable: true)
new RegExp(pattern, caseSensitive: false)
```

# 推荐 使用命令式动词短语来命名带有副作用的函数或者方法

```dart
list.add()
queue.removeFirst()
window.refresh()
connection.downloadData()
```

# 考虑 使用名词短语或者非命令式动词短语命名返回数据为主要功能的方法或者函数

```dart
list.elementAt(3)
string.codeUnitAt(4)
```

# 推荐 使用 to___() 来命名把对象的状态转换到一个新的对象的函数

```dart
list.toSet()
stackTrace.toString()
dateTime.toLocal()
```

# 使用 as___() 来命名把原来对象转换为另外一种表现形式的函数

```dart
list.asMap()
bytes.asFloat32List()
subscription.asFuture()
```

# 避免 在方法或者函数名称中描述参数

```dart
list.add(element)
map.remove(key)
```

# 避免 定义使用简单的方法可以替代的只有一个成员的抽象类

和 Java 不同的是， Dart 支持一等方法（first-class functions）、闭包和优雅的语法来使用它们。 如果你需要的只是一个回调函数，使用方法即可。 如果你定义了一个类，里面只有一个名字无意义的函数， 例如 call 或者 invoke， 这种情况最好用方法替代

```dart
typedef bool Predicate(item);
```

# 避免 定义只包含静态成员的类

```dart
DateTime mostRecent(List<DateTime> dates) {
  return dates.reduce((a, b) => a.isAfter(b) ? a : b);
}

const _favoriteMammal = 'weasel';
```

# 然后，这条规则并不是强制的。对于一些常量或者枚举型的类型， 使用类来把相关的成员组织到一起可能也是合理的。当然， 使用库也是同样合理的。

```dart
class Color {
  static const red = '#f00';
  static const green = '#0f0';
  static const blue = '#00f';
  static const black = '#000';
  static const white = '#fff';
}
```

# 推荐 使用构造函数而不是静态函数来创建对象

```dart
class Point {
  num x, y;
  Point(this.x, this.y);
  Point.polar(num theta, num radius)
      : x = radius * math.cos(theta),
        y = radius * math.sin(theta);
}
```

# 要 使用 getter 来定义访问属性的操作

如果函数的名字带有 get 前缀，或者是一个像 length 或者 size 这样 的名称，这种情况通常最好定义该函数为一个 getter。 当全部满足下面的条件的时候，你应该使用一个 getter：

没有参数。
返回一个值
没有副作用 调用一个 getter 不应该改变对象外部可见的状态 (内部缓存和延时初始化的状态可以发生变化) 如果对象的状态在多次调用同一个 getter 之间没有发生变化，则 多次调用同一个 getter 应该返回同一个值

```dart
rectangle.width
collection.isEmpty
button.canShow
```

# 要 对于本质上为修改对象属性的函数要使用 setter

```dart
rectangle.width = 3;
button.visible = false;
```

# 不要 为 setter 指定返回类型

```dart
set foo(Foo value) {...}
```

# 推荐 为私有成员提供类型

在公开的 API 上使用类型可以帮助使用你的库的用户。同样， 是私有代码上使用类型，可以帮助你的你的同事或者代码维护者。 另外，在私有成员上使用类型，对于将来自己查看代码 也有帮助。

```dart
class CallChainVisitor {
  final SourceVisitor _visitor;
  final Expression _target;

  void _writeCall(Expression call) { ... }

  ...
}
```

# 避免 在方法表达式上使用类型

```dart
var names = people.map((person) => person.name);
```

# 避免 在没必要的地方使用 dynamic 类型

在大部分 Dart 代码中，类型可以忽略，这样该参数类型会自动设置为 dynamic。 所以没必要手动指定类型为 dynamic 的， 只需要省略类型即可。

```dart
lookUpOrDefault(String name, Map map, defaultValue) {
  var value = map[name];
  if (value != null) return value;
  return defaultValue;
}
```

# 避免 使用 Function 类型

- 正确

```dart
bool isValidString(String value, bool predicate(String string)) { ... }
```

- 错误

```dart
bool isValidString(String value, Function predicate) { ... }
```

# 要 使用 Object 来替代 dynamic 来表示可以接受任意对象

```dart
// Accepts any object.
void log(Object object) {
  print(object.toString());
}

// Only accepts bool or String, which can't be expressed in a type annotation.
bool convertToBool(arg) {
  if (arg is bool) return arg;
  if (arg is String) return arg == 'true';
  throw new ArgumentError('Cannot convert $arg to a bool.');
}
```

# 考虑使用命名参数或者命名构造函数以及命名常量来清晰 的表明您的意图：

```dart
new Task.oneShot();
new Task.repeating();
new ListBox(scroll: true, showScrollbars: true);
new Button(ButtonState.enabled);
```

# 对于 setter 则没有这个要求，应为 setter 的名字已经明确的 表明了值所代表的意义

```dart
listBox.canScroll = true;
button.isEnabled = false;
```

# 避免 把用户想要忽略的参数放到位置可选参数的前列

```dart
String.fromCharCodes(Iterable<int> charCodes, [int start = 0, int end])

DateTime(int year,
    [int month = 1,
    int day = 1,
    int hour = 0,
    int minute = 0,
    int second = 0,
    int millisecond = 0,
    int microsecond = 0])

Duration(
    {int days: 0,
    int hours: 0,
    int minutes: 0,
    int seconds: 0,
    int milliseconds: 0,
    int microseconds: 0})
```

# 避免 使用强制无意义的参数

```dart
string.substring(start)
```

# 要 使用包含开始位置并且不包含结束位置的范围参数

如果你定义一个函数或者方法让用户从基于位置排序的集合中 选择一些元素，需要一个开始位置索引和结束位置索引分别制定开始 元素的位置以及结束元素的位置。结束位置通常是指 大于最后一个元素的位置的值。

核心库就是这样定义的，所以最好和核心库保持一致

```dart
[0, 1, 2, 3].sublist(1, 3) // [1, 2].
'abcd'.substring(1, 3)     // "bc".
```

# 不要 在自定义 == 操作符中判断 null

语言规范表明了这种判断已经自动执行了，你的 == 自定义操作符只有当 右侧对象不为 null 的时候才会执行。

- 正确

```dart
class Person {
  final String name;

  operator ==(other) =>
      other is Person && name == other.name;
}
```

- 错误

```dart
class Person {
  final String name;

  operator ==(other) =>
      other != null &&
      other is Person &&
      name == other.name;
}
```

# 参考

- [design](https://www.dartlang.org/guides/language/effective-dart/design)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
