---
title: Dart语言学习 - 30 - 生成器 Generators
date: 2019-01-15 17:25:58
tags: dart
categories: Dart语言学习
---

# 本节目标

- 同步、异步代码生成器

# 环境

- Dart 2.1.0

# 同步生成器

```dart
main(List<String> args) {
  var it = naturalsTo(5).iterator;
  while(it.moveNext()) {
    print(it.current);
  }
}

Iterable<int> naturalsTo(int n) sync* {
  print('start');
  int k = 0;
  while (k < n) {
    yield k++;
  }
  print('end');
}
```

> yield 会等待 `moveNext` 指令

# 异步生成器

```dart
import 'dart:async';

main(List<String> args) {
  // 流监听
  // asynchronousNaturalsTo(5).listen((onData) {
  //   print(onData);
  // });

  // 流监听 StreamSubscription 对象
  StreamSubscription subscription = asynchronousNaturalsTo(5).listen(null);
  subscription.onData((value) {
    print(value);
    // subscription.pause();
  });
}

Stream<int> asynchronousNaturalsTo(int n) async* {
  print('start');
  int k = 0;
  while (k < n) {
    yield k++;
  }
  print('end');
}
```

> 以流的方式一次性推送
>
> `StreamSubscription` 对象进行流监听控制

# 递归生成器

```dart
main(List<String> args) {
  var it = naturalsDownFrom(5).iterator;
  while(it.moveNext()) {
    print(it.current);
  }
}

Iterable<int> naturalsDownFrom(int n) sync* {
  if ( n > 0) {
    yield n;
    yield* naturalsDownFrom(n-1);
  }
}
```

> `yield*` 以指针的方式传递递归对象，而不是整个同步对象

# 代码

- [生成器 generators](https://github.com/ducafecat/dart-learn/tree/master/30-%E7%94%9F%E6%88%90%E5%99%A8)

# 参考

- [generators](https://www.dartlang.org/guides/language/language-tour#generators)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
