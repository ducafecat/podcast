---
title: Flutter Getx 02 - 空安全、更健全的代码
date: 2021-4-9 00:00:00
tags: flutter getx
categories: Flutter Getx
---

![](2021-04-09-14-52-51.png)

# 本节目标

- 空安全意味着什么
- 如何迁移代码
- 如何禁用空安全
- 代码规范示例

## 视频

https://www.bilibili.com/video/bv1g5411c7hF

## 代码

https://github.com/ducafecat/getx_quick_start

## 参考

- https://dart.cn/null-safety
- https://dart.cn/null-safety/migration-guide
- https://dart.cn/null-safety/understanding-null-safety
- https://dart.cn/null-safety/unsound-null-safety
- https://dart.cn/null-safety/faq

## 正文

### 空安全意味着什么

- 默认不可空

```dart
String title = 'ducafecat';
```

- `type?` 操作符

```dart
String? title = null;
```

- `value!` 操作符

```dart
String? title = 'ducafecat';
String newTitle = title!;
```

- `value?` 操作符

```dart
String? title = 'ducafecat';
bool isEmpty = title?.isEmpty();
```

- `value??` 操作符

```dart
String? title = 'ducafecat';
String newTitle = title ?? 'cat';
```

- `late` 会在运行时检查。所以请您仅在确定它被使用前一定会被初始化的情况下使用

```dart
late String? title;
title = 'ducafecat';
```

- List、泛型

| 类型           | 集合是否可空 | 数据项是否可空 |
| -------------- | ------------ | -------------- |
| List<String>   | no           | no             |
| List<String>?  | yes          | no             |
| List<String?>  | no           | yes            |
| List<String?>? | yes          | yes            |

- Map

| 类型               | 集合是否可空 | 数据项是否可空 |
| ------------------ | ------------ | -------------- |
| Map<String, int>   | no           | no\*           |
| Map<String, int>?  | yes          | no\*           |
| Map<String, int?>  | no           | yes            |
| Map<String, int?>? | yes          | yes            |

> `*` 可能返回空

```dart
// 有可能返回 null
int value = <String, int>{'one': 1}['one']; // ERROR

// 需要加上 type?
int? value = <String, int>{'one': 1}['one']; // OK

// 或者 value!
int value = <String, int>{'one': 1}['one']!; // OK

```

### 带来的好处

- 代码更健康
- 用户体验好
- 运行更快
- 编译文件更小

### 开启和迁移

- pubspec.yaml

```yarn
environment:
  sdk: ">=2.12.0 <3.0.0"
```

- 迁移顺序

我们强烈建议您按顺序迁移代码，先迁移依赖关系中的处于最末端的依赖。例如，如果 C 依赖了 B，B 依赖了 A，那么应该按照 A -> B -> C 的顺序进行迁移。

![](2021-04-09-11-16-14.png)

- 检查依赖项目

```
# Dart 版本是否为 2.12 或更高
> dart --version

# 依赖包的迁移状态
> dart pub outdated --mode=null-safety
```

- 升级依赖

```
# 该命令会更改您的 pubspec.yaml 文件
> dart pub upgrade --null-safety

# 升级包
> dart pub upgrade
```

- 迁移工具

```
> dart migrate
```

- 分析

```
> dart analyze
```

### 禁用空安全

- cli 命令

```
> dart --no-sound-null-safety run
> flutter run --no-sound-null-safety
```

- .vscode/launch.json

```json
{
  // 使用 IntelliSense 了解相关属性。
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "getx_quick_start",
      "request": "launch",
      "type": "dart",
      "program": "lib/main.dart",
      "args": ["--no-sound-null-safety"]
    }
  ]
}
```

### 范例、规范

https://dart.cn/null-safety/understanding-null-safety

- 明确处理空状态

```dart
makeCoffee(String coffee, [String? dairy]) {
  if (dairy != null) {
    print('$coffee with $dairy');
  } else {
    print('Black $coffee');
  }
}
```

- **顶层变量和静态字段必须包含一个初始化方法。**由于它们能在程序里的任何位置被访问到，编译器无法保证它们在被使用前已被赋值。唯一保险的选项是要求其本身包含初始化表达式，以确保产生匹配的类型的值。

```dart
int topLevel = 0;

class SomeClass {
  static int staticField = 0;
}
```

- **实例的字段也必须在声明时包含初始化方法，可以为常见初始化形式，也可以在实例的构造方法中进行初始化。**

```dart
class SomeClass {
  int atDeclaration = 0;
  int initializingFormal;
  int initializationList;

  SomeClass(this.initializingFormal)
      : initializationList = 0;
}
```

- 局部变量的灵活度最高。一个非空的变量 **不一定需要** 一个初始化方法。

```dart
int tracingFibonacci(int n) {
  int result;
  if (n < 2) {
    result = n;
  } else {
    result = tracingFibonacci(n - 2) + tracingFibonacci(n - 1);
  }

  print(result);
  return result;
}
```

- 流程分析，在这里 Dart 将 `object` 的类型从它声明的 `Object` 提升到了 `List`。在空安全引入以前，下面的程序无法运行。

```dart
// With (or without) null safety:
bool isEmptyList(Object object) {
  if (object is List) {
    return object.isEmpty; // <-- OK!
  } else {
    return false;
  }
}

->

// Without null safety:
bool isEmptyList(Object object) {
  if (object is! List) return false;
  return object.isEmpty;
}
```

- 绝对的赋值分析

```dart
int tracingFibonacci(int n) {
  final int result;
  if (n < 2) {
    result = n;
  } else {
    result = tracingFibonacci(n - 2) + tracingFibonacci(n - 1);
  }

  print(result);
  return result;
}
```

- 无用代码的警告

```dart
String checkList(List list) {
  if (list?.isEmpty) {
    return 'Got nothing';
  }
  return 'Got something';
}
```

- 懒加载的变量， `late` 修饰符是“在运行时而非编译时对变量进行约束”。这就让 `late` 这个词语约等于 **何时** 执行对变量的强制约束。

```dart
// Using null safety:
class Coffee {
  String? _temperature;

  void heat() { _temperature = 'hot'; }
  void chill() { _temperature = 'iced'; }

  String serve() => _temperature! + ' coffee';
}

->

// Using null safety:
class Coffee {
  late String _temperature;

  void heat() { _temperature = 'hot'; }
  void chill() { _temperature = 'iced'; }

  String serve() => _temperature + ' coffee';
}

```

- `late` 与 `final` 结合使用，与普通的 `final` 字段不同，您不需要在声明或构造时就将其初始化。您可以稍后在运行中的某个地方加载它。但是您只能对其进行 **一次** 赋值，并且它在运行时会进行校验。

```dart
// Using null safety:
class Coffee {
  late final String _temperature;

  void heat() { _temperature = 'hot'; }
  void chill() { _temperature = 'iced'; }

  String serve() => _temperature + ' coffee';
}
```

- 毕传参数，这里的所有参数都必须通过命名来传递。参数 `a` 和 `c` 是可选的，可以省略。参数 `b` 和 `d` 是必需的，调用时必须传递。在这里请注意，是否必需和是否可空无关。

```dart
// Using null safety:
function({int? a, required int? b, int? c, required int? d}) {}
```

- 抽象字段

```dart
abstract class Cup {
  Beverage get contents;
  set contents(Beverage);
}

->

abstract class Cup {
  abstract Beverage contents;
}

```

- 一些赋值计算可以移动到静态的初始化中。

```dart
// Initalized without values
ListQueue _context;
Float32List _buffer;
dynamic _readObject;

Vec2D(Map<String, dynamic> object) {
  _buffer = Float32List.fromList([0.0, 0.0]);
  _readObject = object['container'];
  _context = ListQueue<dynamic>();
}

->

// Initalized with values
final ListQueue _context = ListQueue<dynamic>();
final Float32List _buffer = Float32List.fromList([0.0, 0.0]);
final dynamic _readObject;

Vec2D(Map<String, dynamic> object) : _readObject = object['container'];


```

- 可能返回 `null` 的工厂方法

```dart
factory StreamReader(dynamic data) {
  StreamReader reader;
  if (data is ByteData) {
    reader = BlockReader(data);
  } else if (data is Map) {
    reader = JSONBlockReader(data);
  }
  return reader;
}

->

factory StreamReader(dynamic data) {
  if (data is ByteData) {
    // Move the readIndex forward for the binary reader.
    return BlockReader(data);
  } else if (data is Map) {
    return JSONBlockReader(data);
  } else {
    throw ArgumentError('Unexpected type for data');
  }
}

```

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
