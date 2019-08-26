---
title: Dart语言学习 - 13 Runes
date: 2018-10-27 08:51:21
tags: dart
categories: Dart语言学习
---

# 本节目标

- Runes
- 基础知识 字符编码 ASCII、Unicode、UTF-8、UTF-16、UTF-32

# 环境

- Dart 2.0.0

# Runes

Runes 对象是一个 32位 字符对象，用来表示一个字。
这样设计也是考虑兼容 UTF-16 四个字节的情况。

## `length` 和 `runes.length` 比较

```dart
String a = '👺';
print(a.length);
print(a.runes.length);

>> 输出
2 // 标识占 2 个 16 位字符
1 // 表示占 1 个 32 位字符
```

> runes 是一个 32 位字符对象

## 操作 32-bit Unicode 字符

```dart
Runes b = new Runes('\u{1f596} \u6211');
var c = String.fromCharCodes(b);

或者

String c = '\u{1f596} \u6211'
```

> 如果非4个数值，需要用 {...}

## 返回 16-bit code units 的 `codeUnitAt` `codeUnits`

```dart
var a = '👺';
print(a.codeUnitAt(0));
print(a.codeUnits);

>> 输出
55357           // 第 1 位的 10 进制数值
[55357, 56442]  // 显示 2 位的 10 进制数值
```

## 返回 32-bit Unicode 的 `runes`

```dart
var a = '👺';
print(a.runes);

>> 输出

(128122) // 显示 32 位的 10 进制数值
```

## String 操作整理

名称 | 说明
-----|----------
codeUnitAt      | 某个字符的码 10进制
fromCharCodes   | Runes 转 String 工厂函数
runes           | 返回字对象

# 基础知识字符集

## ASCII

- [ASCII](https://zh.wikipedia.org/wiki/ASCII)

## 非 ASCII 中的 GB2312、GBK

- [汉字内码扩展规范](https://zh.wikipedia.org/wiki/%E6%B1%89%E5%AD%97%E5%86%85%E7%A0%81%E6%89%A9%E5%B1%95%E8%A7%84%E8%8C%83)

## Unicode、UTF-8、UTF-16、UTF-32

- [UTF-8](https://zh.wikipedia.org/wiki/UTF-8)
- [UTF-16](https://zh.wikipedia.org/wiki/UTF-16)
- [UTF-32](https://zh.wikipedia.org/wiki/UTF-32)

# 代码

- [runes.dart](https://github.com/ducafecat/dart-learn/blob/master/13-Runes/runes.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [Runes](https://api.dartlang.org/stable/2.0.0/dart-core/Runes-class.html)
- [ASCII](https://zh.wikipedia.org/wiki/ASCII)
- [Unicode](https://zh.wikipedia.org/wiki/Unicode)
- [UTF-8](https://zh.wikipedia.org/wiki/UTF-8)
- [UTF-16](https://zh.wikipedia.org/wiki/UTF-16)
- [UTF-32](https://zh.wikipedia.org/wiki/UTF-32)
- [在线字符](http://copychar.cc/popular/)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
