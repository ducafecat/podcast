---
title: Dart语言学习 - 13 Runes
date: 2018-10-27 08:51:21
tags: dart
categories: Dart语言学习
---

# 本节目标

- 字符编码 ASCII、Unicode、UTF-8、UTF-16、UTF-32
- 字与字符
- 如何操作

# 环境

- Dart 2.0.0

# 字符集

## ASCII

- [ASCII](https://zh.wikipedia.org/wiki/ASCII)

## 非 ASCII 中的 GB2312、GBK

- [汉字内码扩展规范](https://zh.wikipedia.org/wiki/%E6%B1%89%E5%AD%97%E5%86%85%E7%A0%81%E6%89%A9%E5%B1%95%E8%A7%84%E8%8C%83)

## Unicode、UTF-8、UTF-16、UTF-32

- [UTF-8](https://zh.wikipedia.org/wiki/UTF-8)
- [UTF-16](https://zh.wikipedia.org/wiki/UTF-16)
- [UTF-32](https://zh.wikipedia.org/wiki/UTF-32)

# Runes 对象

Runes 对象是一个 32位 字符对象，用来表示一个字。
这样设计也是考虑兼容 UTF-16 四个字节的情况。

## Runes 转 String

```dart
Runes b = new Runes('\u{1f596} \u6211');
var c = String.fromCharCodes(b);
```

## String 转 Runes

```dart
var a = '👺';
print(a.codeUnitAt(0));// 显示某个字符的 10进制
print(a.length); // 表示这个字符 占2位
print(a.runes.length); // 表示有几个字符
print(a.codeUnits);// 打印 占2位 字符码
print(a.runes);// 打印 字符码 10进制
```

## String 操作整理

名称 | 说明
-----|----------
codeUnitAt      | 某个字符的码 10进制
fromCharCodes   | Runes 转 String 工厂函数
runes           | 返回字对象

# 代码

- []()

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