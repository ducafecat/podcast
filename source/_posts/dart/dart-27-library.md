---
title: Dart语言学习 - 27 库
date: 2018-12-05 10:00:54
tags: dart
categories: Dart语言学习
---

# 本节目标

- 核心库
- 外部库
- 导入模块

# 环境

- Dart 2.1.0

# 导入核心库

```dart
import 'dart:io';

void main() {
  var f = new File('README.md');
  var content = f.readAsStringSync();
  print(content);
}
```

# 导入第三方库

- 编写 `pubspec.yaml`

```yml
name: ducafecat
dependencies:
  dio: 1.0.9
```

- 程序调用

```dart
import 'package:dio/dio.dart';

void main() async {
  Dio dio = new Dio();
  Response<String> response = await dio.get("https://www.baidu.com");
  print(response.data);
}
```

# 导入文件

```dart
import './phone.dart';

void main() {
  var xm = Phone('android');
  xm.startup();
  xm.shutdown();
}
```

# 前缀

```dart
import './phone.dart';
import './phone1.dart' as pp;

void main() {
  var xm = Phone('android');
  xm.startup();
  xm.shutdown();

  var xm1 = pp.Phone('android');
  xm1.startup();
  xm1.shutdown();
}
```

# 筛选包内容

```dart
// import './phone.dart' hide AndroidPhone;
import './phone.dart' show IOSPhone;

void main() {
  var xm = IOSPhone();
  xm.startup();
  xm.shutdown();
}
```

> `hideo` 筛掉某几个包
> `show` 只使用某几个包

# 延迟载入

```dart
import './phone.dart' deferred as pp;

void main() async {
  var run = true;
  if (run) {
    await pp.loadLibrary();
    var xm = pp.Phone('android');
    xm.startup();
    xm.shutdown();
  }
}
```

> `loadLibrary()` 方式在需要的时候载入包
> 可提高程序启动速度
> 用在不常使用的功能
> 用在载入时间过长的包

# 代码

- [库](https://github.com/ducafecat/dart-learn/tree/master/27-%E5%BA%93)

# 参考

- [language-tour](https://www.dartlang.org/guides/language/language-tour)
- [包管理平台](https://pub.dartlang.org/)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
