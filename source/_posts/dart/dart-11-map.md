---
title: Dart语言学习 - 11 Map
date: 2018-10-23 15:36:12
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

key value 形式的集合

```dart
var a = {'name': 'ducafecat', 'web': 'www.ducafecat.tech'};
```

# 声明

## 松散

```dart
var a = new Map();
a['name'] = 'ducafecat';
a['web'] = 'www.ducafecat.tech';
a[0] = 'abc';
```

## 强类型

```dart
var b = new Map<int, String>();
b[0] = 'java';
b[1] = 'python';
```

# 基本属性

名称 | 说明
-----|----------
isEmpty     | 是否为空
isNotEmpty  | 是否不为空
keys        | key 集合
values      | values 集合
length      | 个数
entries     | 加工数据入口

```dart
print(a.isEmpty);
print(a.isNotEmpty);
print(a.keys);
print(a.values);
print(a.length);
print(a.entries);
```

# 常用方法

名称 | 说明
-----|----------
addAll        | 添加
addEntries    | 从入口添加
containsKey   | 按 key 查询
containsValue | 按 value 查询
clear         | 清空
remove        | 删除某个
removeWhere   | 按条件删除
update        | 更新某个
updateAll     | 按条件更新

## addAll

```dart
b.addAll({'first': 'java', 'second': 'python'});
```

## addEntries

```dart
b.addEntries(a.entries);
```

## containsKey

```dart
print(a.containsKey('name'));
```

## containsValue

```dart
print(a.containsValue('www.ducafecat.tech'));
```

## clear

```dart
b.clear();
```

## remove

```dart
a.remove('name');
```

## removeWhere

```dart
a.removeWhere((key,val) => key == 'name');
```

## update

```dart
a.update('name', (val) => 'abc');
```

## updateAll

```dart
a.updateAll((key, val) => "---$val---");
```

# 操作符

名称 | 说明
-----|----------
[]     | 取值
[]=    | 赋值

```dart
print(a['name']);
a['name'] = 'abc';
```

# 代码

- [map.dart](https://github.com/ducafecat/dart-learn/blob/master/11-Map/map.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [Map](https://api.dartlang.org/stable/2.0.0/dart-core/Map-class.html)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
