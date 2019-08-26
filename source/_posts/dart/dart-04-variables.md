---
title: Dart语言学习 - 04 变量的两种类型
date: 2018-10-10 10:18:25
tags: dart
categories: Dart语言学习
---

# 本节目标

- 了解 `弱类型` `强类型`
- 常见 `强类型` 有哪些
- 如何选着何时用那种类型

# 环境

- Dart 2.0.0

# 弱类型

## var

如果没有初始值，可以变成任何类型

```dart
var a;
a = 'ducafecat';
a = 123;
a = true;
a = {'key': 'val123'};
a = ['abc'];
```

## Object

动态任意类型，编译阶段检查类型

```dart
Object a = 'doucafecat';
a = 123;
a = [2222];
a.p();
```

## dynamic

动态任意类型，编译阶段不检查检查类型

```dart
dynamic a = 'doucafecat';
a = 123;
a = [1111];
a.p();
```

## 比较 var 与 dynamic、Object

唯一区别 var 如果有初始值，类型被锁定

```dart
var a = 'ducafecat';
dynamic a = 'doucafecat';
Object a = 'doucafecat';
a = 123;
```

# 强类型

## 申明类型

声明后，类型被锁定

```dart
String a;
a = 'ducafecat';
a = 123;
```

![](2018-10-10-11-24-04.png)

## 常见类型

名称 | 说明
-----|-----------
num           | 数字
int           | 整型
double        | 浮点
bool          | 布尔
String        | 字符串
StringBuffer  | 字符串 buffer
DateTime      | 时间日期
Duration      | 时间区间
List          | 列表
Sets          | 无重复队列
Maps          | kv 容器
enum          | 枚举

```dart
String a = 'doucafecat';
int i = 123;
double d = 0.12;
bool b = true;
DateTime dt = new DateTime.now();
List l = [ a, i, d, b, dt];
```

# 默认值

一切都是 `Object` , 变量声明后默认都是 `null`

```dart
var a;
String a;
print(a);
assert(a == null);
```

> `assert` 检查点函数，如果不符合条件直接抛出错误并终止程序进程

# 如何使用

- 在写 API 接口的时候，请用 `强类型`，一旦不符合约定，接收数据时能方便排查故障
- 你在写个小工具时，可以用 `弱类型`，这样代码写起来很快，类型自动适应

# 代码

- [variables.dart](https://github.com/ducafecat/dart-learn/tree/master/04-%E5%8F%98%E9%87%8F%E7%9A%84%E4%B8%A4%E7%A7%8D%E7%B1%BB%E5%9E%8B)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
