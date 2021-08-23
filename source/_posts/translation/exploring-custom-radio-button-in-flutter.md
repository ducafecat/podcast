---
title: Flutter 自定义单选按钮
tags: flutter
categories: 译文
date: 2021-08-24 00:00:00
---

![](2021-08-24-07-49-36.png)

## 原文

> https://medium.com/flutterdevs/exploring-custom-radio-button-in-flutter-4a93a7892185

## 正文

> 了解如何创建一个自定义单选按钮在您的 Flutter 应用程序

![](2021-08-24-07-36-53.png)

单选按钮则称为选择按钮，它保存布尔值。它允许客户从一组预定义的选择中选择一个选择。这个组件使它不完全相同于一个复选框，我们可以选择一个以上的替代和未选择的状态重新建立。我们可以组织至少两个单选按钮的集合，并在屏幕上显示为带有白色区域的圆形孔用于未选择或圆点用于选择。

我们同样可以给每个相关的单选按钮一个标签，描绘单选按钮地址的决定。一个单选按钮可以选择通过点击鼠标在圆形孔或利用控制台备用方式。

在这个博客，我们将探索自定义 Flutter 单选按钮。我们将看到如何实现一个自定义单选按钮演示程序，以及如何在您的颤振应用程序创建。

### 简介

Flutter 允许我们利用 Radio 小部件制作单选按钮。用这个小部件制作的单选按钮由一个空白的外部圆和一个强内部圆组成，最后一个按钮显示在选择状态。时不时地，你可能需要制作一个 radio gathering，其替代方案利用自定义设计，而不是传统的 radio gathering 。本文举例说明了如何使用定制 catches 进行 radio gathering。

> Demo Module :
>
> 演示模块:

![](demo.gif)

这个演示视频显示了如何创建一个自定义单选按钮在 Flutter。它显示了如何自定义单选按钮将工作在您的 Flutter 应用程序。它展示了当用户点击按钮时，单选组将如何使用自定义按钮。动画的。它会显示在你的设备上。

### 如何实现 dart 文件中的代码:

你需要分别在你的代码中实现它:

> 在 lib 文件夹中创建一个名为 `radio_opton.dart` 的新 dart 文件。

因为单选按钮包含一个标签，所以我们不能使用单选按钮。综上所述，我们将创建一个自定义类，它可以用于做出称为 MyRadioOption 的选择。受 Flutter 的 Radio 小部件的激励，该类具有 value、 groupValue 和 onChanged 属性。该属性的价值解决了替代品的价值，在类似群体的所有选择中，它应该是非同寻常的。

groupValue 属性是当前选定的值。如果选项值与 groupvalue 匹配，则该选项处于选定状态。onChanged 属性存储当用户选择一个选项时将调用的回调函数。当用户选择一个选项时，回调函数负责更新 groupValue。此外，我们还添加了标签和文本属性，因为我们需要在按钮上显示标签，并在按钮的右侧显示文本。

groupValue 属性是目前选择的值。如果选择值与 groupvalue 匹配，则替换处于选择状态。onChanged 属性存储回调函数，客户机选择时将考虑该函数。当客户机选择一个替代方案时，回调函数有义务更新 groupValue。此外，我们还添加了标签和文本属性，因为我们需要在按钮上显示名称，并在按钮的右侧显示内容。

> 下面是类的属性和构造函数。我们利用一个常规的类 T，理由是这个值可以是任何类型。

```dart
class MyRadioOption<T> extends StatelessWidget {

  final T value;
  final T? groupValue;
  final String label;
  final String text;
  final ValueChanged<T?> onChanged;

  const MyRadioOption({
    required this.value,
    required this.groupValue,
    required this.label,
    required this.text,
    required this.onChanged,
  });

  @override
  Widget build(BuildContext context) {
    // TODO implement
  }
}
```

然后，我们将制作布局。按钮是一个圆圈，里面有名字。为了制作圆形，使用一个圆形边框作为形状的图形装饰容器。名称可以使用 Text 作为容器的子部件。然后，在这一点上，我们可以创建一个由按钮和文本小部件组成的 Row。

```dart
import 'package:flutter/material.dart';

class MyRadioOption<T> extends StatelessWidget {

  final T value;
  final T? groupValue;
  final String label;
  final String text;
  final ValueChanged<T?> onChanged;

  const MyRadioOption({
    required this.value,
    required this.groupValue,
    required this.label,
    required this.text,
    required this.onChanged,
  });

  Widget _buildLabel() {
    final bool isSelected = value == groupValue;

    return Container(
      width: 30,
      height: 30,
      decoration: ShapeDecoration(
        shape: CircleBorder(
          side: BorderSide(
            color: Colors.black,
          ),
        ),
        color: isSelected ? Colors.cyan : Colors.white,
      ),
      child: Center(
        child: Text(
          value.toString(),
          style: TextStyle(
            color: isSelected ? Colors.white : Colors.cyan,
            fontSize: 20,
          ),
        ),
      ),
    );
  }

  Widget _buildText() {
    return Text(
      text,
      style: const TextStyle(color: Colors.black, fontSize: 24),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      margin: EdgeInsets.all(8),
      child: InkWell(
        onTap: () => onChanged(value),
        splashColor: Colors.cyan.withOpacity(0.5),
        child: Padding(
          padding: EdgeInsets.all(5),
          child: Row(
            children: [
              _buildLabel(),
              const SizedBox(width: 10),
              _buildText(),
            ],
          ),
        ),
      ),
    );
  }
}
```

> 在 lib 文件夹中创建一个名为 `custom_radio_demo.dart` 的新 dart 文件。

下面是一个类，其中我们使用 MyRadioOption 作为选择的无线电组。有一个状态变量 \_ groupValue 和一个 ValueChanged 函数，这是当客户机选择一个备选方案时要考虑的回调函数。

```dart
String? _groupValue;

ValueChanged<String?> _valueChangedHandler() {
  return (value) => setState(() => _groupValue = value!);
}
```

> 在主体中，如何调用 MyRadioOption 的构造函数。

```dart
MyRadioOption<String>(
  value: '1',
  groupValue: _groupValue,
  onChanged: _valueChangedHandler(),
  label: '1',
  text: 'Phone Gap',
),
MyRadioOption<String>(
  value: '2',
  groupValue: _groupValue,
  onChanged: _valueChangedHandler(),
  label: '2',
  text: 'Appcelerator',
),
```

当我们运行应用程序时，我们应该获得屏幕输出，就像下面的屏幕截图一样。

![](2021-08-24-07-42-36.png)

Final Output 最终输出

### 代码

```dart
import 'package:flutter/material.dart';
import 'package:flutter_custom_radio_button/radio_option.dart';

class CustomRadioDemo extends StatefulWidget {

  @override
  State createState() => new _CustomRadioDemoState();
}

class _CustomRadioDemoState extends State<CustomRadioDemo> {

  String? _groupValue;

  ValueChanged<String?> _valueChangedHandler() {
    return (value) => setState(() => _groupValue = value!);
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: AppBar(
        automaticallyImplyLeading: false,
        title: const Text('Flutter Custom Radio Button Demo'),
        backgroundColor: Colors.cyan,
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(10.0),
            child: Text("Best Cross-Platform Mobile App Development Tools for 2021",
              style: TextStyle(fontWeight: FontWeight.bold,fontSize: 18),),
          ),
          SizedBox(height: 10,),
          MyRadioOption<String>(
            value: '1',
            groupValue: _groupValue,
            onChanged: _valueChangedHandler(),
            label: '1',
            text: 'Phone Gap',
          ),
          MyRadioOption<String>(
            value: '2',
            groupValue: _groupValue,
            onChanged: _valueChangedHandler(),
            label: '2',
            text: 'Appcelerator',
          ),
          MyRadioOption<String>(
            value: '3',
            groupValue: _groupValue,
            onChanged: _valueChangedHandler(),
            label: '3',
            text: 'React Native',
          ),
          MyRadioOption<String>(
            value: '4',
            groupValue: _groupValue,
            onChanged: _valueChangedHandler(),
            label: '4',
            text: 'Native Script',
          ),
          MyRadioOption<String>(
            value: '5',
            groupValue: _groupValue,
            onChanged: _valueChangedHandler(),
            label: '5',
            text: 'Flutter',
          ),
        ],
      ),
    );
  }
}
```

### 结语

在本文中，我已经解释了自定义单选按钮的基本结构，您可以根据自己的选择修改这段代码。这是一个小的介绍自定义单选按钮对用户交互从我这边，它的工作使用 Flutter。

我希望这个博客将提供您尝试在您的 Flutter 项目的自定义单选按钮充足的信息。我们将向您展示介绍是什么？.这是一个如何制作自定义单选按钮的例子。从根本上说，每一种选择都应该具有价值和集团价值。组值应该是类似组中所有备选项中非常相似的东西。当客户选择一个替代方案时，组值会更新，所以请尝试一下。

如果我做错了什么，请在评论中告诉我，我很乐意改进。

鼓掌如果这篇文章对你有帮助。

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
