---
title: Flutter 零基础入门中文教学 - 18 基础组件 Stack IndexedStack Positioned
date: 2019-10-12 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- Stack
- IndexedStack
- Positioned

Stack 和 IndexStack 都是层叠布局方式，类似于 Android 里的 FrameLayout 帧布局，内部子元素是有层级堆起来的。

Stack 继承自 MultiChildRenderObjectWidget，Stack 也是多子元素的一个组件之一（内部可以包含多个子元素）。

而 IndexedStack 继承自 Stack，扩展了 Stack 的一些特性。它的作用是显示第 index 个子元素，其他子元素都是不可见的。所以 IndexedStack 的尺寸永远是跟最大的子元素尺寸一致。

Stack 的布局行为，是根据子元素是 positioned 还是 non-positioned 来区分的：

对于 positioned 的子元素，它们的位置会根据所设置的 top、bottom、right 或 left 属性来确定，这几个值都是相对于 Stack 的左上角；
对于 non-positioned 的子元素，它们会根据 Stack 的 aligment 来设置位置。
Stack 布局的子元素层级堆叠顺序：最先布局绘制的子元素在最底层，越后绘制的越在顶层。类似于 Web 中的 z-index。

## Stack

```dart
Stack({
    Key key,
    // 对齐方式，默认是左上角（topStart）
    this.alignment = AlignmentDirectional.topStart,
    // 对齐方向
    this.textDirection,
    // 定义如何设置无定位子元素尺寸，默认为loose
    this.fit = StackFit.loose,
    // 超过的部分子元素处理方式
    this.overflow = Overflow.clip,
    // 子元素
    List<Widget> children = const <Widget>[],
  })
```

- alignment

```dart
  // 左上角
  static const Alignment topLeft = Alignment(-1.0, -1.0);

  // 主轴顶部对齐，交叉轴居中
  static const Alignment topCenter = Alignment(0.0, -1.0);

  // 主轴顶部对齐，交叉轴偏右
  static const Alignment topRight = Alignment(1.0, -1.0);

  // 主轴居中，交叉轴偏左
  static const Alignment centerLeft = Alignment(-1.0, 0.0);

  // 居中
  static const Alignment center = Alignment(0.0, 0.0);

  // 主轴居中，交叉轴偏右
  static const Alignment centerRight = Alignment(1.0, 0.0);

  // 主轴底部对齐，交叉轴偏左
  static const Alignment bottomLeft = Alignment(-1.0, 1.0);

  // 主轴底部对齐，交叉轴居中
  static const Alignment bottomCenter = Alignment(0.0, 1.0);

  // 主轴底部对齐，交叉轴偏右
  static const Alignment bottomRight = Alignment(1.0, 1.0);
```

- fit

```dart
enum StackFit {
  // 子元素宽松的取值，可以从min到max的尺寸
  loose,

  // 子元素尽可能的占用剩余空间，取max尺寸
  expand,

  // 不改变子元素的约束条件
  passthrough,
}
```

- overflow

```dart
enum Overflow {
  // 超出部分不会被裁剪，正常显示
  visible,

  // 超出部分会被裁剪
  clip,
}
```

## IndexedStack

```dart
IndexedStack({
    Key key,
    AlignmentGeometry alignment = AlignmentDirectional.topStart,
    TextDirection textDirection,
    StackFit sizing = StackFit.loose,
    // 多了一个索引，索引的这个元素显示，其他元素隐藏
    this.index = 0,
    // 子元素
    List<Widget> children = const <Widget>[],
  })
```

## Positioned

```dart
  const Positioned({
    Key key,
    this.left, // 上下左右位置
    this.top,
    this.right,
    this.bottom,
    this.width, // 宽高
    this.height,
    @required Widget child,
  })
```

## 代码

https://github.com/ducafecat/flutter-learn/tree/master/state_less_ful_app

## 参考

- [widgets](https://flutter.dev/docs/development/ui/widgets)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
