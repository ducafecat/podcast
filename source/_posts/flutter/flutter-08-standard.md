---
title: Flutter 零基础入门中文教学 - 08 开发规范
date: 2019-08-16 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- Dart 规范
- Flutter 阿里规范

## VSCode 格式化

- 右键菜单

![](2019-08-26-15-10-18.png)

- 当 Save 时自动格式化

![](2019-08-26-15-08-42.png)

## 规范精要

### 使用小写加下划线来命名库和源文件

```dart
  library peg_parser.source_scanner;

  import 'file_system.dart';
  import 'slider_menu.dart';
```

### 优先使用小驼峰法作为常量命名

```dart
  const pi = 3.14;
  const defaultTimeout = 1000;
  final urlScheme = RegExp('^([a-z]+):');

  class Dice {
    static final numberGenerator = Random();
  }
```

### 所有流控制结构，请使用大括号

```dart
  if (isWeekDay) {
    print('Bike to work!');
  } else {
    print('Go dancing or read a book!');
  }
```

### Doc 注释

使用///文档注释来记录成员和类型。

```dart
  /// The number of characters in this chunk when unsplit.
  int get length => ...
```

### 导入 lib 下文件库，统一指定包名，避免过多的```../../

```dart
package:flutter_go/
```

### 使用相邻字符串连接字符串文字

```dart
raiseAlarm(
    'ERROR: Parts of the spaceship are on fire. Other '
    'parts are overrun by martians. Unclear which are which.');
```

### 优先使用模板字符串

```dart
'Hello, $name! You are ${year - birth} years old.';
```

### 在不需要的时候，避免使用花括号

```dart
  'Hi, $name!'
  "Wear your wildest $decade's outfit."
```

### 不要使用.length 查看集合是否为空

```dart
if (lunchBox.isEmpty) return 'so hungry...';
if (words.isNotEmpty) return words.join(' ');
```

### 遍历一个序列

```dart
for (var person in people) {
  ...
}
```

### 不要显式地将变量初始化为空

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

### 在不需要的时候不要用 this

```dart
  class Box {
    var value;

    void clear() {
      update(null);
    }

    void update(value) {
      this.value = value;
    }
  }
```

### 尽可能使用初始化的形式

```dart
class Point {
  num x, y;
  Point(this.x, this.y);
}
```

### 不要使用 new

```dart
  Widget build(BuildContext context) {
    return Row(
      children: [
        RaisedButton(
          child: Text('Increment'),
        ),
        Text('Click!'),
      ],
    );
  }
```

### 优先使用 async/await 代替原始的 futures

```dart
  Future<int> countActivePlayers(String teamName) async {
    try {
      var team = await downloadTeam(teamName);
      if (team == null) return 0;

      var players = await team.roster;
      return players.where((player) => player.isActive).length;
    } catch (e) {
      log.error(e);
      return 0;
    }
  }
```

### 当异步没有任何用处时，不要使用它

```dart
  Future afterTwoThings(Future first, Future second) {
    return Future.wait([first, second]);
  }
```

## 参考

- [Dart 官方规范](http://dart.goodev.org/guides/language/effective-dart)
- [阿里 Flutter Go 代码开发规范 0.1.0 版](https://github.com/alibaba/flutter-go/blob/develop/Flutter_Go%20%E4%BB%A3%E7%A0%81%E5%BC%80%E5%8F%91%E8%A7%84%E8%8C%83.md)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
