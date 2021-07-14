---
title: Flutter 自动生成json实体类
tags: flutter
categories: 译文
date: 2021-07-14 00:00:00
---

![](2021-07-14-08-07-05.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/automatic-generate-json-serializable-in-flutter-4c9d2d23ed88

## 代码

## 参考

- https://pub.dev/packages/json_serializable
- https://pub.dev/packages/json_annotation

## 正文

Flutter 是一个可移植的 UI 工具包。换句话说，它是一个全面的应用软件开发工具包(SDK) ，包括小部件和工具。Flutter 是一个免费的开源工具，用于开发移动、桌面和 web 应用程序。Flutter 是一种跨平台的开发工具。这意味着用同样的代码，我们可以同时创建 ios 和 android 应用程序。这是在整个过程中节省时间和资源的最佳方式。

在本文中，我们将探索使用 json_serializable 包和 json_annotation，并了解如何使用它将我们的模型解析到 JSON 并通过序列化生成我们自己的代码。我们开始吧。

### JSON Serializable

JSON (JSON)是一种数据格式，它将对象编码成字符串。这种数据可以很容易地在服务器和浏览器之间转换，也可以在服务器和服务器之间转换。序列化是将对象转换为相同字符串的过程。为此，我们使用 json 序列化包，但是它可以根据 json 注释库提供的注释为您生成一个模型类。

### Implementation

每当我们需要建立模型和工厂的时候。因为模型不会总是改变，所以你不需要总是改变模型。因此，为了使用 JSON，我们必须添加下面解释的一些包。

- 这是提供给 Dart 构建系统的。当它在用 json_annotation 定义的类中找到带注释的成员时，就会生成代码
- 它定义了 JSON_serializable 用于创建 JSON 序列化、反序列化类型的代码的注释
- 我们使用 build_runner 包来生成使用 dart 代码的文件

现在让我们看看如何将所有这些包添加到 pubspec 中。

- 第一步: 添加依赖项

> 将依赖项添加到 pubspec ー yaml 文件。

```yaml
---
dependencies:
  flutter:
    sdk: flutter
  # The following adds the Cupertino Icons font to your application.
  # Use with the CupertinoIcons class for iOS style icons.
  cupertino_icons: ^0.1.2
  json_annotation: ^4.0.1

dev_dependencies:
  flutter_test:
    sdk: flutter

  build_runner: ^2.0.5
  json_serializable: ^4.1.3
```

- 第二步: Importing

```dart
import 'package:json_annotation/json_annotation.dart';
import 'package:build_runner/build_runner.dart';
import 'package:json_serializable/json_serializable.dart';
```

- 第三步: 启用 AndriodX

```
org.gradle.jvmargs=-Xmx1536M
android.enableR8=true
android.useAndroidX=true
android.enableJetifier=true
```

### 如何实现 dart 文件中的代码

> 你需要分别在你的代码中实现它

首先，我们将创建一个我们命名为 user.dart 的模型类。

现在我们将看到 Dart 如何使用 Dart: convert 库本机支持手动序列化。用户 dart 文件准备好了，我们将有一个数据 JSON 对象的列表，其中每个对象将有一个用户名，姓氏，和它的地址，我们已经在字符串类型的变量中定义了，你将看到在数据类中我们有两个我们需要创建函数，分别称为 fromJson 和 toJson，它们将 JSON 转换为我们的用户类。

```dart
import 'package:json_annotation/json_annotation.dart';
part 'user.g.dart';

@JsonSerializable()
class User {
  String name, lastName, add;
  bool subscription;

  User({this.name,this.lastName,this.add,this.subscription,});

  factory User.fromJson(Map<String,dynamic> data) => _$UserFromJson(data);

  Map<String,dynamic> toJson() => _$UserToJson(this);

}
```

现在，当我们运行 `build_runner` 命令时，json*serializer 将生成这个 `*$UserFromJson(json)`。我们将从中获得 `user.g.dart` 文件。

要运行 `build_runner` 命令，我们将在 Android Studio 中打开一个终端，并输入以下行。

```sh
flutter pub run build_runner build
```

当我们在构建运行程序中运行这个命令时，会出现一些行，过一段时间后它就成功生成了。

```sh
INFO] Generating build script...
[INFO] Generating build script completed, took 301ms[INFO] Initializing inputs
[INFO] Reading cached asset graph...[INFO] Reading cached asset graph completed, took 305ms[INFO] Checking for updates since last build...[INFO] Checking for updates since last build completed, took 1.5s[INFO] Running build...[INFO] Running build completed, took 4.7s[INFO] Caching finalized
dependency graph...[INFO] Caching finalized dependency graph completed, took 44ms[INFO] Succeeded after 4.8s with 0 outputs (1 actions)
```

在 `build_runner` 进程完成之后，我们在一个包含序列化代码的用户文件下面添加一个名为 user.g.dart 的新文件。当我们制作一个新的模型，然后我们流过这个过程。

```dart
// GENERATED CODE - DO NOT MODIFY BY HAND

part of 'user.dart';

// **************************************************************************
// JsonSerializableGenerator
// **************************************************************************

User _$UserFromJson(Map<String, dynamic> json) {
  return User(
    name: json['name'] as String,
    lastName: json['lastName'] as String,
    add: json['add'] as String,
    subscription: json['subscription'] as bool,
  );
}

Map<String, dynamic> _$UserToJson(User instance) => <String, dynamic>{
      'name': instance.name,
      'lastName': instance.lastName,
      'add': instance.add,
      'subscription': instance.subscription,
    };
```

在此之后，我们创建了一个类，其中显示了一个列表项，我们已经为该列表项定义了一个未来的生成器列表视图生成器，其中我们已经在文本小部件中定义了用户列表的项。

```dart
FutureBuilder<List<User>>(
    future: getData(),
    builder: (context, data) {
      if (data.connectionState != ConnectionState.waiting &&
          data.hasData) {
        var userList = data.data;
        return ListView.builder(
            itemCount: userList.length,
            itemBuilder: (context, index) {
              var userData = userList[index];
              return Container(
                height: 100,
                margin: EdgeInsets.only(top: 30, left: 20, right: 20),
                decoration: BoxDecoration(
                  color: Colors.grey.shade200,
                  borderRadius: BorderRadius.all(Radius.circular(10)),
                ),
                padding: EdgeInsets.all(15),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  mainAxisAlignment: MainAxisAlignment.spaceAround,
                  children: [
                    Text(
                      'First Name: ' + userData.name,
                      style: TextStyle(
                          fontWeight: FontWeight.w600,),
                    ),
                  ],
                ),
              );
            });
      } else {
        return Center(
          child: CircularProgressIndicator(),
        );
      }
    })
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-07-14-07-56-13.png)

### Code File

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:flutter_json_serilization_exm/main.dart';
import 'package:flutter_json_serilization_exm/model/user.dart';

class JsonSerilization extends StatefulWidget {
  @override
  _JsonSerilizationState createState() => _JsonSerilizationState();
}

class _JsonSerilizationState extends State<JsonSerilization> {
  Future<List<User>> getData() async {
    return await Future.delayed(Duration(seconds: 2), () {
      List<dynamic> data = jsonDecode(JSON);
      List<User> users = data.map((data) => User.fromJson(data)).toList();
      return users;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Json Serialization Demo"),
      ),
      body: Container(
        child: FutureBuilder<List<User>>(
            future: getData(),
            builder: (context, data) {
              if (data.connectionState != ConnectionState.waiting &&
                  data.hasData) {
                var userList = data.data;
                return ListView.builder(
                    itemCount: userList.length,
                    itemBuilder: (context, index) {
                      var userData = userList[index];
                      return Container(
                        height: 100,
                        margin: EdgeInsets.only(top: 30, left: 20, right: 20),
                        decoration: BoxDecoration(
                          color: Colors.grey.shade200,
                          borderRadius: BorderRadius.all(Radius.circular(10)),
                        ),
                        padding: EdgeInsets.all(15),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          mainAxisAlignment: MainAxisAlignment.spaceAround,
                          children: [
                            Text(
                              'First Name: ' + userData.name,
                              style: TextStyle(
                                  fontWeight: FontWeight.w600, fontSize: 15),
                            ),
                            Text(
                              'Last Name: ' + userData.lastName,
                              style: TextStyle(
                                  fontWeight: FontWeight.w600, fontSize: 15),
                            ),
                            Text(
                              'Add: ' + userData.add,
                              style: TextStyle(
                                  fontWeight: FontWeight.w600, fontSize: 15),
                            ),
                          ],
                        ),
                      );
                    });
              } else {
                return Center(
                  child: CircularProgressIndicator(),
                );
              }
            }),
      ),
    );
  }
}
```

### Conclusion

在这篇文章中，我解释了自动生成 JSON 系列化 Flutter，你可以根据自己的修改和实验，这个小介绍是从自动生成 JSON 系列化 Flutter 演示从我们这边。

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
