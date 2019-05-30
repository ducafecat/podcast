---
title: Flutter移动开发 - 05 基础控件 button
date: 2019-05-23 00:39:12
tags: flutter
categories: Flutter移动开发
---

## 本节目标

## 1. button

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

## 2. 构造函数

| 名称                                        | 说明                                     |
| ------------------------------------------- | ---------------------------------------- |
| @required VoidCallback onPressed            | 点击事件                                 |
| ValueChanged<bool> onHighlightChanged       | 高亮改变，按下和抬起时都会调用的方法     |
| ButtonTextTheme textTheme                   | 定义按钮样式                             |
| Color textColor                             | 文字的颜色                               |
| Color disabledTextColor                     | 禁用时的文字颜色                         |
| Color color                                 | 背景颜色                                 |
| Color disabledColor                         | 禁用时的背景颜色                         |
| Color highlightColor                        | 按下时的背景颜色                         |
| Color splashColor                           | 水波动画中水波的颜色                     |
| Brightness colorBrightness                  | 按钮主题，默认是浅色主题，分为深色和浅色 |
| EdgeInsetsGeometry padding                  | 填充间距                                 |
| ShapeBorder shape                           | 外形                                     |
| Clip clipBehavior = Clip.none               |
| MaterialTapTargetSize materialTapTargetSize |
| @required Widget child                      | 子元素                                   |

## 3. 代码示例

![](image-20190530170338535.png)

### 3.1 后退、关闭

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

### 3.2 扁平按钮 FlatButton

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

### 3.3 扁平带图标按钮 FlatButton.icon

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

### 3.4 带框按钮 OutlineButton

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

### 3.5 带框图标按钮 OutlineButton.icon

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

### 3.6 立体按钮 RaisedButton

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

### 3.7 立体按钮带图标 RaisedButton.icon

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

### 3.8 Material按钮 MaterialButton

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

### 3.2 RawMaterial按钮 RawMaterialButton

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

### 3.9 浮动按钮 FloatingActionButton

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

https://github.com/ducafecat/flutter-learn/tree/master/p05_button

## 参考

- [FlatButton class](https://api.flutter.dev/flutter/material/FlatButton-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)