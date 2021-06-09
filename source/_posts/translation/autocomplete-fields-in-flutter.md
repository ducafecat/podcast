---
title: Flutter 自动提示文本框
tags: flutter
categories: 译文
date: 2021-06-10 00:00:00
---

![](2021-06-10-05-45-47.png)

## 猫哥说

当你开始用多屏，你就会需要屏幕越来越大、越来越多，不信你试试，会找各种理由来说服自己。

确实一块辅助屏对效率有很大的提升，我是建议辅助屏幕小些，或者说你自己舒服就行。

今天推荐的文章是提示输入框，虽然简单，但是很实用。

这种搜索提示框，可以放在登录账号输入框，搜索输入框提示，基础数据输入框。。。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## 原文

> https://theiskaa.medium.com/autocomplete-fields-in-flutter-ec4eb6ec5ad7

## 代码

- https://github.com/theiskaa/anon

## 参考

- https://flutter.dev/docs/development/packages-and-plugins/using-packages
- https://pub.dev/packages/field_suggestion

## 正文

![](2021-06-10-05-37-25.png)

注意: 如果你是新手，想要创建非常基本的高级自动完成字段，那么这篇文章是为你准备的，继续！

每个人都知道自动补全领域，有我们左右。我们可以在移动应用程序、网站、桌面应用程序等中看到它们。你有没有试过在 Flutter 中创建自动补全字段？啊，这对新手来说有点难。当你是一个新的学习者，这似乎是不可能的。

好吧那包裹可以帮我们。(我想我们大多数人都知道软件包/插件，如果你不知道，不要担心，只要按照这个官方文档的软件包和完整的阅读然后回来!).

https://flutter.dev/docs/development/packages-and-plugins/using-packages

我写的其中一个是 `field_suggestion` 包，它可以帮助我们创建 Basic 或高级的自动完成字段。

https://pub.dev/packages/field_suggestion

在开始解释如何使用这个软件包之前，我想向您展示一个我使用这个软件包的应用程序的概览。

![](2021-06-10-05-37-13.png)

这是一个名为 Anon 的开源应用程序的截图。在那里我们使用字段建议作为我们的搜索栏。

查看详细信息: https://github.com/theiskaa/anon

好了，够了，让我们开始使用现场建议。

### Requirements

一个 textedingcontroller 和建议列表。

```dart
final textEditingController = TextEditingController();

// And
List<String> suggestionList = [
 'test@gmail.com',
 'test1@gmail.com',
 'test2@gmail.com',
];

// Or

List<int> numSuggestions = [
  13187829696,
  13102743803,
  15412917703,
];

// Or
// Note: Take look at [Class suggestions] part.
List<UserModel> userSuggestions = [
  UserModel(email: 'test@gmail.com', password: 'test123'),
  UserModel(email: 'test1@gmail.com', password: 'test123'),
  UserModel(email: 'test2@gmail.com', password: 'test123')
];
```

### 非常基本的用法

这里我们有，只是文本编辑控制器和建议列表(作为字符串)

![](2021-06-10-05-39-16.png)

```dart
FieldSuggestion(
  textController: textEditingController,
  suggestionList: suggestionList,
  hint: 'Email',
),
```

### 外部控制

在这里，我们刚刚用 `GestureDetector` 包装了脚手架来处理屏幕上的手势。现在我们可以在点击屏幕的时候关闭盒子。(您可以在任何地方使用 `BoxController` 进行此操作，其中使用了 `FieldSuggestion`)。

```dart
class Example extends StatelessWidget {
   final _textController = TextEditingController();
   final _boxController = BoxController();

   @override
   Widget build(BuildContext context) {
     return GestureDetector(
       onTap: () => _boxController.close(),
       child: Scaffold(
         body: Center(
           child: FieldSuggestion(
             hint: 'test',
             suggestionList: [], // Your suggestions list here...
             boxController: _boxController,
             textController: _textController,
           ),
         ),
       ),
     );
   }
 }
```

### Class suggestions

> 注意: 你必须在你的模型类中使用 `toJson` 方法。如果你想在建议列表中使用它。

```dart
class UserModel {
  final String? email;
  final String? password;

  const UserModel({this.email, this.password});

  // If we wanna use this model class into FieldSuggestion.
  // Then we must have toJson method, like this:
  Map<String, dynamic> toJson() => {
        'email': this.email,
        'password': this.password,
      };
}
```

如果我们给出一个 `userSuggestions`，它是 `List<usermodel>` 。然后我们必须添加 searchBy 属性。我们的模型只有电子邮件和密码，对吗？所以我们可以这样实现: searchBy: `email` 或者 searchBy: `password`。

```dart
FieldSuggestion(
  hint: 'Email',
  textController: textEditingController,
  suggestionList: userSuggestions,
  searchBy: 'email' // Or 'password'
),
```

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
