---
title: 在 Flutter 中编写自定义小部件(第1部分)ー EllipsizedText
tags: flutter
categories: 译文
date: 2021-05-31 00:00:00
---

![](2021-05-31-08-55-16.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://rlesovyi.medium.com/writing-custom-widgets-in-flutter-part-1-ellipsizedtext-a0efdc1368a8

## 代码

https://github.com/MatrixDev/Flutter-CustomWidgets

## 正文

![](2021-05-31-08-35-42.png)

声明式用户界面在 Flutter 是相当不错，易于使用，它是非常诱人的使用尽可能。但是很多时候，开发人员只是做得太过火了ーー用声明的方式编写所有东西，即使有时候任务可以以更强制性的方式更有效、更容易理解。

每个人都应该明白的---- 在陈述和命令式编程之间必须有一个平衡。每种方法都有自己的用途，每种方法在某些任务上都比其他方法更加出色。

在本系列文章中，我将描述如何通过从头创建自定义小部件来解决不同的问题。每一个都比前一个稍微复杂一点。

### 思考

在查看代码之前，我们需要知道一些基本的事情。

Widget ー只是一个不可变的(最好是 const)类，它包含 Elements 和 RenderObjects 的配置属性。它还负责创建上述元素和渲染对象。需要理解的重要事情ー小部件从不包含状态或任何业务逻辑，只是传递它们。

元素ー是负责实际 UI 树的实体。它包含对所有子元素的引用，以及(不像 Widget)对其父元素的引用。元素在大多数情况下都会被重用，除非键或小部件被更改。因此，如果 onlyWidget 属性被更改，即使分配了新的 Widget，Element 也将保持不变。

State ー只不过是 Element 内部的一个用户定义类，它还公开了一些来自它的回调。

RenderObject ーー负责实际尺寸的计算、子元素的放置、绘制、触摸事件的处理等。这些对象与 Android 或其他框架的经典视图非常相似。

为什么我们同时拥有元素和渲染对象？因为效率高。每个小部件都有各自的元素，但只有一些有渲染对象。由于这一点，很多布局，触摸和其他层次遍历调用可以省略。

### 代码

第一个例子是一个非常简单的小部件，它在文本不适合时用省略号缩放文本。为什么我们需要这样一个小部件时，内置的文本已经省略号支持你可能会问？答案很简单---- 到目前为止，它只是通过文字而不是字符来表达 https://github.com/flutter/flutter/issues/18761。所以如果你有一个非常长的单词在结尾ー大多数时候你只能看到这个单词的前几个字母，即使有足够的空间去填充。

那么，我们开始吧。Flutter 有许多内置的基类和 mixin，它们将帮助构建完全自定义的小部件。以下是其中的一些:

- LeafRenderObjectWidget 没有 child
- SingleChildRenderObjectWidget 一个 child
- MultiChildRenderObjectWidget 多个 child

在我们的例子中，我们将使用 LeafRenderObjectWidget，因为我们只需要渲染文本，并且不会有子节点:

```dart
enum Ellipsis { start, middle, end }

class EllipsizedText extends LeafRenderObjectWidget {
  final String text;
  final TextStyle? style;
  final Ellipsis ellipsis;

  const EllipsizedText(
    this.text, {
    Key? key,
    this.style,
    this.ellipsis = Ellipsis.end,
  }) : super(key: key);

  @override
  RenderObject createRenderObject(BuildContext context) {
    return RenderEllipsizedText()..widget = this;
  }

  @override
  void updateRenderObject(BuildContext context, RenderEllipsizedText renderObject) {
    renderObject.widget = this;
  }
}
```

我们创建了我们的 Widget，唯一不同寻常的是有两种方法:

- createRenderObject — 负责实际创建我们的 RenderObject
- updateRenderObject — 当 Widget 的数据发生变化但 RenderObject 保持不变时，将调用 updateRenderObject ー。在这种情况下，我们需要更新 RenderObject 中的数据，否则它将呈现旧文本

我还需要注意，将每个值从小部件复制到 RenderObject 是首选的。但是我会通过整个 Widget，因为不管怎样它们都是不可变的(而且我懒得编写所有的样板代码)。

现在让我们从实际的渲染对象开始:

```dart
class RenderEllipsizedText extends RenderBox {
  var _widgetChanged = false;
  var _widget = const EllipsizedText('');

  set widget(EllipsizedText widget) {
    if (_widget.text == widget.text &&
        _widget.style == widget.style &&
        _widget.ellipsis == widget.ellipsis) {
      return;
    }
    _widgetChanged = true;
    _widget = widget;
    markNeedsLayout();
  }
}
```

在这里，我们定义了所有的变量，并编写了一个 setter 来实际更新它们。还有一个检查值是否实际发生了更改的防护措施ー如果没有更改，则没有必要重新计算省略号和重绘文本。

现在我们需要布局渲染对象。

```dart
class RenderEllipsizedText extends RenderBox {
  // ...
  var _constraints = const BoxConstraints();

  @override
  void performLayout() {
    if (!_widgetChanged && _constraints == constraints && hasSize) {
      return;
    }

    _widgetChanged = false;
    _constraints = constraints;

    size =_ellipsize(
      minWidth: constraints.minWidth,
      maxWidth: constraints.maxWidth,
    );
  }
}
```

布局的过程相当简单。所有我们需要做的ー根据提供给我们的约束计算渲染对象的大小。约束只描述我们必须遵守的最小和最大规模。另外，如果没有任何变化，并且在以前的布局传递过程中已经计算了大小，则添加额外的检查。

实际创建省略号文本的过程相当繁琐，而且肯定有更好的解决方案，但我选择使用二进制搜索来寻找最佳匹配。

```dart
class RenderEllipsizedText extends RenderBox {
  // ...
  final _textPainter = TextPainter(textDirection: TextDirection.ltr);

  Size _ellipsize({required double minWidth, required double maxWidth}) {
    final text = _widget.text;

    if (_layoutText(length: text.length, minWidth: minWidth) > maxWidth) {
      var left = 0;
      var right = text.length - 1;

      while (left < right) {
        final index = (left + right) ~/ 2;
        if (_layoutText(length: index, minWidth: minWidth) > maxWidth) {
          right = index;
        } else {
          left = index + 1;
        }
      }
      _layoutText(length: right - 1, minWidth: minWidth);
    }

    return constraints.constrain(Size(_textPainter.width, _textPainter.height));
  }
}
```

我不会讲完所有这些逻辑(如果你愿意，你可以通过它来阅读)。但是重要的是 TextPainter 是用来计算文本大小的。如果文本大小长于我们的约束，我会尽量使它越来越短，直到它符合我们的约束。

`_layoutText` 用来计算我们裁剪后的文本大小:

```dart
double _layoutText({required int length, required double minWidth}) {
  final text = _widget.text;
  final style = _widget.style;
  final ellipsis = _widget.ellipsis;

  String ellipsizedText = '';

  switch (ellipsis) {
    case Ellipsis.start:
      if (length > 0) {
        ellipsizedText = text.substring(text.length - length, text.length);
        if (length != text.length) {
          ellipsizedText = '...' + ellipsizedText;
        }
      }
      break;
    case Ellipsis.middle:
      if (length > 0) {
        ellipsizedText = text;
        if (length != text.length) {
          var start = text.substring(0, (length / 2).round());
          var end = text.substring(text.length - start.length, text.length);
          ellipsizedText = start + '...' + end;
        }
      }
      break;
    case Ellipsis.end:
      if (length > 0) {
        ellipsizedText = text.substring(0, length);
        if (length != text.length) {
          ellipsizedText = ellipsizedText + '...';
        }
      }
      break;
  }

  _textPainter.text = TextSpan(text: ellipsizedText, style: style);
  _textPainter.layout(minWidth: minWidth, maxWidth: double.infinity);
  return _textPainter.width;
}
```

差不多就是这样了，我们剩下要做的就是——实际上画出我们的文本。

```dart
@override
void paint(PaintingContext context, Offset offset) {
  _textPainter.paint(context.canvas, offset);
}
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
