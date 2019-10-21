---
title: Flutter 零基础入门中文教学 - 17 基础组件 Fex Expanded
date: 2019-10-11 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- expanded
- flex

Flex 组件是 Row 和 Column 的父类，主要用于弹性布局，类似于 HTML 中的 Flex 弹性盒子布局，可以按照一定比例进行分类布局空间。

Flex 继承自 MultiChildRenderObjectWidget，Flex 也是多子元素的一个组件之一（内部可以包含多个子元素）。

Flex 一般和 Expanded 搭配使用，Expanded 组件从名字就可以看出它的特点，就是让子元素扩展占用 Flex 的剩余空间。

## Expanded Flex

- 构造函数

单独看 Flex 组件没有意义，因为一般直接用它的子类 Row 和 Column 来使用。而 Flex 主要是和 Expanded 搭配使用。我们再看下 Expanded 组件构造方法：

```dart

Flex({
    Key key,
    // 子元素排列方向：横向还是纵向
    @required this.direction,
    this.mainAxisAlignment = MainAxisAlignment.start,
    this.mainAxisSize = MainAxisSize.max,
    this.crossAxisAlignment = CrossAxisAlignment.center,
    this.textDirection,
    this.verticalDirection = VerticalDirection.down,
    this.textBaseline,
    List<Widget> children = const <Widget>[],
  })

const Expanded({
    Key key,
    // 占用空间比重、权重
    int flex = 1,
    // 子元素
    @required Widget child,
  })

```

## 例子 Expanded

```dart
Row(
  children: <Widget>[
    Container(
      width: 50,
      color: Colors.cyan,
    ),
    Expanded(
      child: Container(
        color: Colors.brown,
      ),
    ),
  ],
)
```

![](2019-10-21-10-59-29.png)

## 例子 Flex

```dart
Column(
  children: <Widget>[
    Expanded(
      flex: 1,
      child: Container(
        color: Colors.cyan,
      ),
    ),
    Expanded(
      flex: 4,
      child: Container(
        color: Colors.brown,
      ),
    )
  ],
)
```

![](2019-10-21-11-00-23.png)

## 代码

https://github.com/ducafecat/flutter-learn/tree/master/flex_expanded_widget

## 参考

- [widgets](https://flutter.dev/docs/development/ui/widgets)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
