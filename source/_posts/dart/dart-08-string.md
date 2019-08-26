---
title: Dart语言学习 - 08 字符串
date: 2018-10-18 13:48:22
tags: dart
categories: Dart语言学习
---

# 本节目标

- 声明方式
- 字符串模板
- 字符串连接
- 转义操作
- 其它常用运算

# 环境

- Dart 2.0.0

# 单引号或者双引号

```dart
String a = 'ducafecat';
String b = "ducafecat";
```

# 字符串模板

```dart
var a = 123;
String b = 'ducafecat : ${a}';
print(b);
```

# 字符串连接

```dart
var a = 'hello' + ' ' + 'ducafecat';
var a = 'hello'' ''ducafecat';
var a = 'hello'   ' '     'ducafecat';
var a = 'hello'
' '
'ducafecat';
var a = '''
hello word
this is multi line
''';
var a = """
hello word
this is multi line
""";
print(a);
```

# 转义符号

```dart
var a = 'hello word \n this is multi line';
print(a);
```

# 取消转义

```dart
var a = r'hello word \n this is multi line';
print(a);
```

# 搜索

```dart
var a = 'web site ducafecat.tech';
print(a.contains('ducafecat'));
print(a.startsWith('web'));
print(a.endsWith('tech'));
print(a.indexOf('site'));
```

# 提取数据

```dart
print(a.substring(0,5));
var b = a.split(' ');
print(b.length);
print(b[0]);
```

# 大小写转换

```dart
print(a.toLowerCase());
print(a.toUpperCase());
```

# 裁剪 判断空字符串

```dart
print('    hello word     '.trim());
print(''.isEmpty);
```

# 替换部分字符

```dart
print('hello word word!'.replaceAll('word', 'ducafecat'));
```

# 字符串创建

```dart
var sb = StringBuffer();
sb..write('hello word!')
..write('my')
..write(' ')
..writeAll(['web', 'site', 'https://ducafecat.tech']);
print(sb.toString());
```

# 代码

- [string.dart](https://github.com/ducafecat/dart-learn/blob/master/08-%E5%AD%97%E7%AC%A6%E4%B8%B2/string.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [String](https://api.dartlang.org/stable/2.0.0/dart-core/String-class.html)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
