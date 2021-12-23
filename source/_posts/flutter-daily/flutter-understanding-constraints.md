---
title: Flutter 布局 - 理解约束
tags: 布局
categories: flutter
date: 2021-12-23 00:00:00
---

![](2021-12-23-15-37-00.png)

## 前言

每个渲染引擎都有一套自己的规则，比如浏览器 html 流式布局，Flutter 里的是约束布局，这是一种科学的设计。

我想引擎设计出来肯定是有考虑，如:

- 容易学习，快速上手
- 零配置可用
- 高容错性（溢出、默认尺寸）
- 常见场景方案（横向、纵向、滚动、叠加、嵌套、响应式、自适应、浮动、大列表...）

## 参考

经典的约束规则文章 Marcelo Glasberg 撰写，后来被 flutter.dev 收录，掘金也有人翻译。

- https://medium.com/flutter-community/flutter-the-advanced-layout-rule-even-beginners-must-know-edc9516d1a2
- https://juejin.cn/post/6846687593745088526
- https://docs.flutter.dev/development/ui/layout/constraints

## 本节目标

- 理解布局约束原则
- 掌握调试布局工具
- 名词，紧约束、松约束、unbounded

## 视频

## 正文

![](2021-12-23-16-01-55.png)

布局的话题展开说，就是各种布局方案+具体组件的使用。

但是约束布局是核心，这也是本文的侧重。

### 核心规则：Constraints go down. Sizes go up. Positions are set by parents.

- 上层 widget 向下层 widget 传递约束条件。
- 下层 widget 向上层 widget 传递大小信息。
- 上层 widget 决定下层 widget 的位置。

![约束布局](https://ducafecat.tech/2021/11/24/flutter/flutter-19-layout-rule/2021-11-24-23-10-10.png)

```dart
...

Widget _buildScaffold() {
  return Scaffold(
    body: Column(
      children: const <Widget>[
        Text("aaaaaa"),
        Text("bbbbb"),
        Text("cccc"),
      ],
    ),
  );
}
```

![](2021-12-23-13-31-06.png)

### 紧约束、松约束

- 紧约束 tight

  它的最大/最小宽度是一致的，高度也一样。

- 松约束 loose

  最小宽度/高度为 0

- 同时是紧约束、松约束

  如果最大最小都是 0

```dart
Widget _buildScaffold() {
  return Scaffold(
    body: ConstrainedBox(
      constraints: const BoxConstraints(
        minWidth: 100,
        minHeight: 100,
        maxWidth: 150,
        maxHeight: 150,
      ),
      child: Container(
        width: 10,
        height: 10,
        color: Colors.blue,
      ),
    ),
  );
}
```

![](2021-12-23-15-08-01.png)

### 有边界 bounded、无边界 unbounded

`Row` `Column` `ListView` 这种组件 属于 `unbounded`

```dart
Widget _buildScaffold() {
  return Scaffold(
    body: Column(
      children: [
        const FlutterLogo(size: 50),
        const FlutterLogo(size: 20),
        Container(
          height: 2000,
          color: Colors.amber,
        ),
      ],
    ),
  );
}
```

![](2021-12-23-15-21-24.png)

### UnconstrainedBox 不受约束

`UnconstrainedBox` 可以不受约束自己控制大小

```dart
Widget _buildScaffold() {
  return Scaffold(
    body: ConstrainedBox(
      constraints: const BoxConstraints(
        minWidth: 100,
        minHeight: 100,
        maxWidth: 150,
        maxHeight: 150,
      ),
      child: UnconstrainedBox(
        child: Container(
          width: 10,
          height: 10,
          color: Colors.blue,
        ),
      ),
    ),
  );
}
```

![](2021-12-23-15-27-06.png)

## 总结

今天我们就是把布局很核心的约束、限制规则讲了，大家在布局的时候还是要多思考。

如果你的布局代码写的很复杂，就要去思考重构了。

当然了业务需要是另外一回事，尽量的让布局引擎默认规则来处理，这样兼容性好。

---

© 猫哥

- [ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [b 站](https://space.bilibili.com/404904528)

https://ducafecat.tech/img/banner-gzh.png
