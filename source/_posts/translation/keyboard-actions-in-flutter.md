---
title: Flutter 的 keyboard_actions 插件
tags: flutter
categories: 译文
date: 2021-09-06 00:00:00
---

![](2021-09-05-21-39-17.png)

## 原文

> https://medium.com/flutterdevs/keyboard-actions-in-flutter-ae4781286806

## 代码

https://github.com/flutter-devs/KeyboardAction

## 参考

- https://pub.dev/packages/keyboard_actions

## 正文

了解如何在您的 Flutter 应用程序自定义默认键盘

![](2021-09-05-21-28-04.png)

Flutter 中的键盘动作

安卓/IoS 提供的键盘没有隐藏键盘的按钮，这给用户带来了很多不便。当我们的应用程序有许多需要在工具栏上显示操作键和处理定义为字段的函数的 textfield 时。键盘操作是当用户点击当前字段时指示该字段操作的键。

在这篇文章中，我将演示如何使用包含字段的表单输入在应用程序中显示键盘操作。我们还将实现一个演示程序，并使用包 `keyboard action` 来演示这些特性。我试图用一种简单的方式来解释我的项目

# 简介:

`KEYBOARD_ACTION` 提供了几个软件包，使您的设备键盘可定制。今天，我们讨论 KEYBOARD action。在 iOS 中有一个众所周知的问题，当我们使用数字输入字段时，它不会显示键盘内部/上方的完成按钮。因此，键盘操作提供了各种功能，有助于克服用户和开发人员目前面临的问题。

https://pub.dev/packages/keyboard_actions

> 特点:

- 完成键盘按钮\(您可以自定义按钮\)
- 在文本字段之间上下移动\(可以隐藏设置\)`nextFocus: false`\).
- 键盘栏定制
- 键盘栏下方的自定义页脚小部件
- 用简单的方法创建你自己的键盘
- 你可以在安卓、 iOS 或者两个平台上使用它
- 与对话框兼容

# 设立项目:

**第一步:** 使用包装

## keyboard_actions | Flutter Package

## 键盘操作 | Flutter Package

### 以一种简单的方式为 Android/iOS 键盘添加特性。因为安卓/iOS 提供的键盘..

pub.dev

https://pub.dev/packages/keyboard_actions

在 pubspec.yaml 文件的依赖关系中添加 youtube player_iframe 插件，然后运行 \$flutter pub get 命令。

```dart
dependencies:
  keyboard_actions: ^3.4.4
```

**步骤 2:** 将包导入为

```dart
import 'package:keyboard_actions/keyboard_actions.dart';
```

# **_Code Implementation:_**

# 代码实施:

**1.** 创建一个新的 dart 文件，名为`home_screen.dart` . 文件夹来设计用户界面，并编写您希望在项目中实现的逻辑

**2.** 我在 flutter demo 项目中构建了长长的 Forms，并在 Android 上运行了这个应用程序。如果我们使用的是 IOS 设备，那么它不会显示**done 完成** 在 iOS 系统中，当我们使用数字输入字段时，在 Android 系统中，键盘内/上方的按钮不会显示

```dart
Card(
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.circular(10.0),
  ),
  elevation: 8.0,
  child: Container(
    padding: EdgeInsets.only(left: 12),
    child: Form(
      key: _formKey,
      child: SingleChildScrollView(
        child: Column(
          children: <Widget>[
            TextFormField(
              decoration: InputDecoration(
                  labelText: 'Input Number with Custom Footer'),
              controller: _customController,
              focusNode: _nodeText6,
              keyboardType: TextInputType._number_,
            ),
            TextFormField(
              decoration: InputDecoration(labelText: 'Input Number'),
              focusNode: _nodeText1,
              keyboardType: TextInputType._number_,
                textInputAction: TextInputAction.next
            ),
            TextFormField(
              decoration: InputDecoration(
                  labelText: 'Custom cross Button'),
              focusNode: _nodeText2,
              keyboardType: TextInputType._text_,
            ),
            TextFormField(
              decoration: InputDecoration(
                  labelText: 'Input Number with Custom Action'),
              focusNode: _nodeText3,
              keyboardType: TextInputType._number_,
            ),
            TextFormField(
              decoration: InputDecoration(
                  labelText: 'Input Text without Done button'),
              focusNode: _nodeText4,
              keyboardType: TextInputType._text_,
            ),
            TextFormField(
              decoration: InputDecoration(
                  labelText: 'Input Number with Toolbar Buttons'),
              focusNode: _nodeText5,
              keyboardType: TextInputType._number_,
            ),
          ],
        ),
      ),
    ),
  ),
);
```

**3.** 现在，要在项目中添加键盘操作，您需要将所有 TextFormField 包装在 Widget KeyboardAction 下，这个 Widget KeyboardAction 需要 keyboardactivesconfig 配置才能在键盘上添加配置。

```dart
return KeyboardActions(
    tapOutsideBehavior: TapOutsideBehavior.translucentDismiss,
    _//autoScroll: true,_ config: _buildConfig(context),
    child: Card(
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(10.0),
      ),
      elevation: 8.0,
      child: Container(
        padding: EdgeInsets.only(left: 12),
        child: Form(
          key: _formKey,
          child: SingleChildScrollView(
            child: Column(
              children: <Widget>[
                TextFormField(
                  decoration: InputDecoration(
                      labelText: 'Input Number with Custom Footer'),
                  controller: _customController,
                  focusNode: _nodeText6,
                  keyboardType: TextInputType._number_,
                ),
```

**4.** 提供了一个功能，当我们按下设备屏幕上键盘以外的任何地方时，可以关闭键盘。所以，我们需要加上这一行

```dart
tapOutsideBehavior: TapOutsideBehavior.translucentDismiss,
```

**5.** 我们应该初始化分配给不同文本字段的 FocusNode 对象。因此，开发人员进行定制，因为它允许键盘将焦点集中在这个小部件上。

```dart
final FocusNode _nodeText1 = FocusNode();//Add In TextFormField TextFormField(
              decoration: InputDecoration(
                  labelText: 'Input Number with Toolbar Buttons'),
              focusNode: _nodeText1,
              keyboardType: TextInputType._number_,
            )
```

**6.** 我们将定义 keyboardansconfig。包装器为单个配置键盘的动作栏。在 keyboardansconfig 中，我们根据您的需求为每个 TextFormField 单独定义由键盘执行的操作。我们可以自定义键盘颜色，键盘和内容之间的分隔线颜色，显示箭头前/后移动输入之间的焦点。

```dart
KeyboardActionsConfig _buildConfig(BuildContext context) {
  return KeyboardActionsConfig(
    keyboardActionsPlatform: KeyboardActionsPlatform.ALL,
    keyboardBarColor: Colors._grey_[200],
    keyboardSeparatorColor: Colors._redAccent_,
    nextFocus: true,
    actions: [
```

**7.** 现在，我们将根据不同的 TextFormField 中的需求定义动作。

```dart
actions: [
  KeyboardActionsItem(
    focusNode: _nodeText1,
  ),
```

**8.** 要在应用程序中显示带有自定义页脚的输入，您需要在您的 KeyboardActionsItem 中实现下面的代码，在这里我们必须在 Text 小部件中传递 TextController。

```dart
KeyboardActionsItem(
  focusNode: _nodeText6,
  footerBuilder: (_) => PreferredSize(
      child: SizedBox(
          height: 40,
          child: Center(
            child: Text(_customController.text),
          )),
      preferredSize: Size.fromHeight(40)),
),
```

**9.** 为了在你的应用程序中显示自定义对话框，将这个逻辑添加到 KeyboardActionsItem 中指定的焦点节点。

```dart
KeyboardActionsItem(
  focusNode: _nodeText3,
  onTapAction: () {
    showDialog(
        context: context,
        builder: (context) {
          return AlertDialog(
            content: Text("Show Custom Action"),
            actions: <Widget>[
              FlatButton(
                child: Text("OK"),
                onPressed: () => Navigator._of_(context).pop(),
              )
            ],
          );
        });
  },
),
```

> 当我们运行应用程序时，我们得到屏幕的输出视频，如下所示，用户可以观察输出。

![](demo.gif)

# 结语:

在本文中，我已经简单地介绍了 KeyboardAction 包的基本结构; 您可以根据自己的选择修改这段代码，也可以根据自己的需求使用这个包。这是一个小的介绍键盘定制用户交互从我这边，它的工作使用 Flutter 。

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
