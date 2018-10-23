---
title: Dart语言学习 - 10 列表
date: 2018-10-19 10:49:35
tags: dart
categories: Dart语言学习
---

# 本节目标

- 初始、声明
- 常用属性
- 常用方法

# 环境

- Dart 2.0.0

# 初始

List 是一个有序列表

```dart
var l = [1, 2, 3];
print(l);
```

# 声明

## 定长

```dart
List<int> l = new List();
l
..add(1)
..add(2)
..add(3);
print(l);
```

## 自动

```dart
List<int> l = new List(3);
// print(l[0]);
l[0] = 1;
l[1] = 2;
l[2] = 3;
print(l);
```

# 属性

名称 | 说明
-----|----------
isEmpty     | 是否为空
isNotEmpty  | 是否不为空
first       | 第一个对象
last        | 最后一个对象
length      | 个数
reversed    | 反转

```dart
var l = [1, 2, 3];
print(l.first);
print(l.last);
print(l.length);
print(l.isEmpty);
print(l.isNotEmpty);
print(l.reversed);
```

# 方法

名称 | 说明
-----|----------
add         | 添加
addAll      | 添加多个
insert      | 插入
insertAll   | 插入多个
indexOf     | 查询
indexWhere  | 按条件查询
remove      | 删除
removeAt    | 按位置删除
fillRange   | 按区间填充
getRange    | 按区间获取
shuffle     | 随机变换顺序
sort        | 排序
sublist     | 创建子列表

## 添加

```dart
List<int> l = new List();

l
  ..add(1)
  ..addAll([2, 3, 4, 5])
  ..insert(0, 6)
  ..insertAll(6, [6, 6])
  ;
```

## 查询

```dart
print(l.indexOf(5));
print(l.indexWhere((it) => it == 4));
```

## 删除

```dart
l.remove(6);
print(l);
l.removeAt(5);
print(l);
```

## Range

```dart
l.fillRange(0, 3, 9);
print(l.getRange(0, 5));
```

## 洗牌

```dart
l.shuffle();
print(l);
l.shuffle();
print(l);
```

## 排序

```dart
数字
l.sort();
print(l);
日期
List<DateTime> dtList = new List();
dtList.addAll([
  DateTime.now(),
  DateTime.now().add(new Duration(days: -12)),
  DateTime.now().add(new Duration(days: -2))
  ]);
print(dtList);
dtList.sort((a, b) => a.compareTo(b));
print(dtList);
```

## 复制子列表

```dart
print(l);
var l2 = l.sublist(1,4);
print(l2);
```

## 操作符

名称 | 说明
-----|----------
+      | 连接
[]     | 取值
[]=    | 赋值

```dart
var l1 = [1, 2, 3];
var l2 = [4, 5, 6];
print(l1 + l2);
l1[2] = 9;
print(l1[2]);
```

# 代码

- [list.dart](https://github.com/ducafecat/dart-learn/blob/master/10-%E5%88%97%E8%A1%A8/list.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [List](https://api.dartlang.org/stable/2.0.0/dart-core/List-class.html)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
