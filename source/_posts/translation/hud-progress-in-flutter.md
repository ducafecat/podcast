---
title: Flutter 中使用 HUD Progress 组件
tags: flutter
categories: 译文
date: 2021-04-28 00:00:00
---

![](2021-04-27-22-36-09.png)

## 原文

> https://medium.com/flutterdevs/hud-progress-in-flutter-281ed0f644d0

## 前言

在 flutter 中，我们显示任何进度指示器，因为我们的应用程序是繁忙的或在搁置，为此，我们显示一个循环的进度指示器。覆盖加载屏幕显示一个进度指示器，也称为模态进度 HUD 或平视显示，这通常意味着应用程序正在加载或执行一些工作。

在本文中，我们将利用 HUD 进程程序包来探讨平视显示器在 flutter 方面的进展。有了这个软件包，我们可以很容易地实现平视显示的颤振进度。那么让我们开始吧。

## pub.dev

https://pub.dev/packages/flutter_progress_hud

https://pub.dev/packages/flutter_progress_hud/example

## 正文

### HUD Progress

Flutter HUD Progress 是一种进度指示器库，就像一个循环的进度指示器。在这里，HUD 意味着一个抬头显示器/进度弹出对话框将打开以上的屏幕，将有一个循环的进度指示器。使用这个库，我们可以使用我们的 flutter 。应用程序可以显示循环进度指示器。

![](2021-04-27-22-27-36.png)

### 属性

- borderColor:
  边框/颜色: 边框颜色属性用于更改指示符背景边框颜色

- backgroundColor:
  背景颜色: 背景颜色属性用于更改指示器背景的颜色

- indicatorColor:
  标志/颜色: 背景颜色属性用于更改指示器背景的颜色

- textStyle:
  文字样式: 属性用于指示符下面显示的文本，文本的颜色和样式可以在该属性中更改

### 安装

- 第一步: 添加依赖项

将依赖项添加到 pubspec ー yaml 文件。

```yaml
dependencies:
  flutter_progress_hud: ^2.0.0
```

- 第二步: 导包

```dart
import 'package:flutter_progress_hud/flutter_progress_hud.dart';
```

- 第三步: 启用 AndriodX

```
org.gradle.jvmargs=-Xmx1536M
android.enableR8=true
android.useAndroidX=true
android.enableJetifier=true
```

### 例子

在 `lib` 目录中创建一个名为 `progress_hud_demo.dart` 的新 dart 文件。

在创建 Flutter HUD Progress 之前，我们包装了一个进度遮光罩的容器，其次是建设者类。在内部，我们使用了我们的小部件，并定义了进度指示器的边框颜色和背景颜色。让我们详细地了解一下这一点。

```dart
ProgressHUD(
  borderColor:Colors.orange,
  backgroundColor:Colors.blue.shade300,
  child:Builder(
    builder:(context)=>Container(
      height:DeviceSize.height(context),
      width:DeviceSize.width(context),
      padding:EdgeInsets.only(left:20,right:20,top:20),
    ),
  ),
),
```

现在我们已经采取了一个按钮，在其中指示器设置持续时间 5 秒的指示器时间未来。`delayed()` 并显示进度的文本。

```dart
Container(
  margin: EdgeInsets.only(
      left:20.0, right:20.0, top:55.0),
  child: CustomButton(
    mainButtonText:'Submit',
    callbackTertiary:(){
      final progress = ProgressHUD.of(context);
      progress.showWithText('Loading...');
      Future.delayed(Duration(seconds:5), () {
        progress.dismiss();
      });
    },
    color:Colors.blue,
  ),
),
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-04-27-22-32-07.png)

### 完整代码

```dart
import 'package:flutter/material.dart';
import 'package:progress_hud_demo/shared/custom_button.dart';
import 'package:progress_hud_demo/shared/custom_text_field.dart';
import 'package:progress_hud_demo/themes/device_size.dart';
import 'package:flutter_progress_hud/flutter_progress_hud.dart';
class ProgressHudDemo extends StatefulWidget {
  @override
  _ProgressHudDemoState createState() => _ProgressHudDemoState();
}

class _ProgressHudDemoState extends State<ProgressHudDemo> {
  bool _isInAsyncCall = false;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor:Colors.white,
      appBar:AppBar(
        backgroundColor:Colors.blue,
        title:Text('Flutter HUD Progress Demo'),
        elevation:0.0,
      ),
      body:ProgressHUD(
        borderColor:Colors.orange,
        backgroundColor:Colors.blue.shade300,
        child:Builder(
          builder:(context)=>Container(
            height:DeviceSize.height(context),
            width:DeviceSize.width(context),
            padding:EdgeInsets.only(left:20,right:20,top:20),
            child:Column(
              crossAxisAlignment:CrossAxisAlignment.start,
              children: [

                Column(
                  crossAxisAlignment:CrossAxisAlignment.start,
                  children: [
                    Text('Sign In',style:TextStyle(fontFamily:'Roboto Bold',fontSize:27,fontWeight:FontWeight.bold),),
                  ],
                ),

                SizedBox(height:50,),


                Column(
                  children: [
                    CustomTextField(hintText: 'Email', type:TextInputType.text, obscureText: false),


                    SizedBox(height:35,),

                    CustomTextField(hintText: 'Password', type:TextInputType.text, obscureText: true),
                  ],
                ),

                Container(
                  margin: EdgeInsets.only(
                      left:20.0, right:20.0, top:55.0),
                  child: CustomButton(
                    mainButtonText:'Submit',
                    callbackTertiary:(){
                      final progress = ProgressHUD.of(context);
                      progress.showWithText('Loading...');
                      Future.delayed(Duration(seconds:5), () {
                        progress.dismiss();
                      });
                    },
                    color:Colors.blue,
                  ),
                ),

              ],
            ),
          ),
        ),
      ),
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
