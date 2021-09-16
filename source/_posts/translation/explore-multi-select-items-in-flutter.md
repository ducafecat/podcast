---
title: flutter 多选项目插件
tags: flutter
categories: 译文
date: 2021-09-16 00:00:00
---

![](2021-09-16-09-45-19.png)

## 原文

> https://medium.com/flutterdevs/explore-multi-select-items-in-flutter-a90665e17be

## 参考

- https://pub.dev/packages/multi_select_item

## 正文

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/f02f6f1939a99e095851e09ab65e0317ad6a3851f3fc0f1415ed1b8ac79c5220.png)

Flutter 小部件是使用现代框架构建的。这就像是一种反应。在这里，我们从小部件开始创建任何应用程序。屏幕中的每个组件都是一个小部件。这个小部件描述了根据他目前的配置和状态，他的前景应该是什么样的。小部件展示类似于它的想法和当前的设置和状态。

Flutter 自动化测试使您能够满足您的应用程序的高响应性，因为它有助于在您的应用程序中发现 bug 和各种问题。 Flutter 是一个工具，开发移动，桌面，网络应用程序与代码 \& 是一个免费和开放源码的工具。 Flutter 有能力容易测试任何应用程序。这种能力就是他们在目标平台上按照我们希望的方式工作。 Dart 测试适用于单元测试和非 ui 测试; 它在开发机器上运行，不依赖于 Flutter 应用程序的 GUI。

在这个博客，我们将探索 Flutter 多选择项目。我们还将实现一个演示的多项目在 Flutter 选择。如何使用它们在您的 Flutter 应用程序。那么让我们开始吧。

- 多选择项 | Flutter 包装

https://pub.dev/packages/multi_select_item

### 多选项目:

多选项这是一个处理多选项的 Flutter 库使用这个库我们创建一个列表，当我们可以删除这个列表中的项目时使用这个小部件我们将它包装在列表视图构建器项目中，这样我们就可以很容易地创建一个多选项列表。

### 实施方案:

你需要分别在你的代码中实现它:

**第一步: 添加依赖项。**

> 将依赖项添加到 pubspec ー yaml 文件。

首先，在 pubspec. yaml 中添加 flutter 本地化和 intl 库。

```dart
dependencies:
  multi_select_item: ^1.0.3
```

**步骤 2: 导入包:**

```dart
import 'package:multi_select_item/multi_select_item.dart';
```

**第三步: 启用 AndriodX**

```dart
org.gradle.jvmargs=-Xmx1536M
android.enableR8=true
android.useAndroidX=true
android.enableJetifier=true
```

### 代码步骤:

要实现这一点，您需要先编写“首先定义控制器”。

```dart
MultiSelectController controller = new MultiSelectController();
```

在此之后，我们创建了一个列表，该列表在 initState \(\)内定义了它的值，并在此之后按列表长度确定控制器的设置长度。参考下面的代码包括在内。

```dart
@override
void initState() {
  super.initState();

  multiSelectList.add({"images": 'assets/images/resort_1.jpg', "desc":"Welcome to New York City!"});
  multiSelectList.add({"images":'assets/images/resort_2.jpg' ,"desc":"Welcome to Los Angeles!"});
  multiSelectList.add({"images":'assets/images/resort_3.jpg' ,"desc":"Welcome to Chicago!"});
  multiSelectList.add({"images":'assets/images/resort_4.jpg', "desc":"Welcome to Houston!"});

  controller.disableEditingWhenNoneSelected = true;
  controller.set(multiSelectList.length);
}
```

在这之后，我们使用一个 ListViewBuilder 小部件，其中我们使用了 MultiSelectItem \(\)小部件，其中我们使用了卡片及其子属性，我们使用了行小部件，其中我们初始化上面定义的列表值，该列表值具有图像和文本，并在其 onSelected \(\)上列出控制器是在 setState 内部定义的，用于选择值。

```dart
ListView.builder(
  itemCount: multiSelectList.length,
  itemBuilder: (context, index) {
    return MultiSelectItem(
      isSelecting: controller.isSelecting,
      onSelected: () {
        setState(() {
          controller.toggle(index);
        });
      },
      child:Container(
        height:75,
        margin: EdgeInsets.only(left:15,right:15,top:15),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(12),
          color: Colors._transparent_,
        ),
        child:Card(
          color:controller.isSelected(index)
              ? Colors._grey_.shade200:Colors._white_,
          shape: RoundedRectangleBorder(

            borderRadius: BorderRadius.all(Radius.circular(8.0)),

          ),
          child:Padding(
            padding:EdgeInsets.symmetric(vertical:10, horizontal: 12),
            child: Row(
              children: [
                _//contentPadding: EdgeInsets.symmetric(vertical: 12, horizontal: 16),_ ClipRRect(
                  borderRadius: BorderRadius.circular(12),
                  child:Image.asset(multiSelectList[index]['images'],fit:BoxFit.cover,width:60,height:60,),
                ),
                SizedBox(width:20,),
                Text(multiSelectList[index]['desc'], style: TextStyle(fontSize:14)),
              ],
            ),
          ),
        ),
      ),
    );
  },
),
```

之后，我们将删除选定的项目，以删除这我们采取 forEach 和排序的价值，这将排序我们的选择项目首先从最大的 id 最小的 id，然后它将逐一删除它。

```dart
void delete() {
  var list = controller.selectedIndexes;
  list.sort((b, a) =>
      a.compareTo(b));
  list.forEach((element) {
    multiSelectList.removeAt(element);
  });

  setState(() {
    controller.set(multiSelectList.length);
  });
}
```

### 全部代码 :

```dart
import 'package:multi_select_item/multi_select_item.dart';
import 'package:flutter/material.dart';

class MultiSelectListDemo extends StatefulWidget {
  @override
  _MultiSelectListDemoState createState() => new _MultiSelectListDemoState();
}

class _MultiSelectListDemoState extends State<MultiSelectListDemo> {


  List multiSelectList = [];


  MultiSelectController controller = new MultiSelectController();

  @override
  void initState() {
    super.initState();

    multiSelectList.add({"images": 'assets/images/resort_1.jpg', "desc":"Welcome to New York City!"});
    multiSelectList.add({"images":'assets/images/resort_2.jpg' ,"desc":"Welcome to Los Angeles!"});
    multiSelectList.add({"images":'assets/images/resort_3.jpg' ,"desc":"Welcome to Chicago!"});
    multiSelectList.add({"images":'assets/images/resort_4.jpg', "desc":"Welcome to Houston!"});
    multiSelectList.add({"images":'assets/images/sanfrancisco.jpg', "desc":"Welcome to San francisco!"});

    controller.disableEditingWhenNoneSelected = true;
    controller.set(multiSelectList.length);
  }

  void add() {
    multiSelectList.add({"images": multiSelectList.length});
    multiSelectList.add({"desc": multiSelectList.length});

    setState(() {
      controller.set(multiSelectList.length);
    });
  }

  void delete() {
    var list = controller.selectedIndexes;
    list.sort((b, a) =>
        a.compareTo(b));
    list.forEach((element) {
      multiSelectList.removeAt(element);
    });

    setState(() {
      controller.set(multiSelectList.length);
    });
  }

  void selectAll() {
    setState(() {
      controller.toggleAll();
    });
  }

  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: () async {
        var before = !controller.isSelecting;
        setState(() {
          controller.deselectAll();
        });
        return before;
      },
      child: new Scaffold(
        appBar: new AppBar(
          title: new Text('Selected ${controller.selectedIndexes.length}' ),
          actions: (controller.isSelecting)
              ? <Widget>[
            IconButton(
              icon: Icon(Icons._select_all_),
              onPressed: selectAll,
            ),
            IconButton(
              icon: Icon(Icons._delete_),
              onPressed: delete,
            )
          ]
              : <Widget>[],
        ),
        body:Container(

          child: ListView.builder(
            itemCount: multiSelectList.length,
            itemBuilder: (context, index) {
              return InkWell(
                onTap: () {},
                child: MultiSelectItem(
                  isSelecting: controller.isSelecting,
                  onSelected: () {
                    setState(() {
                      controller.toggle(index);
                    });
                  },
                  child:Container(
                      height:75,
                    margin: EdgeInsets.only(left:15,right:15,top:15),
                    decoration: BoxDecoration(
                      borderRadius: BorderRadius.circular(12),
                      color: Colors._transparent_,
                    ),
                    child:Card(
                      color:controller.isSelected(index)
                          ? Colors._grey_.shade200:Colors._white_,
                      shape: RoundedRectangleBorder(

                        borderRadius: BorderRadius.all(Radius.circular(8.0)),

                      ),
                      child:Padding(
                        padding:EdgeInsets.symmetric(vertical:10, horizontal: 12),
                        child: Row(
                          children: [
                            _//contentPadding: EdgeInsets.symmetric(vertical: 12, horizontal: 16),_ ClipRRect(
                              borderRadius: BorderRadius.circular(12),
                              child:Image.asset(multiSelectList[index]['images'],fit:BoxFit.cover,width:60,height:60,),
                            ),
                            SizedBox(width:20,),
                            Text(multiSelectList[index]['desc'], style: TextStyle(fontSize:14)),
                          ],
                        ),
                      ),
                    ),
                  ),
                ),
              );
            },
          ),
        ),
      ),
    );
  }
}
```

## 结语:

在本文中，我解释了 Explore GetIt In Flutter，你可以根据自己的修改和实验，这个小介绍来自 Explore GetIt In Flutter 从我们这边的演示。

我希望这个博客将为您提供充分的信息，在尝试在您的 Flutter 项目探索 GetIt 在 Flutter 。我们向你展示了什么是探索和 Flutter 是在您的 Flutter 应用的工作，所以请尝试它。

如果我做错了什么，请在评论中告诉我，我很乐意改进。

---

© 猫哥

- https://ducafecat.tech/

- https://github.com/ducafecat

- 微信群 ducafecat

- b 站 https://space.bilibili.com/404904528

![](https://ducafecat.tech/img/public-qrcode.png)

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
