---
title: Dart语言学习 - 06 数值
date: 2018-10-16 17:08:33
tags: dart
categories: Dart语言学习
---

# 本节目标

- 数值类型 int、double、num
- 数值表示法 十进制、十六进制
- 科学计数法
- 数值转换
- 位运算

# 环境

- Dart 2.0.0

# 数值

## 数值类型

### int

整数值，其取值通常位于 -253 和 253 之间。

[int class](https://api.dartlang.org/stable/2.0.0/dart-core/int-class.html)

### double

64-bit (双精度) 浮点数，符合 IEEE 754 标准。

[double class](https://api.dartlang.org/stable/2.0.0/dart-core/double-class.html)

### num

int 和 double 都是 num 的子类。

[num class](https://api.dartlang.org/stable/2.0.0/dart-core/num-class.html)

## 数值表示法 十进制、十六进制

```dart
int a = 1001;
int b = 0xABC;
print([a, b]);
```

## 科学计数法

```dart
num a = 21.2e3;
print([a]);
```

## 数值转换

```dart
// string -> int
// string -> double
int a = int.parse('123');
double b = double.parse('1.223');

// int -> string
// double -> string
String a = 123.toString();
String b = 1.223.toString();
print([a, b]);

// double -> int
double a = 1.8;
int b = a.toInt();
print(b);
```

## 位运算

### `&` 与运算

同时 `1` 才行

```sh
1 0 1 0          10
0 0 1 0          2
--------
0 0 1 0          2
```

```dart
var a = 10;
var b = 2;
print(a & b);
```

### `|` 或运算

有一个 `1` 就行

```sh
1 0 1 0          10
0 0 1 0          2
--------
1 0 1 0          10
```

```dart
var a = 10;
var b = 2;
print(a | b);
```

可以用在常量组合

```dart
const USE_LEFT = 0x1;
const USE_TOP = 0x2;
const USE_LEFT_TOP = USE_LEFT | USE_TOP;
var result = USE_LEFT | USE_TOP;
print(result);
assert(USE_LEFT_TOP == result);
```

### `~` 非运算

二进制数逐位进行逻辑非运算

```sh
0 1 0 0 1          +9 二进制 最高位 0 整数 1 负数
0 0 1 1 0          补码
1 1 0 0 1          取反
1 1 0 1 0          加1
--------
1 1 0 1 0          -10
```

```dart
var a = 9;
print(~a);
```

### `^` 异或

不相同的才出 `1`

```sh
1 0 1 0          10
0 0 1 0          2
--------
1 0 0 0          8
```

```dart
var a = 10;
var b = 2;
print(a ^ b);
```

计算机中可以用来取反色

## 移位运算符

### `<<` 左移

```sh
0 0 0 1          1 二进制
0 0 1 0          左移一位 2
0 1 0 0          左移一位 4
1 0 0 0          左移一位 8
```

向左移动一位

```dart
var a = 1 << 1;
print(a);
```

### `>>` 右移

```sh
1 0 0 0          8 二进制
0 1 0 0          右移一位 4
0 0 1 0          右移一位 2
0 0 0 1          右移一位 1
```

向右移动一位

```dart
var a = 8 >> 1;
print(a);
```

# 代码

- [number.dart](https://github.com/ducafecat/dart-learn/blob/master/06-%E6%95%B0%E5%80%BC/number.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [int class](https://api.dartlang.org/stable/2.0.0/dart-core/int-class.html)
- [double class](https://api.dartlang.org/stable/2.0.0/dart-core/double-class.html)
- [num class](https://api.dartlang.org/stable/2.0.0/dart-core/num-class.html)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
