---
title: Flutter 编写自定义组件 Part2.a ChildSize(with helpers)
tags: flutter
categories: 译文
date: 2021-06-21 05:55:22
---

![](2021-06-21-06-23-23.png)

## 猫哥说

这是自定义组件的第二篇, [第一篇点这里](https://ducafecat.tech/2021/05/31/translation/writing-custom-widgets-in-flutter-part-1-ellipsizedtext/)

有的时候我们需要做些很基础的组件或者工具，我们需要控制渲染、尺寸变化、预处理、销毁

这篇文章是写关于如何封装一个响应式的 ChangeSize 组件，组件继承了 SingleChildRenderObjectWidget，如果你也在写类似的功能，用这个父类可以快速的上手。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://rlesovyi.medium.com/writing-custom-widgets-in-flutter-part-2-singlechildrenderobjectwidget-5637fecdf9bb

## 代码

https://github.com/MatrixDev/Flutter-CustomWidgets

## 参考

- https://pub.flutter-io.cn/packages/get#reactive-state-manager
- https://dart.dev/guides/language/extension-methods

## 正文

![](2021-06-21-05-58-59.png)

### 介绍

我写第二篇文章的时候到了。这次它将是一个非常简单的小部件，当它的子大小发生变化时，它会通知我们。这个任务非常简单，但是它的主要目的是向您展示如何在 RenderObject 中管理子对象。

当我刚开始学习 Flutter 的时候，它是我的问题之一，甚至是完成 Udemy 的课程，很遗憾，我没有得到我的答案。在 StackOverflow 上，人们建议在 Widget 上使用 GlobalKey，找到它的 RenderObject 并获取它的大小。

虽然上述解决方案没有任何问题，但我仍然不喜欢它的一些地方:

- 它改变了 Widget 元素的销毁方式
- 您需要在声明性小部件结构中添加命令式代码
- 没有能力实际跟踪小部件的大小，它需要根据需要拉

一般来说，我想要达到的目标是:

```dart
return ChildSize(
  child: buildChildWidget(),
  onChildSizeChanged: (size) => handleNewChildSize(size),
);
```

### 简单的理论

当编写一个包含子窗口的自定义窗口小部件时，我们需要知道一些重要的事情:

- 对于每个自定义小部件，我们需要编写它的 Element 和(有时) RenderObject 实现
- 元素将扩展其子元素 Widgets 为单独的元素，并在需要时更新/重新创建它们
- 几乎没有什么重要的角色ーー子代管理、布局、绘制和命中测试(鼠标指针、触摸事件等)

### 代码

首先，我们需要声明一个实际的 Widget:

```dart
class ChildSize extends SingleChildRenderObjectWidget {
  final void Function(Size)? onChildSizeChanged;

  const ChildSize({
    Key? key,
    Widget? child,
    this.onChildSizeChanged,
  }) : super(key: key, child: child);
}
```

这里没有什么新东西，除了 SingleChildRenderObjectWidget。这是 Flutter 框架中的一个帮手，它允许我们编写不超过一个子窗口的自定义窗口小部件。这大大简化了我们的代码，因为我们根本不需要编写定制的 Element。

我们唯一需要添加到 Widget 中的是创建 RenderObject，并在 Widget 更改时更新它:

```dart
class ChildSize extends SingleChildRenderObjectWidget {
  // ...

  @override
  RenderObject createRenderObject(BuildContext context) {
    return RenderChildSize().._widget = this;
  }

  @override
  void updateRenderObject(BuildContext context, RenderChildSize renderObject) {
    renderObject.._widget = this;
  }
}
```

现在我们需要创建渲染对象:

```dart
class RenderChildSize extends RenderBox
    with RenderObjectWithChildMixin<RenderBox> {
      var _widget = const ChildSize();
}
```

是一个特殊的 mixin，我们在使用 SingleChildRenderObjectWidget 时必须添加它。这个 mixin 将处理所有无聊的东西(请参阅下一篇文章)。几乎这种类型的每个帮助器都需要添加一些 mixin。您可以在每个帮助器的文档中找到这些要求。

下一步要做的是布局我们的 child:

```dart
class RenderChildSize ... {
  // ...
  var _lastSize = Size.zero;

  @override
  void performLayout() {
    final child = this.child;
    if (child != null) {
      child.layout(constraints, parentUsesSize: true);
      size = child.size;
    } else {
      size = constraints.smallest;
    }    if (_lastSize != size) {
      _lastSize = size;
      _widget.onChildSizeChanged?.call(_lastSize);
    }
  }
}
```

在布局过程中我们必须决定渲染对象的大小。要做到这一点，我们需要布置我们的孩子。渲染对象的大小将与子对象的大小相同(我们没有任何填充、边距等)。

这里有一些重要的注意事项:

- 我们必须通过 parentUsesSize = true 子布局函数，以便事后得到它的大小。否则将抛出异常。感谢这个 flag Flutter 可以添加额外的优化
- 可能有这样一种情况，即使我们有一个子 Widget，也不会有它的子 RenderObject。并不是所有的小工具都有渲染对象。在这种情况下，如果存在任何最近的嵌套渲染对象，Flutter 将尝试提供给我们

在这个方法中，我们还检查大小是否发生了变化，并在发生变化时调用回调。

我们需要做的最后一件事就是描绘我们的 child 和路由命中测试事件:

```dart
class RenderChildSize ... {
  // ...

  @override
  void paint(PaintingContext context, Offset offset) {
    final child = this.child;
    if (child != null) {
      context.paintChild(child, offset);
    }
  }

  @override
  bool hitTestChildren(BoxHitTestResult result, {required Offset position}) {
    return child?.hitTest(result, position: position) == true;
  }
}
```

结果如下:

![](2021-06-21-06-10-50.png)

###

###

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
