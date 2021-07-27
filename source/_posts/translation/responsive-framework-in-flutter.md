---
title: Flutter 中的响应式框架
tags: flutter
categories: 译文
date: 2021-07-27 00:00:00
---

## 猫哥说

这是个自动管理响应界面处理的组件，比较适合在 flutter web 的项目中。

自动管理了你的 Resizing、最大、最小尺寸、Scaling 缩放比例，但是我没有发现布局的控制，但是这已经很不错了，又可以少写代码了。

- 最小尺寸效果

https://gallery.codelessly.com/flutterwebsites/minimal/?utm_medium=link&utm_campaign=demo#/

- Flutter.dev 官网

https://gallery.codelessly.com/flutterwebsites/flutterwebsite/?utm_medium=link&utm_campaign=demo

![](2021-07-27-08-30-34.png)

![](2021-07-27-08-31-01.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/responsive-framework-in-flutter-74b8b2a360a9

## 代码

https://github.com/ducafecat/getx_quick_start

## 参考

- https://pub.dev/packages/responsive_framework

## 正文

![](2021-07-27-08-14-15.png)

Flutter 是 Google 的用户界面工具宝库，用于从一个代码库生成优秀的、本地编译的 iOS 和 Android 应用程序。为了构建任何应用程序，我们从小部件开始ー Flutter 应用程序的构建方块。窗口小部件根据当前的设计和状态描述他们的视图应该类似的内容。它整合了一个文本小部件、行小部件、列小部件、容器小部件等等。

本文利用 Flutter 响应框架软件包对 Flutter 响应框架进行了研究。有了软件包的帮助，我们可以很容易地实现一个响应屏幕。那么让我们开始吧。

### 响应式框架

响应式框架会自动调整你的用户界面以适应不同的屏幕尺寸。创建您的用户界面一次，并有它显示像素完美的移动，平板电脑和桌面！

支持多种显示大小通常意味着多次重新创建相同的布局。在传统的 Bootstrap 方法下，构建响应式 UI 是耗时、令人沮丧和重复的。此外，让一切像素完美几乎是不可能的，简单的编辑需要几个小时。

### 实施方案

- 第一步: 添加依赖项

将依赖项添加到 pubspec.yaml 文件。

依赖性:

```yaml
dependencies:
  responsive_framework: ^0.1.4
```

- 第二步: 导入

```dart
import 'package:responsive_framework/responsive_framework.dart';
```

- 第三步: 启用 AndriodX

```
org.gradle.jvmargs=-Xmx1536M
android.enableR8=true
android.useAndroidX=true
android.enableJetifier=true
```

### 代码实现

> 你需要分别在你的代码中实现它

在创建类似于响应式框架的 UI 之前，我们在 main 的 Material 应用部件中添加了 ResponsiveWrapper.builder() 。文件中初始化 MaxWith，MinWith 和 Breakpoints 的 List 类型，在它内部调整设备的大小，如移动设备，平板电脑，桌面，并且自动缩放值是定义的，让我们理解它与下面的代码引用。

```dart
ResponsiveWrapper.builder(
    BouncingScrollWrapper.builder(context, widget!),
    maxWidth: 1200,
    minWidth: 450,
    defaultScale: true,
    breakpoints: [
      ResponsiveBreakpoint.resize(450, name: MOBILE),
      ResponsiveBreakpoint.autoScale(800, name: MOBILE),
      ResponsiveBreakpoint.autoScale(800, name: TABLET),
      ResponsiveBreakpoint.autoScale(1000, name: TABLET),
      ResponsiveBreakpoint.resize(1200, name: DESKTOP),
      ResponsiveBreakpoint.autoScale(2460, name: "4K"),
    ],
    background: Container(color: Color(0xFFF5F5F5))
  ),
```

自动缩放按比例缩小和扩展布局，保持 UI 的精确外观。这就消除了手动调整布局以适应移动设备、平板电脑和桌面的需要。

在此之后，我们创建了一个用户配置文件屏幕，其中是用户的图像，以及两种不同类型的列表，其中图像和一些文本已经给出。

### 全部代码

```dart
import 'package:flutter/material.dart';
import 'package:responsive_framwork_demo/Constants/Constants.dart';
import 'package:responsive_framwork_demo/device_size.dart';
import 'package:responsive_framwork_demo/model/popular_course_model.dart';
import 'package:responsive_framwork_demo/model/result_model.dart';

class ProfileScreen extends StatefulWidget {
  @override
  _ProfileScreenState createState() => _ProfileScreenState();
}

class _ProfileScreenState extends State<ProfileScreen> {
  late List<PopularCourseModel> popularCourseModel;
  late List<ResultModel> resultModel;

  @override
  void initState() {
    popularCourseModel = Constants.getPopularCourseModel();
    resultModel = Constants.getResultModel();

    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.white,
      appBar: AppBar(
        backgroundColor: Colors.white,
        elevation: 0.0,
        title: Text(
          'PROFILE',
          style: TextStyle(
              fontFamily: 'Poppins Medium',
              color: Colors.black,
              fontSize: 15,
              fontWeight: FontWeight.bold,
              letterSpacing: 0.5),
        ),
        centerTitle: true,
      ),
      body: SingleChildScrollView(
        child: Container(
          padding: EdgeInsets.only(top: 20),
          child: Column(
            children: [
              ClipOval(
                child: CircleAvatar(
                  maxRadius: 50,
                  child: Image.asset(
                    'assets/images/mans_img.jpeg',
                    width: DeviceSize.width(context),
                    fit: BoxFit.fill,
                  ),
                ),
              ),
              SizedBox(
                height: 30,
              ),
              Text(
                'Crowley Singer',
                style: TextStyle(
                    fontFamily: 'Poppins Medium',
                    color: Colors.black,
                    fontSize: 15,
                    fontWeight: FontWeight.bold,
                    letterSpacing: 0.3),
              ),
              Container(
                margin: EdgeInsets.only(top: 25, left: 20, right: 20),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceAround,
                  children: [
                    Column(
                      children: [
                        Text(
                          '22',
                          style: TextStyle(
                              fontFamily: 'Poppins Medium',
                              color: Colors.black,
                              fontSize: 15,
                              fontWeight: FontWeight.bold,
                              letterSpacing: 0.5),
                        ),
                        SizedBox(
                          height: 10,
                        ),
                        Text(
                          'Courses',
                          style: TextStyle(
                              fontFamily: 'Poppins Regular',
                              color: Colors.black45,
                              fontSize: 11,
                              fontWeight: FontWeight.bold,
                              letterSpacing: 0.3),
                        ),
                      ],
                    ),
                    Container(
                      height: 32,
                      width: 1,
                      color: Colors.black12,
                    ),
                    Column(
                      children: [
                        Text(
                          '32',
                          style: TextStyle(
                              fontFamily: 'Poppins Medium',
                              color: Colors.black,
                              fontSize: 15,
                              fontWeight: FontWeight.bold,
                              letterSpacing: 0.5),
                        ),
                        SizedBox(
                          height: 10,
                        ),
                        Text(
                          'Mentors',
                          style: TextStyle(
                              fontFamily: 'Poppins Regular',
                              color: Colors.black45,
                              fontSize: 11,
                              fontWeight: FontWeight.bold,
                              letterSpacing: 0.3),
                        ),
                      ],
                    ),
                    Container(
                      height: 32,
                      width: 1,
                      color: Colors.black12,
                    ),
                    Column(
                      children: [
                        Text(
                          '48',
                          style: TextStyle(
                              fontFamily: 'Poppins Medium',
                              color: Colors.black,
                              fontSize: 15,
                              fontWeight: FontWeight.bold,
                              letterSpacing: 0.5),
                        ),
                        SizedBox(
                          height: 10,
                        ),
                        Text(
                          'Friends',
                          style: TextStyle(
                              fontFamily: 'Poppins Regular',
                              color: Colors.black45,
                              fontSize: 11,
                              fontWeight: FontWeight.bold,
                              letterSpacing: 0.3),
                        ),
                      ],
                    ),
                  ],
                ),
              ),
              Container(
                margin: EdgeInsets.only(top: 30, left: 20, right: 20),
                height: 1,
                color: Colors.black12,
                width: DeviceSize.width(context),
              ),
              Container(
                margin: EdgeInsets.only(top: 30, left: 20, right: 20),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    Text(
                      'YOUR COURSES',
                      style: TextStyle(
                          fontFamily: 'Poppins Medium',
                          color: Colors.black,
                          fontSize: 12,
                          fontWeight: FontWeight.w700,
                          letterSpacing: 0.5),
                    ),
                    Icon(Icons.more_horiz),
                  ],
                ),
              ),
              Container(
                margin: EdgeInsets.only(top: 10, left: 20, right: 20),
                height: DeviceSize.height(context) / 7,
                // color:Colors.purple,
                child: ListView.separated(
                  separatorBuilder: (BuildContext context, int index) {
                    return SizedBox(width: 10);
                  },
                  scrollDirection: Axis.horizontal,
                  // padding:EdgeInsets.only(left:10),
                  //shrinkWrap: true,
                  itemCount: popularCourseModel.length,
                  itemBuilder: (BuildContext context, int index) {
                    return _buildYourCourseModel(
                        popularCourseModel[index], index);
                  },
                ),
              ),
              Container(
                margin: EdgeInsets.only(top: 30, left: 20, right: 20),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    Text(
                      'YOUR PROGRESS',
                      style: TextStyle(
                          fontFamily: 'Poppins Medium',
                          color: Colors.black,
                          fontSize: 12,
                          fontWeight: FontWeight.w700,
                          letterSpacing: 0.5),
                    ),
                    Icon(Icons.more_horiz),
                  ],
                ),
              ),
              Container(
                //  height:DeviceSize.height(context),
                width: DeviceSize.width(context),

                margin: EdgeInsets.only(top: 20),

                child: ListView.separated(
                  separatorBuilder: (BuildContext context, int index) {
                    return SizedBox(height: 10);
                  },
                  scrollDirection: Axis.vertical,
                  // padding:EdgeInsets.only(left:10),
                  physics: NeverScrollableScrollPhysics(),
                  shrinkWrap: true,
                  itemCount: resultModel.length,
                  itemBuilder: (BuildContext context, int index) {
                    return _buildResultModel(resultModel[index], index);
                  },
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildYourCourseModel(PopularCourseModel items, int index) {
    return Container(
      width: 70,
      child: Column(
        //mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: <Widget>[
          Container(
            decoration: BoxDecoration(
              shape: BoxShape.circle,
              color: Colors.white,
              boxShadow: [
                BoxShadow(
                  color: Colors.blueGrey.shade50,
                  offset: const Offset(
                    5.0,
                    5.0,
                  ),
                  blurRadius: 10.0,
                  spreadRadius: 2.0,
                ), //BoxShadow
                BoxShadow(
                  color: Colors.white,
                  offset: const Offset(0.0, 0.0),
                  blurRadius: 0.0,
                  spreadRadius: 0.0,
                ), //BoxShadow
              ],
            ),
            child: ClipOval(
              child: CircleAvatar(
                backgroundColor: Colors.white,
                maxRadius: 30,
                //  child:Image.asset('assets/images/earth.png',height:45,color:Colors.blueAccent,) ,
                child: Padding(
                  padding: EdgeInsets.all(4),
                  child: Image.asset(
                    items.img,
                    fit: BoxFit.cover,
                    scale: 7,
                  ),
                ),
              ),
            ),
          ),
          Expanded(
            child: Container(
              padding: EdgeInsets.only(top: 0),
              alignment: Alignment.center,
              child: Text(
                items.title,
                style: TextStyle(
                    fontFamily: 'Poppins Medium',
                    fontWeight: FontWeight.w700,
                    letterSpacing: 0.3,
                    fontSize: 11,
                    color: Colors.black),
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildResultModel(ResultModel items, int index) {
    return Container(
      margin: EdgeInsets.only(left: 20, right: 20),

      height: DeviceSize.height(context) / 9,

//     / color:Colors.pink,
      child: Row(
        children: [
          Container(
            decoration: BoxDecoration(
              shape: BoxShape.circle,
              color: Colors.white,
              boxShadow: [
                BoxShadow(
                  color: Colors.blueGrey.shade50,
                  offset: const Offset(
                    5.0,
                    5.0,
                  ),
                  blurRadius: 10.0,
                  spreadRadius: 2.0,
                ), //BoxShadow
                BoxShadow(
                  color: Colors.white,
                  offset: const Offset(0.0, 0.0),
                  blurRadius: 0.0,
                  spreadRadius: 0.0,
                ), //BoxShadow
              ],
            ),
            child: ClipOval(
              child: CircleAvatar(
                backgroundColor: Colors.white,
                maxRadius: 28,
                //  child:Image.asset('assets/images/earth.png',height:45,color:Colors.blueAccent,) ,
                child: Padding(
                  padding: EdgeInsets.all(4),
                  child: Image.asset(
                    items.img,
                    fit: BoxFit.cover,
                    scale: 7,
                  ),
                ),
              ),
            ),
          ),
          SizedBox(
            width: 20,
          ),
          Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                items.title,
                style: TextStyle(
                    fontFamily: 'Poppins Medium',
                    color: Colors.black,
                    fontSize: 11.5,
                    fontWeight: FontWeight.w700,
                    letterSpacing: 0.5),
              ),
              SizedBox(
                height: 5,
              ),
              Text(
                items.subTitle,
                style: TextStyle(
                    fontFamily: 'Poppins Regular',
                    color: Colors.black45,
                    fontSize: 11,
                    fontWeight: FontWeight.w700,
                    letterSpacing: 0.5),
              ),
            ],
          ),
        ],
      ),
    );
  }
}
```

### 总结

在这篇文章中，我解释了在 Flutter 响应框架，你可以根据自己的修改和实验，这个小介绍是从我们这边的 Flutter 响应框架演示。

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
