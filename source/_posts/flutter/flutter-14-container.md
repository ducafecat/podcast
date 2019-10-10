---
title: Flutter 零基础入门中文教学 - 14 基础组件 Container
date: 2019-10-8 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- 基础用法
- Padding 和 Margin
- BoxDecoration 装饰

## 基础用法

Container 是一个组合类容器，它本身不对应具体的 RenderObject，它是 DecoratedBox、ConstrainedBox、Transform、Padding、Align 等组件组合的一个多功能容器，所以我们只需通过一个 Container 组件可以实现同时需要装饰、变换、限制的场景。下面是 Container 的定义：

- 构造函数

```dart
Container({
    Key key,
    // 容器子Widget对齐方式
    this.alignment,
    // 容器内部padding
    this.padding,
    // 背景色
    Color color,
    // 背景装饰
    Decoration decoration,
    // 前景装饰
    this.foregroundDecoration,
    // 容器的宽度
    double width,
    // 容器的高度
    double height,
    // 容器大小的限制条件
    BoxConstraints constraints,
    // 容器外部margin
    this.margin,
    // 变换，如旋转
    this.transform,
    // 容器内子Widget
    this.child,
  })
```

## BoxDecoration 装饰

```dart
  const BoxDecoration({
    // 背景色
    this.color,
    // 背景图片
    this.image,
    // 边框样式
    this.border,
    // 边框圆角
    this.borderRadius,
    // 阴影
    this.boxShadow,
    // 渐变
    this.gradient,
    // 背景混合模式
    this.backgroundBlendMode,
    // 形状
    this.shape = BoxShape.rectangle,
  })
```

## 示例

```dart
  Container(
      constraints: BoxConstraints.expand(
        height: 200.0,
      ),
      margin: const EdgeInsets.all(20.0),
      padding: const EdgeInsets.all(8.0),

      // 背景色
      // color: Colors.teal.shade700,

      // 子Widget居中
      alignment: Alignment.centerLeft,

      // 子Widget元素
      child: Text('Hello World',
          style: Theme.of(context)
              .textTheme
              .display1
              .copyWith(color: Colors.white)),

      // 背景装饰
      decoration: BoxDecoration(
        // 背景色
        color: Colors.blueAccent,
        // 圆角
        // borderRadius: BorderRadius.all(
        //   Radius.circular(20.0),
        // ),
        // 渐变
        gradient: RadialGradient(
          colors: [Colors.red, Colors.orange],
          center: Alignment.topLeft,
          radius: .98,
        ),
        // 阴影
        boxShadow: [
          BoxShadow(
            blurRadius: 2,
            offset: Offset(0, 2),
            color: Colors.blue,
          ),
        ],
        // 背景图
        // image: DecorationImage(
        //   image: AssetImage('assets/flutter.png'),
        //   fit: BoxFit.cover,
        // ),
        // 背景混合模式
        backgroundBlendMode: BlendMode.color,
        // 形状
        shape: BoxShape.circle,
      ),

      // 前景装饰
      // foregroundDecoration: BoxDecoration(
      //   image: DecorationImage(
      //     image: AssetImage('assets/flutter.png'),
      //   ),
      // ),

      // Container旋转
      // transform: Matrix4.rotationZ(0.1),
    ),

```

## 代码

https://github.com/ducafecat/flutter-learn/blob/master/container_widget

## 参考

- [Container class](https://api.flutter.dev/flutter/widgets/Container-class.html)
- [BoxDecoration class](https://api.flutter.dev/flutter/painting/BoxDecoration-class.html)
- [RadialGradient class](https://api.flutter.dev/flutter/painting/RadialGradient-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
