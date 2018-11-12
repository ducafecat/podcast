---
title: Dart语言学习 - 15 函数 Function
date: 2018-11-12 09:36:04
tags: dart
categories: Dart语言学习
---

# 本节目标

- 函数定义
- 可选参数
- 默认值
- 命名参数
- 内部定义

# 环境

- Dart 2.0.0

# 函数定义

```dart
int add(int x) {
  return x + 1;
}

调用
add(1);
```

# 可选参数

```dart
int add(int x, [int y, int z]) {
  if (y == null) {
    y = 1;
  }
  if (z == null) {
    z = 1;
  }
  return x + y + z;
}

调用
int(1, 2);
```

# 可选参数 默认值

```dart
int add(int x, [int y = 1, int z = 2]) {
  return x + y;
}

调用
int(1, 2);
```

# 命名参数 默认值

```dart
int add({int x = 1, int y = 1, int z = 1}) {
  return x + y + z;
}

调用
int(x: 1, y: 2);
```

# 函数内定义

```dart
void main(){
  int add(int x){
    return x + x;
  }
  print(add(1));
}
```

# Funcation 返回函数对象

```dart
Function makeAdd(int x) {
  return (int y) => x + y;
}

调用
var add = makeAdd(1);
print(add(5));
```

# 代码

- [function.dart](https://github.com/ducafecat/dart-learn/blob/master/15-%E5%87%BD%E6%95%B0-function/function.dart)

# 参考

- [Functions](https://www.dartlang.org/guides/language/language-tour#functions)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
