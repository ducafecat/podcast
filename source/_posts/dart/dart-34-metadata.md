---
title: Dart语言学习 - 34 注解 Metadata
date: 2019-01-22 00:15:56
tags: dart
categories: Dart语言学习
---

# 本节目标

- 了解内置注解 `deprecated` `override`
- 自定义注解，并使用反射实现

# 环境

- Dart 2.1.0

# 作用

官方称之为 `元数据` , 其实在 `java` 里就是注解

简化代码编写，方便阅读，和重用

# 内置 `deprecated`

用来注解 不建议使用、老旧的 成员对象

```dart
class Television {

  @deprecated
  void activate() {
    turnOn();
  }

  void turnOn() {
    print('on!');
  }
}

main(List<String> args) {
  var t = new Television();
  t.activate();
  t.turnOn();
}
```

# 内置 `override`

表明你的函数是想覆写超类的一个函数

超类就是被你集成的父类

下面的代码中父类是 `Object`

```dart
class A {
  @override
  noSuchMethod(Invocation mirror) {
    print('没有找到方法');
  }
}

main(List<String> args) {
  dynamic a = new A();
  a.message();
}
```

# 内置 `proxy`

注解来避免警告信息

在 Dart2 中已经被标记为过时老旧

```dart
@proxy
class A {
  noSuchMethod(Invocation mirror) {
    print('没有找到方法');
  }
}

main(List<String> args) {
  dynamic a = new A();
  a.say();
}
```

# 自定义注解

使用反射可以在运行时获取元数据信息

比如服务端的控制器开发

下面的代码 展示了如何在反射中读取 `metadata` 信息

```dart
import 'dart:mirrors';

@Todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}

class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}

main(List<String> args) {
  currentMirrorSystem().libraries.forEach((uri, lib) {
    // print('lib: ${uri}');
    lib.declarations.forEach((s, decl) {
      // print('decl: ${s}');
      decl.metadata.where((m) => m.reflectee is Todo).forEach((m) {
        var anno = m.reflectee as Todo;
        if (decl is MethodMirror) {
          print('Todo(${anno.who}, ${anno.what})');
          ((decl as MethodMirror).owner as LibraryMirror).invoke(s, []);
        }
        ;
      });
    });
  });
}
```

# 代码

- [34-媒体信息](https://github.com/ducafecat/dart-learn/tree/master/34-%E5%AA%92%E4%BD%93%E4%BF%A1%E6%81%AF)

# 参考

- [metadata](https://www.dartlang.org/guides/language/language-tour#metadata)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
