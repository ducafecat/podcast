---
title: Flutter 零基础入门中文教学 - 13 基础组件 Button FlatButton RaisedButton OutlineButton
date: 2019-10-7 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

![](2019-10-07-17-06-41.png)

## 本节目标

常用按钮操作

- FlatButton（扁平化）
- RaisedButton（有按下状态）
- OutlineButton（有边框）
- MaterialButton（Material 风格）
- RawMaterialButton（没有应用 style 的 Material 风格按钮）
- FloatingActionButton（悬浮按钮）

## Button

在 Flutter 中 Button 有很多封装好的 Widget 类：

- FlatButton（扁平化）
- RaisedButton（有按下状态）
- OutlineButton（有边框）
- MaterialButton（Material 风格）
- RawMaterialButton（没有应用 style 的 Material 风格按钮）
- FloatingActionButton（悬浮按钮）
- BackButton（返回按钮）
- IconButton（Icon 图标）
- CloseButton（关闭按钮）
- ButtonBar（可以排列放置按钮元素的）

其中大部分的 Button 都是基于 RawMaterialButton 进行的修改定制而成的。

- 构造函数

```dart
const FlatButton({
    Key key,
    // 点击事件
    @required VoidCallback onPressed,
    // 高亮改变，按下和抬起时都会调用的方法
    ValueChanged<bool> onHighlightChanged,
    // 定义按钮的基色，以及按钮的最小尺寸，内部填充和形状的默认值
    ButtonTextTheme textTheme,
    // 按钮文字的颜色
    Color textColor,
    // 按钮禁用时的文字颜色
    Color disabledTextColor,
    // 按钮背景颜色
    Color color,
    // 按钮禁用时的背景颜色
    Color disabledColor,
    // 按钮按下时的背景颜色
    Color highlightColor,
    // 点击时，水波动画中水波的颜色，不要水波纹效果设置透明颜色即可
    Color splashColor,
    // 按钮主题，默认是浅色主题，分为深色和浅色
    Brightness colorBrightness,
    // 按钮的填充间距
    EdgeInsetsGeometry padding,
    // 外形
    ShapeBorder shape,
    Clip clipBehavior = Clip.none,
    MaterialTapTargetSize materialTapTargetSize,
    // 按钮的内容，里面可以放子元素
    @required Widget child,
  })
```

## 示例

- 后退、关闭

```dart
ButtonBar(
  children: <Widget>[
    BackButton(
      color: Colors.orange,
    ),
    CloseButton(),
  ],
),
```

- 扁平按钮 FlatButton

```dart
ButtonBar(
  children: <Widget>[
    FlatButton(
      child: Text('扁平按钮'),
      onPressed: () {
        print('我是扁平按钮');
      },
    ),
    FlatButton(
      child: Text(
        '扁平按钮 禁用',
      ),
      onPressed: null,
    ),
  ],
),
```

- 扁平带图标按钮 FlatButton.icon

```dart
ButtonBar(
  children: <Widget>[
    FlatButton.icon(
      label: Text('带图标扁平按钮'),
      icon: Icon(Icons.add_call, size: 18.0),
      onPressed: () {},
    ),
    FlatButton.icon(
      icon: const Icon(Icons.add_call, size: 18.0),
      label: const Text('带图标扁平按钮 禁用'),
      onPressed: null,
    ),
  ],
),
```

- 带框按钮 OutlineButton

```dart
ButtonBar(
  children: <Widget>[
    OutlineButton(
      onPressed: () {},
      child: Text('带框按钮'),
    ),
    OutlineButton(
      onPressed: null,
      child: Text('带框按钮 禁用'),
    ),
  ],
),
```

- 带框图标按钮 OutlineButton.icon

```dart
ButtonBar(
  children: <Widget>[
    OutlineButton.icon(
      label: Text('带框图标按钮'),
      icon: Icon(Icons.add_to_photos, size: 18.0),
      onPressed: () {},
    ),
    OutlineButton.icon(
      disabledTextColor: Colors.orange,
      icon: Icon(Icons.add_to_photos, size: 18.0),
      label: Text('带框图标按钮 禁用'),
      onPressed: null,
    ),
  ],
),
```

- 立体按钮 RaisedButton

```dart
ButtonBar(
  children: <Widget>[
    RaisedButton(
      child: Text('立体按钮'),
      onPressed: () {},
    ),
    RaisedButton(
      child: Text('立体按钮 禁用'),
      onPressed: null,
    ),
  ],
),
```

- 立体按钮带图标 RaisedButton.icon

```dart
ButtonBar(
  children: <Widget>[
    RaisedButton.icon(
      icon: Icon(Icons.add, size: 18.0),
      label: Text('立体按钮带图标'),
      onPressed: () {},
    ),
    RaisedButton.icon(
      icon: Icon(Icons.add, size: 18.0),
      label: Text('立体按钮带图标 禁用'),
      onPressed: null,
    ),
  ],
),
```

- Material 按钮 MaterialButton

```dart
ButtonBar(
  children: <Widget>[
    MaterialButton(
      child: Text('Material按钮'),
      onPressed: () {
        // Perform some action
      },
    ),
    MaterialButton(
      child: Text('Material按钮 禁用'),
      onPressed: null,
    ),
  ],
),
```

- RawMaterial 按钮 RawMaterialButton

```dart
ButtonBar(
  children: <Widget>[
    RawMaterialButton(
      child: Text('RawMaterial按钮'),
      onPressed: () {
        // Perform some action
      },
    ),
    RawMaterialButton(
      child: Text('RawMaterial按钮 禁用'),
      onPressed: null,
    ),
  ],
),
```

- 浮动按钮 FloatingActionButton

```dart
ButtonBar(
  children: <Widget>[
    FloatingActionButton(
      child: const Icon(Icons.add),
      heroTag: '浮动按钮',
      onPressed: () {
        // Perform some action
      },
      tooltip: '浮动按钮提示1',
    ),
    FloatingActionButton(
      child: const Icon(Icons.add),
      onPressed: null,
      heroTag: '浮动按钮 禁用',
      tooltip: '浮动按钮提示2',
    ),
  ],
),
```

## 代码

https://github.com/ducafecat/flutter-learn/blob/master/button_widget

## 参考

- [FlatButton class](https://api.flutter.dev/flutter/material/FlatButton-class.html)
- [RaisedButton class](https://api.flutter.dev/flutter/material/RaisedButton-class.html)
- [OutlineButton class](https://api.flutter.dev/flutter/material/OutlineButton-class.html)
- [MaterialButton class](https://api.flutter.dev/flutter/material/MaterialButton-class.html)
- [RawMaterialButton class](https://api.flutter.dev/flutter/material/RawMaterialButton-class.html)
- [FloatingActionButton class](https://api.flutter.dev/flutter/material/FloatingActionButton-class.html)
- [BackButton class](https://api.flutter.dev/flutter/material/BackButton-class.html)
- [IconButton class](https://api.flutter.dev/flutter/material/IconButton-class.html)
- [CloseButton class](https://api.flutter.dev/flutter/material/CloseButton-class.html)
- [ButtonBar class](https://api.flutter.dev/flutter/material/ButtonBar-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
