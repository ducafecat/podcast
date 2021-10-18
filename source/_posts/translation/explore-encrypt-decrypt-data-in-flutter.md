---
title: Flutter 的加密和解密数据
tags: flutter
categories: 译文
date: 2021-10-18 00:00:00
---

![](2021-10-18-09-03-02.png)

## 原文

> https://medium.com/flutterdevs/explore-encrypt-decrypt-data-in-flutter-576425347439

## 参考

- https://pub.flutter-io.cn/packages/encrypt

## 正文

了解如何加密和解密数据在您的 Flutter 应用程序

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c68cf5a5fe6f5f211ebeb7dc336451a98eef5d2b33c53dbe934bf3170f1771b2.png)

Flutter 是一个可移植的 UI 工具包。换句话说，它是一个全面的应用软件开发工具包(SDK) ，包括小部件和工具。Flutter 是一个免费的开源工具，用于开发移动、桌面和 web 应用程序。 Flutter 是一种跨平台的开发工具。这意味着用同样的代码，我们可以同时创建 IOs 和 Android 应用程序。这是在整个过程中节省时间和资源的最佳方式。在这方面，hot reload 正在获得移动开发者的支持。允许我们通过热重装快速查看在代码中实现的更改。

在本文中，我们将探讨使用加密包 Flutter 加密和解密数据文件。借助于这个软件包，用户可以轻松地加密和解密数据。那么让我们开始吧。

加密是将数据转换为编码(密码)数据形式的过程。如果任何未经授权的个人或实体获得访问权限，他们将无法读取。在发送文件之前，有必要将文件保护到每个移动应用程序或网站上，并在网络上传递数据，以防止未经授权的接收者访问其数据。它有助于保护私有和敏感信息，并可以提高客户端应用程序和服务器之间通信的安全性。

解密是将已编码的数据从后台转换为正常(普通)数据形式的过程。在这篇文章中，我们将展示如何加密输入数据，然后将其解密回正常形式。

> 演示模块:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/76068d37e710ebba517464df37ec8ccb962f9911c1c220eb6b7114a954ce1cac.gif)

### 加密数据类型:

> 我们将看到 3 种不同类型的算法加密和解密数据的 Flutter 。

**1- AES 算法 :**

(高级加密标准)已经成为世界各地政府、金融机构和安全意识强的企业的首选加密算法。美国国家安全局(NSC)使用它来保护国家的“最高机密”信息。

**2- Fernet 算法:**

Fernet 是一种非对称加密方法，它可以确保加密的消息在没有密钥的情况下不能被操纵/读取。它对密钥使用 url 安全编码。Fernet 还在 CBC 模式和 PKCS7 填充中使用 128 位 AES，HMAC 使用 SHA256 进行身份验证。这个 IV 是由 os 创建的。随机()。所有这些都是好的软件所需要的。

**3- Salsa 算法 :**

Salsa20 是提交给 eSTREAM 项目的一个密码，运行时间从 2004 年到 2008 年，该项目旨在促进流密码的发展。该算法被认为是一种设计良好的高效算法。目前还没有任何针对 Salsa20 密码家族的已知有效攻击。

### 代码步骤:

让我们从代码实现开始。要实现以下项目，你需要集成到您的 Flutter 应用程序的加密包。

**第一步: 添加依赖项**

> 将依赖项添加到 pubspec ー yaml 文件。

- > encrypt: ^ 5.0.1

现在在你的 Dart 代码中，你可以使用以下导入这个包。

```dart
import 'package:crypto/crypto.dart';
```

**步骤 2: 我创建了一个 dart 文件来定义 AES、 Fernet 和 Salsa 算法。**

这个文件有两种方法，用 AES 算法加密和解密数据。

```dart
class EncryptData{
_//for AES Algorithms_ static Encrypted? _encrypted_;
  static var _decrypted_;


 static _encryptAES_(plainText){
   final key = Key.fromUtf8('my 32 length key................');
   final iv = IV.fromLength(16);
   final encrypter = Encrypter(AES(key));
    _encrypted_ = encrypter.encrypt(plainText, iv: iv);
   print(_encrypted_!.base64);
 }

  static _decryptAES_(plainText){
    final key = Key.fromUtf8('my 32 length key................');
    final iv = IV.fromLength(16);
    final encrypter = Encrypter(AES(key));
    _decrypted_ = encrypter.decrypt(_encrypted_!, iv: iv);
    print(_decrypted_);
  }
}
```

这个文件有 2 个方法加密和解密数据使用萨尔萨算法。

```dart
_//for Fernet Algorithms
_static Encrypted? _fernetEncrypted_;
static var _fernetDecrypted_; static _encryptFernet_(plainText){
    final key = Key.fromUtf8('my32lengthsupersecretnooneknows1');
    final iv = IV.fromLength(16);
    final b64key = Key.fromUtf8(base64Url.encode(key.bytes));
    final fernet = Fernet(b64key);
    final encrypter = Encrypter(fernet);
    _fernetEncrypted_ = encrypter.encrypt(plainText);
    print(_fernetEncrypted_!.base64); _// random cipher text_ print(fernet.extractTimestamp(_fernetEncrypted_!.bytes));
  }

  static _decryptFernet_(plainText){
    final key = Key.fromUtf8('my32lengthsupersecretnooneknows1');
    final iv = IV.fromLength(16);
    final b64key = Key.fromUtf8(base64Url.encode(key.bytes));
    final fernet = Fernet(b64key);
    final encrypter = Encrypter(fernet);
    _fernetDecrypted_ = encrypter.decrypt(_fernetEncrypted_!);
    print(_fernetDecrypted_);
  }
```

**步骤 3: 现在终于在主屏幕 Dart 文件中调用上面的方法。**

在这个文件中，我已经设计了一个卡，1 个文本字段和 2 个按钮，2 个文本视图显示加密和解密的结果。下面是截图和代码片段。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/6ab3e53509f6db305b01f8890d1351b77a3ff4be1e29a91fb9647336b1d6d625.png)

### 全部代码:

```dart
import 'package:encrypt_data_demo/encrypt_data.dart';
import 'package:flutter/material.dart';



class HomeView extends StatefulWidget {

  @override
  _HomeViewState createState() => _HomeViewState();
}

class _HomeViewState extends State<HomeView> {
  TextEditingController? _controller=TextEditingController();
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors._orange_,
      appBar: AppBar(
        title: Text("Encrypt and Decrypt Data"),
      ),
      body: SingleChildScrollView(
        child: Container(
          margin: EdgeInsets.only(top:10,bottom: 10),
          child: _buildBody(),
        ),
      ),
    );
  }

 Widget _buildBody() {
    return Container(
      height: 280,
      width: MediaQuery._of_(context).size.width,
      margin: EdgeInsets.only(left: 10,right: 10),
      child: Card(
         elevation: 2,
        child:  Container(
          padding: EdgeInsets.only(left: 15,right: 15,top:
15,bottom: 15),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Container(
                child: TextField(
                  controller: _controller,
                  decoration: InputDecoration(
                    border: OutlineInputBorder(),
                    hintText: 'Enter Text',
                  ),
                ),
              ),
              SizedBox(height: 30),
              Text("EncryptText : ${EncryptData._aesEncrypted_!=null?EncryptData._aesEncrypted_?.base64:''}",
                maxLines: 2,
                style:TextStyle(
                    color: Colors._black_,
                    fontSize: 16
                ),
                overflow: TextOverflow.ellipsis,
              ),
              SizedBox(height: 10,),
              Expanded(
                child: Text("DecryptText : ${EncryptData._aesDecrypted_!=null?EncryptData._aesDecrypted_:''}",
                    maxLines: 2,
                    overflow: TextOverflow.ellipsis,
                    style:TextStyle(
                        color: Colors._black_,
                        fontSize: 16
                    )
                ),
              ),
              SizedBox(height: 20,),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  ElevatedButton(
                    style: ElevatedButton._styleFrom_(
                      primary: Colors._blue_, _// background_ onPrimary: Colors._white_,
                    ),
                    onPressed: () {
                      setState(() {
                        EncryptData._encryptAES_(_controller!.text);
                      });
                    },
                    child: Text('Encryption'),
                  ),
                  ElevatedButton(
                    style: ElevatedButton._styleFrom_(
                      primary: Colors._blue_, _// background_ onPrimary: Colors._white_,
                    ),
                    onPressed: () {
                      setState(() {
                        EncryptData._decryptAES_(_controller!.text);
                      });
                    },
                    child: Text('Decryption'),
                  )
                ],
              )
            ],
          ),
        ),
      ),
    );
 }
}
```

### 结束语:

在本文中，我解释了 Flutter 加密数据的基本概况，您可以根据自己的选择修改这段代码。这是一个小的介绍加密和解密数据的用户交互从我这边，它的工作使用 Flutter 。

我希望这个博客将提供您尝试在您的 Flutter 项目的探索，加密和解密数据充足的信息。

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
