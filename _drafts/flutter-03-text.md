---
title: Flutter移动开发 - 03 基础控件 Text、RichText、TextSpan
date: 2019-05-21 00:41:21
tags: flutter
categories: Flutter移动开发
---

## 课程目标

![](image-20190522183923353.png)

- Text 构造参数
- TextStyle 样式构造参数
- Text.rich、RichText、TextSpan 处理复杂字符显示

## 1. Text

Text 就是用来展示文本的

### 1.1 基础用法

```dart
import 'package:flutter/material.dart';

main(List<String> args) {
  runApp(MaterialApp(
      home: Scaffold(
          appBar: AppBar(title: Text('my app')), 
        			body: Text('hello word!'))));
}
```

> Text('hello word!')

### 1.2 构造函数

```dart
  /// Creates a text widget.
  ///
  /// If the [style] argument is null, the text will use the style from the
  /// closest enclosing [DefaultTextStyle].
  ///
  /// The [data] parameter must not be null.
  const Text(
    data, {
    Key key,
    style,
    strutStyle,
    textAlign,
    textDirection,
    locale,
    softWrap,
    overflow,
    textScaleFactor,
    maxLines,
    semanticsLabel,
  }) : assert(
         data != null,
         'A non-null String must be provided to a Text widget.',
       ),
       textSpan = null,
       super(key: key);
```

### 1.3 构造参数表

| 名称            | 说明             |
| --------------- | ---------------- |
| data            | 文本             |
| style           | 样式             |
| strutStyle      | 支柱风格         |
| textAlign       | 对齐方式         |
| textDirection   | 文本方向         |
| locale          | 语言环境         |
| softWrap        | 自动换行         |
| overflow        | 文字溢出处理方式 |
| textScaleFactor | 字体缩放         |
| maxLines        | 最大显示行数     |
| semanticsLabel  | 图像描述         |

### 1.4 style 样式参数表

| 名称                | 说明                       |
| ------------------- | -------------------------- |
| inherit = true      | 是否继承父类组件属性       |
| color               | 字体颜色                   |
| backgroundColor     | 背景颜色                   |
| fontSize            | 文字大小，默认 14px        |
| fontWeight          | 字体粗细                   |
| fontStyle           | 字体样式                   |
| letterSpacing       | 字母间距                   |
| wordSpacing         | 单词间距                   |
| textBaseline        | 字体基线                   |
| height              | 行高                       |
| locale              | 设置区域                   |
| foreground          | 前景色                     |
| background          | 背景色                     |
| shadows             | 阴影                       |
| decoration          | 文字划线                   |
| decorationColor     | 划线颜色                   |
| decorationStyle     | 划线样式，虚线、实线等样式 |
| decorationThickness | 划线厚度                   |
| debugLabel          | 描述信息                   |
| String fontFamily   | 字体                       |

### 1.5 代码示例

#### 1.5.1 颜色、大小、样式

```dart
Text('字体24下划线',
     style: TextStyle(
       color: Colors.blue, // 蓝色
       fontSize: 24,				// 24 号字体
       decoration: TextDecoration.underline, // 下划线
     )),
```

#### 1.5.2 缩放、加粗

```dart
Text('放大加粗',
     textScaleFactor: 1.2, // 放大 1.2
     style: TextStyle(
       fontWeight: FontWeight.bold,	// 加粗 bold
       fontSize: 24,								// 24 号字体
       color: Colors.green,				// 绿色
       decoration: TextDecoration.none, // 不要下滑线
     )),
```

#### 1.5.3 文字溢出

```dart
Text(
  '缩放，Each line here is progressively more opaque. The base color is material.Colors.black, and Color.withOpacity is used to create a derivative color with the desired opacity. The root TextSpan for this RichText widget is explicitly given the ambient DefaultTextStyle, since RichText does not do that automatically. The inner TextStyle objects are implicitly mixed with the parent TextSpans TextSpan.style.',
  textScaleFactor: 1.0,
  textAlign: TextAlign.center,
  softWrap: true,
  maxLines: 3, // 3 行
  overflow: TextOverflow.ellipsis, // 剪切 加省略号
  style: TextStyle(
    fontWeight: FontWeight.bold,
    fontSize: 18,
  )),
```

## 2. Text.rich、RichText 、TextSpan

处理复杂文本，拼接、手势、个性

### 2.1 调用关系

- Text.rich 构造函数

```dart
  const Text.rich(
    this.textSpan, {
    Key key,
    this.style,
    this.strutStyle,
    this.textAlign,
    this.textDirection,
    this.locale,
    this.softWrap,
    this.overflow,
    this.textScaleFactor,
    this.maxLines,
    this.semanticsLabel,
  }) : assert(
         textSpan != null,
         'A non-null TextSpan must be provided to a Text.rich widget.',
       ),
       data = null,
       super(key: key);
```

textSpan 类型是 TextSpan ，其它参数同上

- TextSpan 构造函数

```dart
  const TextSpan({
    this.style,
    this.text,	// 文字
    this.children, // 子元素
    this.recognizer, // 交互
    this.semanticsLabel, // 描述
  });
```

### 2.2 代码示例

#### 2.2.1 拼接字符

```dart
Text.rich(TextSpan(
  text: 'TextSpan',
  style: TextStyle(
    color: Colors.red,
    fontSize: 24.0,
  ),
  children: <TextSpan>[
    new TextSpan(
      text: 'aaaaa',
      style: new TextStyle(
        color: Colors.blueGrey,
      ),
    ),
    new TextSpan(
      text: 'bbbbbb',
      style: new TextStyle(
        color: Colors.cyan,
      ),
    ),
  ],
)),
```

#### 2.2.2 添加交互

```dart
Text.rich(TextSpan(
  children: <TextSpan>[
    new TextSpan(
      text: 'Tap点击',
      style: new TextStyle(
        color: Colors.blueGrey,
      ),
      recognizer: new TapGestureRecognizer()
      ..onTap = () {
        //增加一个点击事件
        print('被点击了');
      },
    ),
  ],
)),
```

> recognizer 用来识别事件
>
> TapGestureRecognizer tap点击手势

## 代码

https://github.com/ducafecat/flutter-learn/tree/master/p03_text

## 参考

- [Text class](https://api.flutter.dev/flutter/widgets/Text-class.html)
- [TextSpan class](https://api.flutter.dev/flutter/painting/TextSpan-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)