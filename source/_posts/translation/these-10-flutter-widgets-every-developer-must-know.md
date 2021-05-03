---
title: 这10个每个开发者都必须知道的Widgets
tags: flutter
categories: 译文
date: 2021-05-03 00:00:00
---

![](2021-05-03-17-09-37.png)

## 原文

> https://genotechies.medium.com/these-10-flutter-widgets-every-developer-must-know-d0b61529796b

## 这些是我们将要讨论的 widgets:

- Dismissible
- SizedBox
- Draggable
- Flexible
- MediaQuery
- Spacer
- AnimatedIcon
- Placeholder
- RichText
- ReorderableListView

## 正文

### Dismissible

滑动和隐藏是移动应用程序中常见的 UI 模式。要在 Flutter 做到这一点，可以使用 Dismissible widget。它有一个 child，background 和 key 。它将检测滑动手势和动画的 child 小部件。你也可以双向和垂直的交换。你可以用自己的方式使用更多的属性。您可以通过复制并粘贴下面的代码来尝试。

```dart
class _MyHomePageState extends State<MyHomePage> {
  List<String> _values = ['Item 1', 'Item 2', 'Item 3', 'Item 4', 'Item 5'];

  @override
  Widget build(BuildContext context) {
    return ListView.separated(
        itemCount: _values.length,
        padding: const EdgeInsets.all(5.0),
        separatorBuilder: (context, index) => Divider(
          color: Colors.black,
        ),
        itemBuilder: (context, index) {
          return Dismissible(
            key: Key('item ${_values[index]}'),
            onDismissed: (DismissDirection direction) {
              if (direction == DismissDirection.startToEnd) {
                print("Selected Item");
              } else {
                print('Delete item');
              }

              setState(() {
                _values.removeAt(index);
              });
            },
            child: ListTile(
              leading: Icon(Icons.email, size: 50),
              title: Text(_values[index]),
            ),
          );
        }
    );

  }
}
```

### SizedBox

这是一个小部件示例。当你有一个小部件，应该是固定的大小。例如，一个按钮的大小应该为 width = 100px 和 height = 50px。您需要将按钮包装在 SizedBox 中。下面是类的构造函数。

```dart
const SizedBox(
{Key key,
double width,
double height,
Widget child}
)
```

### Draggable

在许多应用程序中，我们可以看到拖动选项，如在电子邮件，文档拖动。有了这个 Flutter 小部件，很容易实现这个功能。在这里，我们拖动数据。这里我传递一个从 Draggable 到 DragTarget 的字符串。然后你需要说明你传递的数据是什么，子属性显示你的数据。DragTarget 目标是拖曳 Draggable 的着陆区。主要有三种调用方法。

- onwillAccept: 以测试移动目标是否可以接受数据
- onAccept: 调用有效的可拖动区域
- onLeave: 当区域不成功时调用

### Flexible

大多数时候，我们使用行和列来显示一组子窗口小部件。但他们需要灵活的大小来显示与父母的相关性。您只需要将所有子窗口小部件包装在一个灵活的窗口小部件中。Flex 值决定每个子元素获得多少空间。当改变屏幕大小时，它不会改变儿童之间的比例。

```dart
child:  Column(
  children: <Widget>[
    Flexible(
        flex: 3,
        child: Container(
          color: Colors.red,
        )
    ),
    Flexible(
        flex: 1,
        child: Container(
          color: Colors.green,
        )
    ),
    Flexible(
        flex: 2,
        child: Container(
          color: Colors.blue,
        )
    ),
  ],
)
```

![](2021-05-03-16-58-40.png)

### MediaQuery

如果你的目标是在手机和选项卡上运行你的应用程序，你的应用程序需要支持不同的用户界面大小。此外，有时用户有自己的 UI 期望，如字体大小或小，方向，填充等。使用这个 MediaQuery，您可以获得屏幕大小信息和用户首选项，并根据这些细节构建布局。

```dart
const MediaQueryData({
  this.size = Size.zero,
  this.devicePixelRatio = 1.0,
  this.textScaleFactor = 1.0,
  this.platformBrightness = Brightness.light,
  this.padding = EdgeInsets.zero,
  this.viewInsets = EdgeInsets.zero,
  this.systemGestureInsets = EdgeInsets.zero,
  this.viewPadding = EdgeInsets.zero,
  this.alwaysUse24HourFormat = false,
  this.accessibleNavigation = false,
  this.invertColors = false,
  this.highContrast = false,
  this.disableAnimations = false,
  this.boldText = false,
  this.navigationMode = NavigationMode.traditional,
})
```

这是一个提取屏幕尺寸的示例。

```dart
MediaQueryData deviceInfo = MediaQuery.of(context);
```

输出

```
I/flutter ( 6508): size: Size(360.0, 592.0)
I/flutter ( 6508): padding: EdgeInsets(0.0, 24.0, 0.0, 0.0)
I/flutter (6508) : Size: Size (360.0,592.0) i/flutter (6508) : padding: EdgeInsets (0.0,24.0,0.0,0.0)
```

### Spacer

这是另一个小部件，您最好在事先自定义中使用它。在一行中，我们可以使用 MainAxisAlignment 定义子级之间的空间。但是使用 Spacer 小部件，你可以做得更多。只需在其他小部件之间添加间隔符即可。然后 children 扩展开来制造额外的空间。有一个 flex 属性来确定相对大小。

```dart
SizedBox(
  height: 50,
  child: Row(
    children: <Widget>[
      Container(
        width: 50,
        color: Colors.red,
      ),
      Spacer(flex: 2,),
      Container(
        width: 50,
        color: Colors.green,
      ),
      Spacer(flex: 1,),
      Container(
        width: 50,
        color: Colors.blue,
      ),
      Container(
        width: 50,
        color: Colors.yellow,
      ),
    ],
  ),
);
```

![](2021-05-03-17-02-13.png)

### AnimatedIcon

已经有一个巨大的图标集已经在框架。也有动画图标，你可以在你的应用程序中使用。要使用这些，我们需要一个 AnimatedIcon 小部件。你需要提供图标和主要的进度属性。Flutter 提供了许多不同的动画图标供您使用。

```dart
import 'package:flutter/animation.dart';

import 'package:flutter/material.dart';

void main() => runApp(LogoApp());

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  bool isPlaying = false;

  Animation animation;

  AnimationController controller;

  @override
  void initState() {
    super.initState();

    controller = AnimationController(
        duration: const Duration(milliseconds: 500), vsync: this);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
            child: IconButton(
              iconSize: 70,
              icon: AnimatedIcon(
                icon: AnimatedIcons.play_pause,
                progress: controller,
              ),
              onPressed: () => _onpressed(),
            )),
      ),
    );
  }

  @override
  void dispose() {
    controller.dispose();

    super.dispose();
  }

  _onpressed() {
    setState(() {
      isPlaying = !isPlaying;

      isPlaying ? controller.forward() : controller.reverse();
    });
  }
}
```

![](2021-05-03-17-03-11.png)

### Placeholder

有时您需要为 UI 的特定组件保留空间，直到最后确定该组件的视图。因此，与其保留一个空间，我们可以在那里放置 Plaholder 以便进一步实现。在你可以开始实施它之后。这将填补所有提到的空间。

```dart
Center(
  child: Column(
    children: <Widget>[
      Container(
          child: Placeholder()
      ),
      Expanded(
        child: Row(
          children: <Widget>[
            Flexible(
              flex: 1,
              child: Placeholder(color: Colors.red,),
            ),
            Flexible(
              flex: 4,
              child: Placeholder(color: Colors.green,),
            ),
          ],
        ),
      )
    ],
  )
),
```

![](2021-05-03-17-03-39.png)

### RichText

文本是每个应用程序的主要 UI 组件之一。因此字体设计非常重要。你必须注意文字的样式和外观，如文字大小、字体、样式等。有时候你需要显示一个结合了不同风格的段落。用粗体表示强调，或用斜体表示，或用下划线表示，或用不同的颜色，不同的字体大小，或同时显示所有内容。你最好使用 RichText。下面是一个例子:

```dart
RichText(
  text: TextSpan(
    style: TextStyle(color: Colors.black, fontSize: 24),
    children: <TextSpan>[
      TextSpan(text: 'Flutter ', style: TextStyle(color: Colors.red)),
      TextSpan(text: 'Placeholder '),
      TextSpan(text: 'Widget', style: TextStyle(decoration: TextDecoration.underline, fontStyle: FontStyle.italic))
    ],
  ),
)
```

![](2021-05-03-17-04-16.png)

### ReorderableListView

在我们的应用程序中，我们使用列表视图来显示一组数据并滚动它们。通常，您不能移动和更改列表中的位置。ReorderbaleListView 是解决方案。有了它，用户可以长时间按下该项目，并将其放入一个新的他或她喜欢的地方。列表视图的每个项都有一个用于标识该项的键，在移动该项时，调用 onReorder 方法并跟踪移动和更改。下面是一个例子。

```dart
class _TopListState extends State<TopList> {
  List<String> topMovies = [
    "It Happened One Night(1934)",
    "Black Panther(2018)",
    "Citizen Kane(1941)",
    "Parasite (Gisaengchung)(2019)",
    "Avengers: Endgame(2019)",
    "The Wizard of Oz(1939)",
    "Casablanca(1942)",
    "Knives Out(2019)"
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("ReorderableListView Example"),
      ),
      body: ReorderableListView(
        onReorder: (int oldIndex, int newIndex) {},
        children: getListItems(),
      ),
    );
  }

  List<ListTile> getListItems() =>
      topMovies
          .asMap()
          .map((i, item) => MapEntry(i, buildTenableListTile(item, i)))
          .values
          .toList();

  ListTile buildTenableListTile(String item, int index) {
    return ListTile(
      key: ValueKey(item),
      title: Text(item),
      leading: Text("${index + 1}"),
    );
  }
}
```

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
