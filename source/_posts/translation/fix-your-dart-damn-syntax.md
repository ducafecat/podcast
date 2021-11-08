---
title: fix-your-dart-damn-syntax
tags: flutter
categories: 译文
date: 2021-11-09 00:00:00
---

![](2021-11-08-09-08-24.png)

## 参考

- https://dart.dev/guides/language/effective-dart
- https://dart.dev/guides/language/effective-dart/style
- https://dart-lang.github.io/linter/lints/index.html

## 正文

当我检查其他项目的时候，有些事情经常困扰着我，那就是我们大多数人不遵守 Dart 语法规则

我知道你可能来自另一种语言背景，但是你现在使用的是 Dart，而 Dart 做的有些不同。

实际上，Dart 文档完美地解释了一切，但是我们大多数人都懒得阅读整个文档。所以我决定为我们的懒虫们写一个总结。

希望你能从中受益！

### 文件夹/档案

```dart
lower_snake_caseNOT
FolderName
fileName
file-name
```

### 类

```dart
UpperCamelCase
```

### 函数

```dart
lowerCamelCase
```

### 变量

```dart
lowerCamelCase
```

### extensions 扩展

```dart
UpperCamelCase
```

### mixins 混合

```dart
UpperCamelCase
```

### constants 常量

```dart
CAPITALIZE_EVERY_DAMN_LETTER // NO

lowerCamelCase // yes
```

### enums 枚举

```dart
enum Name { ENUM, NAME } // WRONG!!

enum Name { enum, name } // RIGHT!!
```

### 对于未使用的回调参数常量名，最好使用 `_` `__`

```dart
// IF YOU WON'T USE DON'T MENTION IT

futureOfVoid.then((unusedParameter) => print('Operation complete.'));

futureOfVoid.then((_) => print('Operation complete.'));
```

### 更喜欢使用字符串模板来组合字符串和值

```dart
// GOOD BOY
'Hello, $name! You are ${year - birth} years old.';

// BAD BOY
'Hello, ' + name + '! You are ' + (year - birth).toString() + ' y...';
```

### 避免使用不必要的 `getters` 和 `setters`

```dart
// GOOD
class Box {
  var contents;
}

// BAD
class Box {
  var _contents;
  get contents => _contents;
  set contents(value) {
    _contents = value;
  }
}
```

### 尽可能的写上类型定义

```dart
add(a,b) => a + b; // DAMN WRONG

int add(int a, int b) => a + b;  // HELL YEAH

BUT

final List<String> users = <String>[];  // THAT'S OVERKILL

final List<String> users = []; // GREAT
final users = <String>[]; // WONDERFUL
```

### `new` 可以不要用了

```dart
// I'm old dude
new Container();

// I'm a brand new energetic open-minded sexy young dude
Container();
```

对不起，如果我有点咄咄逼人，但请立即修复您的代码，否则我会找到你。此外，我想如果我遇到新的沉船时间增加更多的提示，所以请小心。

## 谢谢你的阅读

---

© 猫哥

- https://ducafecat.tech/

- https://github.com/ducafecat

- 微信群 ducafecat

- b 站 https://space.bilibili.com/404904528

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
