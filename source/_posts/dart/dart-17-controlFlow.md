---
title: Dart语言学习 - 17 流程控制语句
date: 2018-11-12 15:56:51
tags: dart
categories: Dart语言学习
---

# 本节目标

- 条件判断
- 循环控制

# 环境

- Dart 2.0.0

# if else

```dart
bool isPrint = true;
if (isPrint) {
  print('hello');
}
```

# for

```dart
for (var i = 0; i < 5; i++) {
  print(i);
}
```

# while

```dart
bool isDone = false;
while(!isDone) {
  print('is not done');
  isDone = true;
}
```

# do while

```dart
bool isRunning = true;
do {
  print('is running');
  isRunning = false;
} while (isRunning);
```

# switch case

```dart
String name = 'cat';
switch (name) {
  case 'cat':
    print('cat');
    break;
  default:
    print('not find');
}
```

# break

```dart
num i = 1;
while(true) {
  print('${i} - run');
  i++;
  if(i == 5) {
    break;
  }
}
```

# continue

```dart
for (var i = 0; i < 5; i++) {
  if (i < 3) {
    continue;
  }
  print(i);
}
```

# continue 指定位置

```dart
String command = "close";
switch(command) {
  case "open":
    print("open");
    break;
  case "close":
    print("close");
    continue doClose;

  doClose:
  case "doClose":
    print("DO_CLOSE");
    break;

  default:
    print("-----");
}
```

# 代码

- [controlFlow.dart](https://github.com/ducafecat/dart-learn/blob/master/17-%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6%E8%AF%AD%E5%8F%A5/controlFlow.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
