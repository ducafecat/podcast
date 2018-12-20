---
title: Dart语言学习 - 29 异步 async
date: 2018-12-05 15:51:09
tags: dart
categories: Dart语言学习
---

# 本节目标

- 调用异步 等待、递归
- 异步返回值

# 环境

# 调用异步 回调

```dart
import 'package:dio/dio.dart';

void main() {
  Dio dio = new Dio();
  dio.get("https://www.baidu.com").then((response) {
    print(response.data);
  });
}
```

> `then` 的方式异步回调

# 调用异步 等待

```dart
import 'package:dio/dio.dart';

void main() async {
  Dio dio = new Dio();
  Response<String> response = await dio.get("https://www.baidu.com");
  print(response.data);
}
```

> `async` 写在函数定义
> `await` 写在异步请求头

# 异步返回值

```dart
import 'package:dio/dio.dart';

void main() async {
  var content = await getUrl('https://www.baidu.com');
  print(content);
}

Future<String> getUrl(String url) async {
  Dio dio = new Dio();
  Response<String> response = await dio.get(url);
  return response.data;
}
```

> 定义 `Future<T>` 返回对象

# 代码

- [异步](https://github.com/ducafecat/dart-learn/tree/master/29-%E5%BC%82%E6%AD%A5)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
