---
title: Flutter ExpansionPanelList 创建一个扩展面板列表
tags: flutter
categories: 译文
date: 2022-04-19 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220419231754.png)

## 原文

> https://medium.flutterdevs.com/expansionpanellist-in-flutter-48aa06f764e

## 参考

- https://api.flutter.dev/flutter/material/ExpansionPanelList-class.html

## 正文

了解如何在您的 Flutter 应用程序创建一个扩展面板列表

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220419230230.png)

在本文中，我们将探讨 **ExpansionPanelList In Flutter.** 。我们将实施一个扩展面板列表演示程序，并学习如何自定义其风格与不同的属性在您的 Flutter 应用程序。

### Expansion Panel List:

它是一个类似于 listView 的实质性 Flutter 小部件。它可以只有扩展面板作为儿童。在某些情况下，我们可能需要显示一个列表，其中子元素可以展开/折叠以显示/隐藏一些详细的数据。为了显示这样的列表 flutter，提供了一个名为 ExapansionPanelList 的小部件。

在这个列表中，每个子元素都是 expsionpanel 小部件。在这个列表中，我们不能使用不同的窗口小部件作为子窗口。我们可以借助于 expsioncallback 属性来处理事物的状态调整(扩展或崩溃)。

> 演示模块:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220419230735.png)

这个演示视频显示了如何在一个 Flutter 扩展面板清单。它显示如何扩展面板列表将工作在您的 Flutter 应用程序。它显示了一个列表，在这个列表中孩子们可以展开/折叠以显示/隐藏一些详细信息。它会显示在你的设备上。

### Constructor:

> 要使用 **_ExpansionPanelList_**，需要调用下面的构造函数:

```dart
const ExpansionPanelList({
  Key? key,
  this.children = const <ExpansionPanel>[],
  this.expansionCallback,
  this.animationDuration = kThemeAnimationDuration,
  this.expandedHeaderPadding = _kPanelHeaderExpandedDefaultPadding,
  this.dividerColor,
  this.elevation = 2,
})
```

### Properties:

> **_ExpansionPanelList_** 的一些属性如下:

- **> children:** 此属性用于扩展面板 List 的子元素。它们的布局类似于[ListBody]。
- **> expansionCallback:** 此属性用于每当按下一个展开/折叠按钮时调用的回调。传递给回调的参数是按下的面板的索引，以及面板当前是否展开。
- **> animationDuration:** 这个属性用于展开或折叠时，我们可以观察到一些动画在一定时间内发生。我们可以通过使用扩展面板 List 的 animationDuration 属性来更改持续时间。我们只需要提供以微秒、毫秒或分钟为单位的持续时间值。
- **> expandedHeaderPadding:** 此属性用于展开时围绕面板标头的填充。默认情况下，16px 的空间是在扩展期间垂直添加到标题(上面和下面)。
- **> dividerColor:** 当[ expsionpanel.isexpanded ]为 false 时，此属性用于定义分隔符的颜色。如果‘ dividerColor’为空，则使用[ DividerThemeData.color ]。如果为 null，则使用[ ThemeData.dividerColor ]。
- **> elevation:** 此属性用于在扩展时定义[ expsionpanel ]的提升。这使用[ kElevationToShadow ]来模拟阴影，它不使用[ Material ]的任意高度特性。默认情况下，仰角的值为 2。

### 如何实现 dart 文件中的代码:

你需要分别在你的代码中实现它:

> 在 `lib` 文件夹中创建一个名为 `main.dart` 的新 dart 文件。

首先，我们将生成虚拟数据。我们将创建一个列表 <Map<String, dynamic>> 并添加变量 \_ items 等于生成一个列表。在这个列表中，我们将添加 number、 id、 title、 description 和 isExpanded。

```dart
List<Map<String, dynamic>> _items = List.generate(
    10,
        (index) => {
      'id': index,
      'title': 'Item $index',
      'description':
      'This is the description of the item $index. Lorem Ipsum is simply dummy text of the printing and typesetting industry.',
      'isExpanded': false
    });
```

在正文中，我们将添加 **ExpansionPanelList()** 小部件。在这个小部件中，我们将添加标高为 3，在括号中添加 expsioncallback 索引和 isExpanded。我们将添加 setState ()方法。在方法中，我们将添加 \_ items [ index ][‘ isexpanded’] equal not isExpanded。

```dart
ExpansionPanelList(
  elevation: 3,
  expansionCallback: (index, isExpanded) {
    setState(() {
      _items[index]['isExpanded'] = !isExpanded;
    });
  },
  animationDuration: Duration(milliseconds: 600),
  children: _items
      .map(
        (item) => ExpansionPanel(
      canTapOnHeader: true,
      backgroundColor:
      item['isExpanded'] == true ? Colors._cyan_[100] : Colors._white_,
      headerBuilder: (_, isExpanded) => Container(
          padding:
          EdgeInsets.symmetric(vertical: 15, horizontal: 30),
          child: Text(
            item['title'],
            style: TextStyle(fontSize: 20),
          )),
      body: Container(
        padding: EdgeInsets.symmetric(vertical: 15, horizontal: 30),
        child: Text(item['description']),
      ),
      isExpanded: item['isExpanded'],
    ),
  )
      .toList(),
),
```

我们将增加 animationDuration 为 600 毫秒。我们将添加子节点，因为 variable_items 映射到 expsionpanel ()小部件。在这个小部件中，我们将添加 canTapOnHeader was true，backgroundColor，headerBuilder 返回 Container ()小部件。在这个小部件中，我们将添加填充，并在其子属性上添加文本。在正文中，我们将添加 Conatiner 及其子属性，我们将添加文本。当我们运行应用程序时，我们应该获得屏幕输出，就像下面的屏幕截图一样。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220419230755.png)

### 全部代码

```dart
import 'package:flutter/material.dart';
import 'package:flutter_expansion_panel_list/splash_screen.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        debugShowCheckedModeBanner: false,
        theme: ThemeData(
          primarySwatch: Colors._teal_,
        ),
        home: Splash());
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  List<Map<String, dynamic>> _items = List.generate(
      10,
          (index) => {
        'id': index,
        'title': 'Item $index',
        'description':
        'This is the description of the item $index. Lorem Ipsum is simply dummy text of the printing and typesetting industry.',
        'isExpanded': false
      });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        automaticallyImplyLeading: false,
        title: Text('Flutter Expansion Panel List Demo'),
      ),
      body: SingleChildScrollView(
        child: ExpansionPanelList(
          elevation: 3,
          // Controlling the expansion behavior
          expansionCallback: (index, isExpanded) {
            setState(() {
              _items[index]['isExpanded'] = !isExpanded;
            });
          },
          animationDuration: Duration(milliseconds: 600),
          children: _items
              .map(
                (item) => ExpansionPanel(
              canTapOnHeader: true,
              backgroundColor:
              item['isExpanded'] == true ? Colors._cyan_[100] : Colors._white_,
              headerBuilder: (_, isExpanded) => Container(
                  padding:
                  EdgeInsets.symmetric(vertical: 15, horizontal: 30),
                  child: Text(
                    item['title'],
                    style: TextStyle(fontSize: 20),
                  )),
              body: Container(
                padding: EdgeInsets.symmetric(vertical: 15, horizontal: 30),
                child: Text(item['description']),
              ),
              isExpanded: item['isExpanded'],
            ),
          )
              .toList(),
        ),
      ),
    );
  }
}
```

## 结语

在本文中，我已经简单地解释了 **ExpansionPanelList** 的基本结构; 您可以根据自己的选择修改这段代码。这是一个小的介绍扩展/panellist On User Interaction 从我这边，它的工作使用 Flutter。

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
