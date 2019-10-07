---
title: Flutter 零基础入门中文教学 - 11 基础组件 Text
date: 2019-10-6 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- Text 构造参数
- TextStyle 样式构造参数
- Text.rich、RichText、TextSpan 处理复杂字符显示

## Text

Text Widget，从名字也可以看出，在 Flutter 里是用来负责显示文本信息的一个组件，功能类似于 Android 的 TextView、HTML 的一些文本标签等等，属于基础组件。

- 基础用法

```dart
Text('hello word!');
```

### 构造函数

- Text

```dart
const Text(
    //要显示的文字内容
    this.data,
   {
    //key类似于id
    Key key,
    //文字显示样式和属性
    this.style,
    this.strutStyle,
    //文字对齐方式
    this.textAlign,
    //文字显示方向
    this.textDirection,
    //设置语言环境
    this.locale,
    //是否自动换行
    this.softWrap,
    //文字溢出后处理方式
    this.overflow,
    //字体缩放
    this.textScaleFactor,
    //最大显示行数
    this.maxLines,
    //图像的语义描述，用于向Andoid上的TalkBack和iOS上的VoiceOver提供图像描述
    this.semanticsLabel,
  })
```

- TextStyle

style 属性比较常用，传入的是 TextStyle 对象，我们细看下它可以配置哪些属性样式。

```dart
const TextStyle({
    //是否继承父类组件属性
    this.inherit = true,
    //字体颜色
    this.color,
    //文字大小，默认14px
    this.fontSize,
    //字体粗细
    this.fontWeight,
    //字体样式,normal或italic
    this.fontStyle,
    //字母间距，默认为0，负数间距缩小，正数间距增大
    this.letterSpacing,
    //单词间距，默认为0，负数间距缩小，正数间距增大
    this.wordSpacing,
    //字体基线
    this.textBaseline,
    //行高
    this.height,
    //设置区域
    this.locale,
    //前景色
    this.foreground,
    //背景色
    this.background,
    //阴影
    this.shadows,
    //文字划线，下换线等等装饰
    this.decoration,
    //划线颜色
    this.decorationColor,
    //划线样式，虚线、实线等样式
    this.decorationStyle,
    //描述信息
    this.debugLabel,
    //字体
    String fontFamily,
    List<String> fontFamilyFallback,
    String package,
  })
```

### 代码示例

- 颜色、大小、样式

```dart
Text('字体24下划线',
     style: TextStyle(
       color: Colors.blue, // 蓝色
       fontSize: 24,				// 24 号字体
       decoration: TextDecoration.underline, // 下划线
     )),
```

- 缩放、加粗

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

- 文字溢出

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

## Text.rich、RichText 、TextSpan

### 构造函数

可以在 Text 里加入一些 Span 标签，对某部分文字进行个性化改变样式，如加入 @ 符号，加入超链接、变色、加表情等等。Text.rich(…) 等价于 RichText(...)，用哪个都可以。

```dart
const Text.rich(
    // 样式片段标签TextSpan
    this.textSpan,
  {
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
  })


const RichText({
    Key key,
    // 样式片段标签TextSpan
    @required this.text,
    this.textAlign = TextAlign.start,
    this.textDirection,
    this.softWrap = true,
    this.overflow = TextOverflow.clip,
    this.textScaleFactor = 1.0,
    this.maxLines,
    this.locale,
    this.strutStyle,
  })
```

textSpan 类型是 TextSpan ，其它参数同上

```dart
const TextSpan({
    //样式片段
    this.style,
    //要显示的文字
    this.text,
    //样式片段TextSpan数组，可以包含多个TextSpan
    this.children,
    //用于手势进行识别处理,如点击跳转
    this.recognizer,
  })
```

### 代码示例

- 拼接字符

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

- 添加交互

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
> TapGestureRecognizer tap 点击手势

## 代码

https://github.com/ducafecat/flutter-learn/tree/master/text_widget

## 参考

- [Text class](https://api.flutter.dev/flutter/widgets/Text-class.html)
- [TextSpan class](https://api.flutter.dev/flutter/painting/TextSpan-class.html)
- [RichText class](https://api.flutter.dev/flutter/widgets/RichText-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
