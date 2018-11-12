---
title: Dart语言学习 - 16 操作符
date: 2018-11-12 10:41:28
tags: dart
categories: Dart语言学习
---

# 本节目标

- 优先级
- 算术操作符
- 相等相关的操作符
- 类型判定操作符
- 赋值操作符
- 逻辑操作符
- 位和移位操作符
- 条件表达式
- 级联操作符

# 环境

# 操作符表

| 描述      | 操作符                                      |
| --------- | ------------------------------------------- |
|  后缀操作 | expr++ expr-- () [] . ?.                    |
| 前缀操作  | -expr !expr ~expr ++expr --expr             |
| 乘除      | \* / % ~/                                   |
| 加减      | + -                                         |
| 位移      | << >>                                       |
| 按位与    | &                                           |
| 按位异或  | ^                                           |
| 按位或    | \|                                          |
| 类型操作  | >= > <= < as is is!                         |
| 相等      | == !=                                       |
| 逻辑与    | &&                                          |
| 逻辑或    | \|\|                                        |
| 是为为空  | ??                                          |
| 三目运算  | expr1 ? expr2 : expr3                       |
| 级联      | ..                                          |
| 赋值      | = \*= /= ~/= %= += -= <<= >>= &= ^= \|= ??= |

> 优先级顺序 `上面左边` 优先级高于 `右边下面`

```dart
if(x == 1 && y == 2){
  ...
}
```

# 算术操作符

| 操作符 | 解释                   |
| ------ | ---------------------- |
| +      | 加号                   |
| –      | 减号                   |
| -expr  | 负号                   |
| \*     | 乘号                   |
| /      | 除号                   |
| ~/     | 除号，但是返回值为整数 |
| %      | 取模                   |

```dart
  print(5/2);
  print(5~/2);
  print(5 % 2);
```

# 相等相关的操作符

| 操作符 | 解释     |
| ------ | -------- |
| ==     | 相等     |
| !=     | 不等     |
| >      | 大于     |
| <      | 小于     |
| >=     | 大于等于 |
| <=     | 小于等于 |

# 类型判定操作符

| 操作符 | 解释                           |
| ------ | ------------------------------ |
| as     | 类型转换                       |
| is     | 如果对象是指定的类型返回 True  |
| is!    | 如果对象是指定的类型返回 False |

```dart
  int a = 123;
  String b = 'ducafecat';
  String c = 'abc';
  print(a as Object);
  print(b is String);
  print(c is! String);
```

# 条件表达式

| 操作符                    | 解释                                                                                  |
| ------------------------- | ------------------------------------------------------------------------------------- |
| condition ? expr1 : expr2 | 如果 condition 是 true，执行 expr1 (并返回执行的结果)； 否则执行 expr2 并返回其结果。 |
| expr1 ?? expr2            | 如果 expr1 是 non-null，返回其值； 否则执行 expr2 并返回其结果。                      |

```dart
  bool isFinish = true;
  String txtVal = isFinish ? 'yes' : 'no';

  bool isFinish;
  isFinish = isFinish ?? false;
  or
  isFinish ??= false;
```

# 位和移位操作符

| 操作符 | 解释     |
| ------ | -------- |
| &      | 逻辑与   |
|        | 逻辑或   |
| ^      | 逻辑异或 |
| ~expr  | 取反     |
| <<     | 左移     |
| >>     | 右移     |

# 级联操作符

| 操作符 | 解释                                                  |
| ------ | ----------------------------------------------------- |
| ..     | 可以在同一个对象上 连续调用多个函数以及访问成员变量。 |

```dart
  StringBuffer sb = new StringBuffer();
  sb
  ..write('hello')
  ..write('word')
  ..write('\n')
  ..writeln('doucafecat');
```

# 其他操作符

| 操作符 | 解释                                                                                                                 |
| ------ | -------------------------------------------------------------------------------------------------------------------- |
| ()     | 使用方法 代表调用一个方法                                                                                            |
| []     | 访问 List 访问 list 中特定位置的元素                                                                                 |
| .      | 访问 Member 访问元素，例如 foo.bar 代表访问 foo 的 bar 成员                                                          |
| ?.     | 条件成员访问 和 . 类似，但是左边的操作对象不能为 null，例如 foo?.bar 如果 foo 为 null 则返回 null，否则返回 bar 成员 |

```dart
  String a;
  print(a?.length);
```

# 代码

- [operator.dart](https://github.com/ducafecat/dart-learn/blob/master/16-%E6%93%8D%E4%BD%9C%E7%AC%A6/operator.dart)

# 参考

- [operators](https://www.dartlang.org/guides/language/language-tour#operators)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
