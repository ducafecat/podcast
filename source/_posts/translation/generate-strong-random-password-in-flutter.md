---
title: Flutter 强随机密码生成器
tags: flutter
categories: 译文
date: 2021-06-29 00:00:00
---

![](2021-06-29-05-57-15.png)

## 猫哥说

有的时候手机上也需要强密码生成器，这样就不用再 PC 上生成后复制过去了，用 Flutter 自己来打造一个吧。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/generate-strong-random-password-in-flutter-723e631727cc

## 代码

https://github.com/flutter-devs/flutter_generate_strong_random_password_demo

## 参考

## 正文

![](2021-06-29-05-41-13.png)

只是让我对你能够有效地做到的令人愉快的用户界面感兴趣，而且显然，它允许你同时为两个平台创建。存在的重要原因是用小部件构造应用程序。它描述了您的应用程序视图应该如何看待它们的当前设置和状态。当您修改代码时，小部件通过计算过去和当前小部件之间的对比来重新构建它的描述，以确定在应用程序的 UI 中呈现的可忽略的更改。

在这个博客中，我们将探索在 Flutter 中生成强随机密码。我们将实现一个生成随机密码演示程序，并了解如何创建一个强大的随机密码产生您的 Flutter 应用程序。

### 生成随机密码

毫无疑问，我们可以创建复杂的密码，并用于您的客户端帐户。选择长度和字符，以利用和生成您的密码安全。

![](generate-password.gif)

这个演示视频展示了如何在一次 Flutter 中创建一个强大的随机密码。它显示了如何生成强随机密码将工作在您的 Flutter 应用程序。当用户点击按钮，然后，密码将产生的组合长度，字符，数字，特殊，低字母，和上游字母表。它将在文本表单字段上生成，用户还将复制生成的密码。它会显示在你的设备上。

### 如何实现 dart 文件中的代码

你需要分别在你的代码中实现它:

> 在 lib 文件夹中创建一个名为 generate \_ password. dart 的新 dart 文件。

在主体部分，我们将添加 Container。在内部，我们将添加一个 Column 小部件。在这个小部件中，我们将添加 mainAxisAlignmnet 和 crossAxisAlignmnet。我们将添加文本并将其包装到行中。

```dart
Container(
  padding: EdgeInsets.all(32),
  child: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    crossAxisAlignment: CrossAxisAlignment.center,
    children: [
      Row(
        children: [
          Text("Generate Strong Random Password",style: TextStyle(
              fontSize: 18, fontWeight: FontWeight.bold
          ),),
        ],
       SizedBox(height: 15,),
       TextFormField(),
       SizedBox(height: 15,),
       buildButtonWidget()
      ),
    ],
  ),),
```

现在我们将添加 TextFormFeld，我们将使 \_ contoller 的变量等于 TextEditingController ()。

```dart
final _controller = TextEditingController();
```

我们将真正的只读，因为密码生成时没有编辑。我们将假设 enableInteractiveSelection 并添加 InputDecoration 作为边框。

```dart
TextFormField(
  controller: _controller,
  readOnly: true,
  enableInteractiveSelection: false,
  decoration: InputDecoration(
      focusedBorder: OutlineInputBorder(
        borderSide: BorderSide(color: Colors.cyan,),
      ),
      enabledBorder: OutlineInputBorder(
        borderSide: BorderSide(color: Colors.cyan),
      ),
  ),
),
```

> 我们还将创建一个 dispose ()方法来释放控制器

```dart
@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```

在 TextFormField 中，我们还将创建一个后缀图标。在内部，我们将添加 IconButton ()。我们将添加一个复制图标和 onPressed 方法。在 onPressed 方法中，我们将添加最终数据等于 clipboard data，在括号中，我们将添加 \_ controller。文本和设置剪贴板上的数据。我们将显示一个 snackbar 时，复制图标按下，然后显示一个消息是“密码复制”。

```dart
suffixIcon: IconButton(
    onPressed: (){
      final data = ClipboardData(text: _controller.text);
      Clipboard.setData(data);

      final snackbar = SnackBar(
          content: Text("Password Copy"));

      ScaffoldMessenger.of(context)
        ..removeCurrentSnackBar()
        ..showSnackBar(snackbar);
    },
    icon: Icon(Icons.copy))
```

> 现在我们将创建一个 buildButtonWidget ()

我们将创建 buildButtonWidget () ，内部将返回一个标高按钮()。在这个按钮中，我们将添加标题按钮的样式并添加子属性。我们将添加文本“ Password Generate”，并在 child 属性中添加 onPressed 函数。在这个函数中，我们将添加一个等于 generatePassword ()的最终密码。我们将在下面的 generatePassword ()中进行深入的描述。添加 \_ controller。文本等于密码。

```dart
Widget buildButtonWidget() {
  return ElevatedButton(
      style: ElevatedButton.styleFrom(
          primary: Colors.black
      ),
      onPressed: (){
        final password = We will deeply describe;
        _controller.text = password;
      },
      child: Text("Password Generate",style: TextStyle(color: Colors.white),)
  );
}
```

> 我们将深入描述 generatePassword () :

在 lib 文件夹中创建一个名为 custom.dart 的新 dart 文件。

我们将创建一个 String generatePassword ()方法。在内部，我们将添加最终长度等于 20，letterLowerCase，letterUpperCase，number，special 字符。当我们使用一个强密码，然后真正的所有 bool 字母，isNumber，和 isSpecial。添加字符串字符并返回 List。生成()。添加 final indexRandom 等于 Random.secure ()。nextInt (chars.length)和 return chars [ indexRandom ]。

```dart
import 'dart:math';

String generatePassword({
  bool letter = true,
  bool isNumber = true,
  bool isSpecial = true,
}) {
  final length = 20;
  final letterLowerCase = "abcdefghijklmnopqrstuvwxyz";
  final letterUpperCase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  final number = '0123456789';
  final special = '@#%^*>\$@?/[]=+';

  String chars = "";
  if (letter) chars += '$letterLowerCase$letterUpperCase';
  if (isNumber) chars += '$number';
  if (isSpecial) chars += '$special';


  return List.generate(length, (index) {
    final indexRandom = Random.secure().nextInt(chars.length);
    return chars [indexRandom];
  }).join('');
}
```

当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-06-29-05-46-34.png)

### 代码

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:flutter_generate_strong_random/custom.dart';

class GeneratePassword extends StatefulWidget {
  @override
  _GeneratePasswordState createState() => _GeneratePasswordState();
}

class _GeneratePasswordState extends State<GeneratePassword> {

  final _controller = TextEditingController();

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) =>
      Scaffold(
        appBar: AppBar(
          automaticallyImplyLeading: false,
          backgroundColor: Colors.cyan,
          title: Text('Flutter Generate Random Password'),
        ),
        body: Container(
          padding: EdgeInsets.all(32),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              Row(
                children: [
                  Text("Generate Strong Random Password",style: TextStyle(
                      fontSize: 18, fontWeight: FontWeight.bold
                  ),),
                ],
              ),
              SizedBox(height: 15,),
              TextFormField(
                controller: _controller,
                readOnly: true,
                enableInteractiveSelection: false,
                decoration: InputDecoration(
                    focusedBorder: OutlineInputBorder(
                      borderSide: BorderSide(color: Colors.cyan,),
                    ),
                    enabledBorder: OutlineInputBorder(
                      borderSide: BorderSide(color: Colors.cyan),
                    ),
                    suffixIcon: IconButton(
                        onPressed: (){
                          final data = ClipboardData(text: _controller.text);
                          Clipboard.setData(data);

                          final snackbar = SnackBar(
                              content: Text("Password Copy"));

                          ScaffoldMessenger.of(context)
                            ..removeCurrentSnackBar()
                            ..showSnackBar(snackbar);
                        },
                        icon: Icon(Icons.copy))
                ),
              ),
              SizedBox(height: 15,),
              buildButtonWidget()
            ],
          ),

        ),
      );

  Widget buildButtonWidget() {
    return ElevatedButton(
        style: ElevatedButton.styleFrom(
            primary: Colors.black
        ),
        onPressed: (){
          final password = generatePassword();
          _controller.text = password;
        },
        child: Text("Password Generate",style: TextStyle(color: Colors.white),)
    );
  }

}
```

### 结语

在本文中，我解释了生成强随机密码的基本结构，您可以根据自己的选择修改这个代码。这是一个小的介绍生成强随机密码用户交互从我这边，它的工作使用 Flutter。

我希望这个博客将提供您尝试在您的 Flutter 项目生成强随机密码充足的信息。我们将向您展示生成随机密码是什么？.制作一个工作生成强随机密码的演示程序，它会显示当用户点击按钮，然后，密码将生成的长度，字符，数字，特殊，低字母和上游字母组合。它将生成的文本表单字段和用户也复制生成的密码在您的 Flutter 应用程序。所以请尝试一下。

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
