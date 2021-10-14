---
title: 桌面 Flutter 应用程序
tags: flutter
categories: 译文
date: 2021-10-14 00:00:00
---

![](2021-10-14-08-58-04.png)

## 原文

> https://medium.com/flutterdevs/flutter-app-for-desktop-d949f4d20cdb

## 代码

https://github.com/flutter-devs/flutter_app_for_desktop

## 参考

- https://flutter.dev/docs/get-started/install/windows#windows-setup

## 正文

了解如何设置运行桌面上的应用程序在您的 Flutter 应用程序

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/849801885669e4585a027127ff4966e2793c12db07416d68d2f0dbb4459b1323.png)

在 Flutter 中，Flutter 应用程序屏幕上的每个组件都是一个小工具。屏幕的透视图完全依赖于用于构建应用程序的小部件的选择和分组。此外，应用程序代码的结构是一个小部件树。

在这个博客中，我们将了解如何在桌面上运行 Flutter 应用程序，以及设置这个应用程序的要求是什么？.我们将看到一步一步的流程，并创建一个应用程序来理解桌面应用程序的构建过程。

### Flutter :

> “ Flutter 是谷歌的 UI 工具包，它可以帮助你在创纪录的时间内用一个代码库为移动设备、网络和桌面构建漂亮的本地组合应用程序。”

它是免费和开源的。它最初是由谷歌发展而来，目前由 ECMA 标准监管。 Flutter 应用程序利用 Dart 编程语言来制作应用程序。这个 dart 编程和其他编程语言有一些相同的亮点，比如 Kotlin 和 Swift，并且可以被转换成 JavaScript 代码。

### Flutter 的好处:

Flutter 为我们提供了在多个平台上运行应用程序的机会。比如，网络，桌面，Android/iOS。市场上有许多语言可以在多种平台上运行应用程序。但与其他语言相比，flutter 在不同平台上运行应用程序所需的时间更少。因为 flutter 不像其他语言那样使用 mediator 桥来运行应用程序。因此，在不同的平台上运行应用程序时， Flutter 速度很快。下面是一些关键点

> 所有平台中相同的用户界面和业务逻辑。
>
> 减少代码和开发时间。
>
> 类似于本地应用程序的性能。
>
> 自定义，动画 UI 可用于任何复杂的小部件。
>
> 自己的渲染图形引擎，即 skia。
>
> 简单的平台专用逻辑实现。
>
> 超越手机的潜在能力。

### Flutter 平台特定的标准:

> 特定于 Android 平台
>
> 特定于 iOS 平台
>
> 特定于 Web 平台
>
> 桌面平台专用

### 为桌面平台上运行的应用程序设置特定:

1.  首先，创建您的 Flutter 项目

2.  然后将你的频道切换到贝塔 Flutter 频道。因为它涵盖了桌面支持，在 Beta 版本中可以使用，并且在 Beta 头条大新闻中可以使用这个命令。

```
> flutter channel beta Flutter
```

转到 flutter 文档，点击窗口设置选项，阅读文档。

https://flutter.dev/docs/get-started/install/windows#windows-setup

3. 然后使用这个命令启用你的窗口。

```
> flutter config — enable-windows-desktop
```

> 查看下面的文档

https://flutter.dev/docs/get-started/install/windows#windows-setup

5. 启用窗口后 **重新启动 android studio** 。

6. 重新启动 android studio 之后，现在使用下面的命令创建 windows 支持目录。

```
> flutter create.
```

7. 现在安装 visual studio 去这个链接。

https://visualstudio.microsoft.com/downloads/

8. 在 visual studio 安装后，你终于可以在桌面上运行你的应用了。选择桌面设备在你的 `android studio` 和运行应用程序。

### 实施方案:

现在我正在设计一个在桌面上测试的页面。你可以在桌面上运行任何应用程序。我在这里展示的只是这个应用程序的最后一个页面代码实现，它是代码片段。如果你想查看完整的代码，请访问下面给出的 Github 链接。

> 在这个网页上，我正在设计一个课程列表卡与图像和文字为购买课程列表。

```dart
import 'package:e_learning_app/constants/constants.dart';
import 'package:e_learning_app/model/Courses_items_model.dart';
import 'package:e_learning_app/model/read_blog_model.dart';
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'dart:io' show Platform;

class ReadScreenView extends StatefulWidget {
  const ReadScreenView({Key? key}) : super(key: key);

  @override
  _ReadScreenViewState createState() => _ReadScreenViewState();
}

class _ReadScreenViewState extends State<ReadScreenView> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: _buildBody(),
    );
  }

  Widget _buildBody() {
    return Container(
      height: MediaQuery._of_(context).size.height,
      child: Stack(
        children: [
          Container(
            height: MediaQuery._of_(context).size.height,
            _//color: Colors.cyan,_ child: Container(
              margin: EdgeInsets.only(bottom: 350),
              height: 250,
              decoration: BoxDecoration(
                  color: Color(0xffff9b57),
                  borderRadius: BorderRadius.only(
                      bottomLeft: Radius.circular(40),
                      bottomRight: Radius.circular(40))),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Container(
                      padding: EdgeInsets.only(
                          left: 20,
                          right: 20,
                          top: 30),
                      child: Row(
                          mainAxisAlignment: MainAxisAlignment.spaceBetween,
                          children: [
                            InkWell(
                             onTap:(){
                               Navigator._pop_(context);
                            },
                              child: Container(
                                _//margin: EdgeInsets.only(right: 25,top: 10),_ height: 30,
                                width: 30,
                                decoration: BoxDecoration(
                                  borderRadius: BorderRadius.circular(5),
                                  color: Color(0xfff3ac7c),
                                ),
                                child: Icon(
                                  Icons._arrow_back_,
                                  color: Colors._white_,
                                  size: 20,
                                ),
                              ),
                            ),
                            Container(
                              _// margin: EdgeInsets.only(right: 25,top: 10),_ height: 30,
                              width: 30,
                              decoration: BoxDecoration(
                                borderRadius: BorderRadius.circular(5),
                                color: Color(0xfff3ac7c),
                              ),
                              child: Icon(
                                Icons._menu_,
                                color: Colors._white_,
                                size: 20,
                              ),
                            ),
                          ])),
                  Container(
                    padding: EdgeInsets.only(left: 20, right: 20, top: 16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Container(
                          child: Text(
                            "Purchase Courses",
                            style: TextStyle(
                              color: Colors._white_,
                              fontSize: 20,
                              _//fontWeight: FontWeight.bold_ ),
                          ),
                        ),
                        SizedBox(
                          height: 5,
                        ),
                        Container(
                          child: Text(
                            "Categories",
                            style: TextStyle(
                              color: Colors._white_,
                              fontSize: 12,
                              _//fontWeight: FontWeight.bold_ ),
                          ),
                        ),
                      ],
                    ),
                  ),
                  Container(
                    padding: EdgeInsets.only(left: 20, top: 16),
                    height: 140,
                    alignment: Alignment._center_,
                    _//color: Colors.orange,_ child: ListView.builder(
                        scrollDirection: Axis.horizontal,
                        shrinkWrap: true,
                        itemCount: readBlogList.length,
                        itemBuilder: (BuildContext context, int index) {
                          return _buildCategorySection(readBlogList[index]);
                        }),
                  ),
                ],
              ),
            ),
          ),
          Positioned(
            top: 260,
            left: 0,
            right: 0,
            bottom: 0,
            child: SizedBox(
              height: MediaQuery._of_(context).size.height - 260,
              width: MediaQuery._of_(context).size.width,
              child: Container(
                _//color: Colors.yellow,_ padding: EdgeInsets.only(left: 4, right: 4),
                width: MediaQuery._of_(context).size.width,
                child: ListView.builder(
                    _//physics:  NeverScrollableScrollPhysics(),_ scrollDirection: Axis.vertical,
                    shrinkWrap: true,
                    itemCount: readBlogList.length,
                    itemBuilder: (BuildContext context, int index) {
                      return _buildPurchaseCourses(readBlogList[index]);
                    }),
              ),
            ),
          ),
          Positioned(
            bottom: 0,
            child: Container(
              padding:
                  EdgeInsets.only(left: 20, right: 20, top: 10, bottom: 10),
              height: 70,
              width: MediaQuery._of_(context).size.width,
              color: Colors._white_,
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        "Purchase Courses",
                        style: TextStyle(
                          color: Colors._black_,
                          fontSize: 14,
                          _//fontWeight: FontWeight.bold_ ),
                      ),
                      Text(
                        "5",
                        style: TextStyle(
                          color: Colors._red_,
                          fontSize: 20,
                          _//fontWeight: FontWeight.bold_ ),
                      ),
                    ],
                  ),
                  Container(
                    height: 40,
                    width: 130,
                    margin: EdgeInsets.only(bottom: 10),
                    decoration: BoxDecoration(
                        borderRadius: BorderRadius.circular(20),
                        color: Color(0xffdc4651)),
                    child: Container(
                      padding: EdgeInsets.only(left: 20, right: 20),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            "Category",
                            style: TextStyle(color: Colors._white_),
                          ),
                          Icon(Icons._arrow_drop_down_, color: Colors._white_)
                        ],
                      ),
                    ),
                  )
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildCategorySection(ReadBlogModel readBlogList) {
    return Container(
      height: 50,
      width: 110,
      child: Card(
        color: Colors._white_,
        child: Column(
          _//mainAxisAlignment: MainAxisAlignment.spaceBetween,_ children: [
            Container(
                height: 90,
                child: ClipRRect(
                  borderRadius: BorderRadius.only(topLeft: Radius.circular(5),topRight: Radius.circular(5)),
                  child: Image.network(
                    readBlogList.image!,
                    fit: BoxFit.fill,
                    height: 50,
                    width: 110,
                  ),
                )),
            Container(
              padding: EdgeInsets.all(5),
              child: Text(
                "Categories",
                style: TextStyle(
                  color: Colors._black_,
                  fontSize: 12,
                  _//fontWeight: FontWeight.bold_ ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildPurchaseCourses(ReadBlogModel readBlogList) {
    return Container(
        margin: EdgeInsets.only(left: 10, right: 10),
        _//padding: EdgeInsets.only(left: 10,right: 20),_ height: 80,
        child: Card(
          child: Container(
            padding: EdgeInsets.only(left: 10, right: 10),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Container(
                  height: 40,
                  width: 40,
                  child: ClipRRect(
                    child: Image.network(
                      readBlogList.image!,
                      fit: BoxFit.fill,
                    ),
                    borderRadius: BorderRadius.circular(8),
                  ),
                ),
                _//SizedBox(width: 20,),_ Container(
                  padding: EdgeInsets.only(right: 120),
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Text(
                        readBlogList.title!,
                        style: TextStyle(
                          fontSize: 12,
                          _//fontWeight: FontWeight.bold_ ),
                      ),
                      Text(
                        readBlogList.subTitle!,
                        style: TextStyle(
                          fontSize: 12,
                          _//fontWeight: FontWeight.bold_ ),
                      ),
                    ],
                  ),
                ),
                _// SizedBox(width: 130,),_ Container(
                  height: 30,
                  width: 30,
                  decoration: BoxDecoration(
                    borderRadius: BorderRadius.circular(20),
                    color: Color(0xfffee8db),
                  ),
                  child: Icon(
                    Icons._done_,
                    color: Color(0xffdd8e8d),
                    size: 16,
                  ),
                )
              ],
            ),
          ),
        ));
  }

}
```

当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/7693457c5d55913584ff148a990f3ef90661e7541f04adfd58c2ab3e859de435.gif)

### 结语:

在本文中，我已经简单地解释了桌面应用程序的设置。

### GitHub Link:

https://github.com/flutter-devs/flutter_app_for_desktop

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
