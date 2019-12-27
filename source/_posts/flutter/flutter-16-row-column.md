---
title: Flutter 零基础入门中文教学 - 16 基础组件 Row Column
date: 2019-10-10 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- Row 行组件
- Column 列组件
- mainAxisAlignment 主轴
- crossAxisAlignment 交叉轴
- textDirection 排列方向
- verticalDirection 交叉轴起始位置

## Row

Row 布局组件类似于 Android 中的 LinearLayout 线性布局，它用来做水平横向布局使用，里面的 children 子元素按照水平方向进行排列。

- 构造

```dart
Row({
    Key key,
    // 主轴方向上的对齐方式（Row的主轴是横向轴）
    MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,
    // 在主轴方向（Row的主轴是横向轴）占有空间的值，默认是max
    MainAxisSize mainAxisSize = MainAxisSize.max,
    // 在交叉轴方向(Row是纵向轴)的对齐方式，Row的高度等于子元素中最高的子元素高度
    CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,
    // 水平方向子元素的排列方向：从左到右排列还是反向
    TextDirection textDirection,
    // 表示纵轴（垂直）的对齐排列方向，默认是VerticalDirection.down，表示从上到下。这个参数一般用于Column组件里
    VerticalDirection verticalDirection = VerticalDirection.down,
    // 字符对齐基线方式
    TextBaseline textBaseline,
    // 子元素集合
    List<Widget> children = const <Widget>[],
  })
```

- MainAxisAlignment

主轴属性：主轴方向上的对齐方式，Row 是横向轴为主轴

```dart
enum MainAxisAlignment {
  // 按照主轴起点对齐，例如：按照靠近最左侧子元素对齐
  start,

  // 将子元素放置在主轴的末尾，按照末尾对齐
  end,

  // 子元素放置在主轴中心对齐
  center,

  // 将主轴方向上的空白区域均分，使得子元素之间的空白区域相等，首尾子元素都靠近首尾，没有间隙。有点类似于两端对齐
  spaceBetween,

  // 将主轴方向上的空白区域均分，使得子元素之间的空白区域相等，但是首尾子元素的空白区域为1/2
  spaceAround,

  // 将主轴方向上的空白区域均分，使得子元素之间的空白区域相等，包括首尾子元素
  spaceEvenly,
}

```

- CrossAxisAlignment

交叉属性：在交叉轴方向的对齐方式，Row 是纵向轴。Row 的高度等于子元素中最高的子元素高度

```dart
enum CrossAxisAlignment {
  // 子元素在交叉轴上起点处展示
  start,

  // 子元素在交叉轴上末尾处展示
  end,

  // 子元素在交叉轴上居中展示
  center,

  // 让子元素填满交叉轴方向
  stretch,

  // 在交叉轴方向，使得子元素按照baseline对齐
  baseline,
}
```

- MainAxisSize

在主轴方向子元素占有空间的方式，Row 的主轴是横向轴。默认是 max

```dart
enum MainAxisSize {
  // 根据传入的布局约束条件，最大化主轴方向占用可用空间，也就是尽可能充满可用宽度
  max,

  // 与max相反，是最小化占用主轴方向的可用空间
  min,
}
```

## Column

Column 是纵向排列子元素

参数用法同上

## 例子

```dart
    // Row 行组件
    Widget _buildRow() {
      return Row(
        mainAxisAlignment: MainAxisAlignment.center,
        verticalDirection: VerticalDirection.up,
        textBaseline: TextBaseline.ideographic,
        children: <Widget>[
          RaisedButton(
            color: Colors.blue,
            child: Text('按钮1'),
            onPressed: () {},
          ),
          RaisedButton(
            color: Colors.grey,
            child: Text('按钮2'),
            onPressed: () {},
          ),
          RaisedButton(
            color: Colors.orange,
            child: Text('按钮3'),
            onPressed: () {},
          ),
        ],
      );
    }

    // Column 列组件
    Widget _buildColumn() {
      return Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          RaisedButton(
            color: Colors.blue,
            child: Text('按钮1'),
            onPressed: () {},
          ),
          RaisedButton(
            color: Colors.grey,
            child: Text('按钮2'),
            onPressed: () {},
          ),
          RaisedButton(
            color: Colors.orange,
            child: Text('按钮3'),
            onPressed: () {},
          ),
        ],
      );
    }

    // Row Column 组件嵌套
    Widget _buildRowColumn() {
      return Column(
        mainAxisAlignment: MainAxisAlignment.center,
        // crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Row(
            // 元素排列顺序
            textDirection: TextDirection.rtl,
            // 主轴方向
            mainAxisAlignment: MainAxisAlignment.center,
            // 交叉轴的起始位置
            verticalDirection: VerticalDirection.up,
            // 交叉轴对齐方式
            crossAxisAlignment: CrossAxisAlignment.end,
            children: <Widget>[
              RaisedButton(
                color: Colors.blue,
                child: Text('按钮1'),
                onPressed: () {},
              ),
              RaisedButton(
                color: Colors.blue,
                child: Text('按钮2222222'),
                onPressed: () {},
              ),
              Container(
                width: 100,
                height: 100,
                color: Colors.yellow,
              )
            ],
          )
        ],
      );
    }

    return MaterialApp(
      title: 'Material App',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Material App Bar'),
        ),
        body:
            //_buildRow(),
            // _buildColumn(),
            _buildRowColumn(),
      ),
    );
```

## 代码

https://github.com/ducafecat/flutter-learn/blob/master/row_column_widget

## 参考

- [Row class](https://api.flutter.dev/flutter/widgets/Row-class.html)
- [Column class](https://api.flutter.dev/flutter/widgets/Column-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
