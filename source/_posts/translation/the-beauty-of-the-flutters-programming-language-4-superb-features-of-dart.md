---
title: Dart 的编程语言之美 4 个超级特性
tags: flutter
categories: 译文
date: 2021-06-15 07:46:06
---

![](2021-06-15-07-58-44.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 Flutter 技术群 ducafecat

## 原文

> https://reprom.io/the-beauty-of-the-flutters-programming-language-4-superb-features-of-dart

## 参考

- https://dart.dev/codelabs/async-await
- https://medium.com/flutter-community/dart-what-are-mixins-3a72344011f3
- https://dart.dev/guides/language/language-tour
- https://api.dart.dev/stable/2.13.1/dart-isolate/dart-isolate-library.html

## 正文

在阅读 Flutter 时，我读到最多的缺点之一就是使用 Dart 编程语言。它还没有 Kotlin 那么成熟，这是我读到的最常被提及的论点之一。在我看来(我承认这可能会引起争议) ，Dart 是一种伟大的语言，我不会在 Flutter 创建应用程序时为其他任何语言改变它，我是在 Kotlin 专业地创建 android 应用程序之后说的，顺便说一句，这也是一种优雅而美丽的语言。

在这篇文章中，我打算展示我最喜欢的 Dart 编程语言的 4 个特性，没有特定的顺序; 让我们看看我们如何利用这个现代工具:

### Null safety

最近在 2.12 版本中添加(包括在 Flutter 2.0 中)。在任何假装坚实和高效的现代语言中，空安全都是必须的。这就是为什么 Dart 团队一直致力于实现声音 null 安全，这意味着我们可以有可以为空的类型和不能为空的类型，如果我们尝试在后面执行一个不安全的操作，我们会在应用程序构建之前得到一个编译错误:

```dart
// This is a String that can be null
String? nullVar;

// This String cannot be null, the compiler forces me
// to set it a value because of its non-nullable nature.
String nonNullVar = 'I am not null';

// Alternatively, I can specify that the value will be
// set later, but the property continues to be non-nullable.
late String lateNonNullVar;

// If I want to call a method on an instance of a type
// that can be null, I need first to do a runtime check that
// its value is not null.
if (nullVar != null) {
  nonNullVar.toLowerCase();
}

// Or call it using the '?' operator, which means that the
// method will only be called if the instance is not null:
nullVar?.toLowerCase();

// If the type is not nullable I can safely call any
// method on it directly.
nonNullVar.toLowerCase();

// Always remember to initialize late vars, or you
// will get an exception when trying to access its members.
lateNonNullVar = 'some value';
lateNonNullVar.toLowerCase();
```

### Async / await

就像在 Javascript 中我们有 Promises，在 Dart 中我们有 Futures，其中 async/await 是主要的关键词，这给了我们开发者一个简单而强大的方法来处理异步操作:

使用 Futures，我们可以轻松地停止当前操作流，等待某个异步操作完成，然后继续工作。

```dart
// To specify that a function will perform an asynchronous
// operation (like doing a network request or reading from
// a database) we mark it with the 'async' keyword:
asyncFunction() async {
  // ...
}

// Use the 'await' keyword to stop the flow until the function
// has completed its task:
await asyncFunction();

// You must declare a function as 'async' if it contains
// calls to other async functions:
main() async {
  await asyncFunction();
}

// If the async function returns a value, wrap it within
// a Future. For instance, the following function
// would do a network call and return its result:
Future<NetworkResult> doNetworkCall() async {
  // ...
}

final result = await doNetworkCall();

// But what if the network request fails and an exception
// is thrown? Just wrap the call in a try/catch block:
late NetworkResult result;
try {
  result = await doNetworkCall();
} on NetworkException catch (e) {
  // Handle error
}
```

需要指出的是，即使使用 async/await 关键字，所有的操作都是在同一个线程中执行的，如果我们需要具体的性能要求，我们可以使用 isolates 产生替代线程。

### 定义函数参数的多种方法

在 Dart 中，我们在定义函数参数时有多个选项:

```dart
// You can define mandatory parameters as you do in
// many other languages, specifying their type and setting a label:
functionWithMandatoryParameters(String someString, int someNumber) {
  // ...
}

// You are forced to send the defined parameters
// when using the function:
functionWithMandatoryParameters('some_string', 46);

// You can however specify that the parameters are optional:
// (note that the type must be defined as nullable, precisely because
// there's no guarantee that the caller will send a value)
functionWithOptionalParams(
  {String? optionalString, int? optionalNumber}) {
  // ...
}

// You can call this function without sending any values,
// or specifying a value for an optional parameter with its label:
functionWithOptionalParams();
functionWithOptionalParams(optionalString: 'some_string');
functionWithOptionalParams(
  optionalString: 'some_string', optionalNumber: 46);

// When defining optional parameters, you can set a default value
// that will be used in case that there is no value sent by the caller:
functionWithDefaultValue({String someString = 'default'}) {
  // ...
}

// The value of someString is 'default'
functionWithDefaultValue();
// The value of someString is 'some_string'
functionWithDefaultValue(someString: 'some_string');

// Lastly, you can even define mandatory named parameters with the
// 'required' keyword, this is useful to enhance code readability.
createUser(
  {required String username,
  required String name,
  required String surname,
  required String address,
  required String city,
  required String country}) {
// ...
}

createUser(
  username: 'Ghost',
  name: 'John',
  surname: 'Doe',
  address: '3590  Mill Street',
  city: 'Beaver',
  country: 'US');
```

### Composition with mixins

软件开发中最不流行的趋势之一是重组而不是继承，这意味着使用类似组件的元素向类添加功能，而不是从父类继承。这种方法允许我们轻松地添加封装的功能，而无需处理复杂的继承层次结构。

例如，假设您有一个登录逻辑，您可能希望在应用程序中的不同位置使用它。您可以使用这个逻辑创建一个组件(mixin) ，然后在需要时重用它:

```dart
abstract class AuthUtils {
  Future<User> login() async {
    // Here we can add the login logic that will later be reused
    // in any class that ads this mixin.
  }
}

class LoginPage extends StatefulWidget {
  LoginPage({Key? key}) : super(key: key);

  @override
  _LoginPageState createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> with AuthUtils {
  @override
  Widget build(BuildContext context) {
    return FutureBuilder<User>(
      future: login(), // Calling the mixin function
      builder: (BuildContext context, AsyncSnapshot<User> snapshot) {
        // ...
      },
    );
  }
}
```

这里的优点是，我们可以随心所欲地添加任意多的 mixin，而不是仅通过使用继承从一个父类继承。

### 总结

这些只是 Dart 提供给开发者的多个有用特性中的 4 个。如果你想了解更多，我建议你去参观 Dart 语言之旅，它以一种非常友好的方式解释了这门语言的每一个细节。

https://dart.dev/guides/language/language-tour

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

![](https://ducafecat.tech/img/public-qrcode.png)

## 往期

### 开源

#### GetX Quick Start

https://github.com/ducafecat/getx_quick_start

#### 新闻客户端

https://github.com/ducafecat/flutter_learn_news

### strapi 手册译文

https://getstrapi.cn

### 微信讨论群 ducafecat

### 系列集合

#### 译文

https://ducafecat.tech/categories/%E8%AF%91%E6%96%87/

#### 开源项目

https://ducafecat.tech/categories/%E5%BC%80%E6%BA%90/

#### Dart 编程语言基础

https://space.bilibili.com/404904528/channel/detail?cid=111585

#### Flutter 零基础入门

https://space.bilibili.com/404904528/channel/detail?cid=123470

#### Flutter 实战从零开始 新闻客户端

https://space.bilibili.com/404904528/channel/detail?cid=106755

#### Flutter 组件开发

https://space.bilibili.com/404904528/channel/detail?cid=144262

#### Flutter Bloc

https://space.bilibili.com/404904528/channel/detail?cid=177519

#### Flutter Getx4

https://space.bilibili.com/404904528/channel/detail?cid=177514

#### Docker Yapi

https://space.bilibili.com/404904528/channel/detail?cid=130578
