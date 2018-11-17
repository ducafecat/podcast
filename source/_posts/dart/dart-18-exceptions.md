---
title: Dart语言学习 - 18 异常
date: 2018-11-17 10:53:26
tags: dart
categories: Dart语言学习
---

# 本节目标

# 环境

- Dart 2.0.0

# 错误的两种类型

## Exception 类

[Exception class](https://api.dartlang.org/stable/2.1.0/dart-core/Exception-class.html)

| 名称                           | 说明         |
| ------------------------------ | ------------ |
| DeferredLoadException          | 延迟加载错误 |
| FormatException                | 格式错误     |
| IntegerDivisionByZeroException | 整数除零错误 |
| IOException                    | IO 错误      |
| IsolateSpawnException          | 隔离产生错误 |
| TimeoutException               | 超时错误     |

## Error 类
[Error class](https://api.dartlang.org/stable/2.1.0/dart-core/Error-class.html)

| 名称                            | 说明              |
| ------------------------------- | ----------------- |
| AbstractClassInstantiationError | 抽象类实例化错误  |
| ArgumentError                   | 参数错误          |
| AssertionError                  | 断言错误          |
| AsyncError                      | 异步错误          |
| CastError                       | Cast 错误         |
| ConcurrentModificationError     | 并发修改错误      |
| CyclicInitializationError       | 周期初始错误      |
| FallThroughError                | Fall Through 错误 |
| JsonUnsupportedObjectError      | json 不支持错误   |
| NoSuchMethodError               | 没有这个方法错误  |
| NullThrownError                 | Null 错误错误     |
| OutOfMemoryError                | 内存溢出错误      |
| RemoteError                     | 远程错误          |
| StackOverflowError              | 堆栈溢出错误      |
| StateError                      | 状态错误          |
| UnimplementedError              | 未实现的错误      |
| UnsupportedError                | 不支持错误        |

# 抛出错误

```dart
  // Exception 对象
  // throw new FormatException('这是一个格式错误提示');
  
  // Error 对象
  // throw new OutOfMemoryError();

  // 任意对象
  // throw '这是一个异常';
```

# 捕获错误

```dart
  // try {
  //   // throw new FormatException('这是一个格式错误提示');
  //   throw new OutOfMemoryError();
  // } on OutOfMemoryError {
  //   print('没有内存了');
  // } catch (e) {
  //   print(e);
  // }
```

# 重新抛出错误

```dart
  // try {
  //   throw new OutOfMemoryError();
  // } on OutOfMemoryError {
  //   print('没有内存了');
  //   rethrow;
  // } catch (e) {
  //   print(e);
  // }
```

# Finally 执行

```dart
  // try {
  //   throw new OutOfMemoryError();
  // } on OutOfMemoryError {
  //   print('没有内存了');
  //   rethrow;
  // } catch (e) {
  //   print(e);
  // } finally {
  //   print('end');
  // }
```

# 代码

- [exception.dart](https://github.com/ducafecat/dart-learn/blob/master/18-异常/exception.dart)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [Exception class](https://api.dartlang.org/stable/2.1.0/dart-core/Exception-class.html)
- [Error class](https://api.dartlang.org/stable/2.1.0/dart-core/Error-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
