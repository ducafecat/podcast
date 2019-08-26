---
title: Dart语言学习 - 12 Set
date: 2018-10-25 14:47:10
tags: dart
categories: Dart语言学习
---

# 本节目标

- 初始、声明
- 常用属性
- 常用方法

# 环境

- Dart 2.0.0

# 声明

`Set` 是一个元素唯一的有序队列

## 松散

```dart
  // var a = new Set();
  // a.add('java');
  // a.add('php');
  // a.add('python');
  // a.add('java');
  // a.add('sql');
  // a.add('swift');
  // a.add('dart');
```

## 强类型

```dart
  // var b = new Set<String>();
  // b.addAll(['dart', 'c#', 'j#', 'e#']);
```

# 基本属性

名称 | 说明
-----|----------
isEmpty     | 是否为空
isNotEmpty  | 是否不为空
first       | 第一个
last        | 最后一个
length      | 个数

# 常用方法

名称 | 说明
-----|----------
addAll        | 添加
contains      | 查询单个
containsAll   | 查询多个
difference    | 集合不同
intersection  | 交集
union         | 联合
lookup        | 按对象查询到返回对象
remove        | 删除单个
removeAll     | 删除多个
clear         | 清空
firstWhere    | 按条件正向查询
lastWhere     | 按条件反向查询
removeWhere   | 按条件删除
retainAll     | 只保留几个
retainWhere   | 按条件只保留几个

```dart
  // b.addAll(['dart', 'c#', 'j#', 'e#']);
  // print(b.contains('dart'));
  // print(b.containsAll(['dart', 'swift']));
  // print('b => $b');
  // print(a.difference(b));
  // print(a.intersection(b));
  // print(b.lookup('dart'));
  // print(b.union(a));
  // b.remove('dart');
  // b.removeAll(['dart','c#']);
  // b.clear();
  // print(b.firstWhere((it) => it == 'c#'));
  // print(b.lastWhere((it) => it == 'c#'));
  // b.removeWhere((it) => it == 'c#');
  // b.retainAll(['e#']);
  // b.retainWhere((it) => it == 'e#');
  // b.retainWhere((it) {
  //   bool ret = it == 'e#';
  //   return ret;
  // });
```

# 代码

- [set.dart](https://github.com/ducafecat/dart-learn/blob/master/12-Set/set.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [Set](https://api.dartlang.org/stable/2.0.0/dart-core/Set-class.html)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
