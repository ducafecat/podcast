---
title: Flutter 创建多图像的 PDF 文件
tags: flutter
categories: 译文
date: 2021-06-18 00:00:00
---

![](2021-06-18-09-17-32.png)

## 猫哥说

保持热情去改变!

今天这篇文章是让你在客户端完成 PDF 的创建，这样能减轻服务器的压力。

这是很有必要的，服务器的 CPU 资源很宝贵。

Flutter 插件 https://pub.dev/packages/pdf

- 功能有：
  - 载入图片
  - 写上文字
  - 加密、签名文件
  - 也可以载入 pdf

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutterdevs/flutter-create-pdf-files-with-multiple-images-4458e813fe37

## 代码

https://github.com/flutter-devs/flutter_pdf_create_view_demo

## 参考

- https://pub.dev/packages/pdf
- https://pub.dev/packages/path_provider/versions/2.0.1
- https://pub.dev/packages/syncfusion_flutter_pdfviewer/versions/19.1.64-beta

## 正文

![](2021-06-18-08-51-35.png)

在 Flutter 不同的功能使您的应用程序丰富的有用性，并给简单的客户端做东西内的应用程序和改善客户端的经验，是一个专家合作是另外必不可少的开发人员。

有很多软件包可以用来在应用程序中打开 pdf，有些比较复杂，有些并不难执行，在这里我将阐明可能最容易使用的方法。

在这个博客中，我们将探索 Flutter ー创建多张图片的 PDF 文件。我们将实施一个演示程序，以显示如何 Flutter 创建一个 pdf 文件与多个图像使用的三个要素包在您的 Flutter 应用程序。

### 状态管理

#### 简介:

PDF 很可能是用于交换业务信息的最著名的文档格式，因为内容不能像不同配置那样有效地更改。这样可以保护我们的信息不受未经批准的更改的影响。一旦你知道了策略，这通常是一个简单的互动，我会告诉你在你的任务中制作 pdf 文档的最好方法。

> 对于这个演示，需要三个基本的软件包。

- https://pub.dev/packages/pdf
- https://pub.dev/packages/path_provider/versions/2.0.1
- https://pub.dev/packages/syncfusion_flutter_pdfviewer/versions/19.1.64-beta

#### 演示模块:

![](2021-06-18-08-56-12.png)

这个演示视频显示了如何在一个 Flutter 与多个图像创建 pdf 文件。它显示了 pdf 文件将如何使用这三个软件包在您的 Flutter 应用程序。它显示当用户点击一个创建按钮，然后出现 pdf，根据页面有多个图像。它会显示在你的设备上。

#### 实施方案:

- 第一步: 添加依赖项

将依赖项添加到 pubspec ー yaml 文件。

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  path_provider: ^2.0.1
  pdf: ^3.3.0
  syncfusion_flutter_pdfviewer: ^19.1.64-beta
```

- 第二步: 添加 assets

将 assets 添加到 pubspec ー yaml 文件。

```yaml
assets:
  - assets/images/
```

- 第三步: 导入

```yaml
import 'package:pdf/pdf.dart';
import 'package:path_provider/path_provider.dart';
import 'package:syncfusion_flutter_pdfviewer/pdfviewer.dart';
```

- 第四步: 在应用程序的根目录中运行 flutter 软件包

#### 如何实现 dart 文件中的代码:

你需要分别在你的代码中实现它:

> 在 lib 文件夹中创建一个名为 pdf _ screen _ demo. dart 的新 dart 文件。

- 首先，让我们创建一个基本的 PDF 文件:

在文件中创建一个 StatefulWidget，名为 PdfScreenDemo。

```dart
String pdfFile = '';
```

一个基本的用户界面，我们有一个凸起的按钮，使我们的 PDF 文件和一个可见性小部件揭示 PDF 浏览器一旦 PDF 记录。要查看 PDF 记录，我们将使用 syncfusion/flutter/pdfviewer 包的 SfPdfViewer.file 小部件，在这个小部件中，我们将制作一个文档，其方式与我们制作的 PDF 类似。

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: [
      Visibility(
          visible: pdfFile.isNotEmpty,
          child: SfPdfViewer.file(File(pdfFile),
              canShowScrollHead: false, canShowScrollStatus:
           false),
        ),
    RaisedButton(
        color: Colors.tealAccent,
        onPressed: () {
        },
        child: Text('Create a Pdf File')),
  ],
),
```

PDF 包有它自己的小部件库存，为了调用这些库存，我们需要导入 PDF 小部件作为一个变量名为 pw。

```dart
import 'package:pdf/widgets.dart' as pw;
```

> 为了构造一个 PDF 格式，我们将通过调用 pw < widgetnme > 来调用小部件。

为了保存一个 pdf 文件，我们应该做一个。文件()。这个不常见的小部件将保存已生成 PDF 的数据，因此我们在 \_ PdfScreenDemoState 中生成一个变量如何。

```dart
var pdf = pw.Document();
```

> Lest’s create createPdfFile() method:

在这个模型中，我们的想法是制作一个多页的带有 a4 设计的 PDF。因为我们要添加的图片的数量，页面的数量可能是多个的，按照这些顺序，现在制作一个多页面小部件是个好兆头。

在 MultiPage 小部件的构建方法内部，将有一个带有 Header 的 Column 和 Divider，用于将实质内容与 Header 分离。在接下来的几个阶段中，我们将在本栏内的分隔符下添加实质内容。

```dart
createPdfFile() {
  pdf.addPage(pw.MultiPage(
      margin: pw.EdgeInsets.all(10),
      pageFormat: PdfPageFormat.a4,
      build: (pw.Context context) {
        return <pw.Widget>[
          pw.Column(
              crossAxisAlignment: pw.CrossAxisAlignment.center,
              mainAxisSize: pw.MainAxisSize.min,
              children: [
                pw.Text('Create a basic PDF',
                    textAlign: pw.TextAlign.center,
                    style: pw.TextStyle(fontSize: 26)),
                pw.Divider(),
              ]),
        ];
      }));
}
```

> 此后，我们应该创建一个 savePdfFile ()方法。

它是一种异步方法，等待 IOS 或 Android 平台的目录。然后，该技术使用 ID 的名称和目录的 documentPath 创建一个记录。你可以用任何你喜欢的方式来命名你的记录，但是对于不同的 PDF 创建来说，更聪明的做法是为每个 PDF 文档都起一个特殊的名字。

最后，该方法将 PDF 保存为文件。Path 和 value 被赋给 setState 策略中的 pdfFile 变量，以便稍后刷新 UI。

```dart
savePdfFile() async {
Directory documentDirectory = await getApplicationDocumentsDirectory();

String documentPath = documentDirectory.path;

String id = DateTime.now().toString();

File file = File("$documentPath/$id.pdf");

file.writeAsBytesSync(await pdf.save());
setState(() {
pdfFile = file.path;
pdf = pw.Document();
});
}
```

> 重要提示: 任命民主党人是根本。在保存 pdf 文件之后，将 Document ()转换为 pdf 变量。如果不这样做，该 pdf 将尝试使另一个最近制作的 pdf 文件，这将导致一个有缺陷的 pdf 文件被制作。

如果您需要保存您的 PDF 文件的字节，您可以利用下方的方法。

```dart
List<int> pdfBytes;
pdfBytes = await file.readAsBytes();
pdfFile base64Encode(pdfBytes);
```

> 除此之外，使用异步 onPressed 方法:

为了保存记录，我们需要首先等待 createPdfFile ()策略。

```dart
onPressed: () async {
  await createPdfFile();
  savePdfFile();
},
```

好了，现在一切都安排好了。你可以点击“创建一个 PDF 文件”按钮来查看你的第一个基本 PDF 文档。当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-06-18-09-02-23.png)

- 让我们用一个图片创建一个 pdf 文件:

在 pdf 包中，你可以添加一个图片。MemoryImage.因此，我们需要将图片更改为内存字节。

> 首先导入下面的软件包。

```dart
import 'dart:typed_data';
import 'package:flutter/services.dart';
```

从那时起，将 createdffile 转换为一个 async 方法，并添加这两个变量。

```dart
final ByteData bytes = await rootBundle.load('assets/images/null_safety.png');
final Uint8List byteList = bytes.buffer.asUint8List();
```

第一个变量在资源图片上更改为字节，第二个变量在字节上更改为 Uint8List 字节列表。

然后，在这一点上添加一个 pw 图像小部件在分隔符下面。

```dart
createPdfFile() async {
  final ByteData bytes =
      await rootBundle.load('assets/images/null_safety.png');
  final Uint8List byteList = bytes.buffer.asUint8List();
  pdf.addPage(pw.MultiPage(
      margin: pw.EdgeInsets.all(10),
      pageFormat: PdfPageFormat.a4,
      build: (pw.Context context) {
        return <pw.Widget>[
          pw.Column(
              crossAxisAlignment: pw.CrossAxisAlignment.center,
              mainAxisSize: pw.MainAxisSize.min,
              children: [
                pw.Text('Flutter Pdf File with Image',
                    textAlign: pw.TextAlign.center,
                    style: pw.TextStyle(fontSize: 26)),

                pw.Divider(),
              ]),
          pw.SizedBox(height: 70),
          pw.Image(
              pw.MemoryImage(
                byteList,
              ),
              height: 300,
              fit: pw.BoxFit.fitHeight),
        ];
      }));
}
```

MemoryImage 接受 byteList 作为位置竞争，将图片传送到 pdf 记录中。现在重新启动应用程序，并尝试制作另一个 PDF 文档，以查看其中包含图片的文件。当我们运行应用程序时，我们应该获得屏幕输出，就像下面的屏幕截图一样。

![](2021-06-18-09-03-57.png)

- 让我们创建一个有多个图片的 pdf 文件:

在演示的最后一步，我们将制作一个 pdf 构建技术，它可以用给定的各种图像生成一条记录。首先，我们需要制定一个策略，将图片转换为 Uint8List 设计。

> 在状态小部件中，创建一个空白列表，用于将图片放入 Uint8list。

```dart
List<Uint8List> imagesUint8list = [];
```

> 然后，我们应该重构 createdffile 方法，并集中于另一种获取图片字节的技术。

```dart
getImageBytes(String assetImage) async {
    final ByteData bytes = await rootBundle.load(assetImage);
    final Uint8List byteList = bytes.buffer.asUint8List();
    imagesUint8list.add(byteList);
  }
```

现在我们将制作一个类型为 pw 的列表。createPdfFile ()技术中的小部件，它生成具有图像标题和图像本身的 Column。

```dart
final List<pw.Widget> pdfImages = imagesUint8list.map((image) {
      return pw.Padding(
          padding: pw.EdgeInsets.symmetric(vertical: 20, horizontal:    10),
          child: pw.Column(
              crossAxisAlignment: pw.CrossAxisAlignment.center,
              mainAxisSize: pw.MainAxisSize.max,
              children: [
                pw.Text(
                    'Image'
                            ' ' +
                        (imagesUint8list
                                    .indexWhere((element) => element
                                   == image) +
                                1)
                            .toString(),
                    style: pw.TextStyle(fontSize: 22)),
                pw.SizedBox(height: 10),
                pw.Image(
                    pw.MemoryImage(
                      image,
                    ),
                    height: 400,
                    fit: pw.BoxFit.fitHeight)
              ]));
    }).toList();
```

> 注意: 这是紧急给一个特定的最大大小的图片小部件，否则一个图片可能会溢出页面设计促使一个失败的 pdf 创建。

Createdffile 最初会创建一个 for 循环，将多个图片更改为 Uint8List。稍后，它将制作一个带有头部的图片列表。最后，pdfImages 将作为子文件显示在 pdf 的基本列中。

```dart
createPdfFile() async {
    for (String image in assetImages) await getImageBytes(image);
    final List<pw.Widget> images = imagesUint8list.map((image) {
      return pw.Padding(
          padding: pw.EdgeInsets.symmetric(vertical: 20, horizontal:  10),
          child: pw.Column(
              crossAxisAlignment: pw.CrossAxisAlignment.center,
              mainAxisSize: pw.MainAxisSize.max,
              children: [
                pw.Text(
                    'Image'
                            ' ' +
                        (imagesUint8list
                                   .indexWhere((element) => element
                             == image) +
                                1)
                            .toString(),
                    style: pw.TextStyle(fontSize: 22)),
                pw.SizedBox(height: 10),
                pw.Image(
                    pw.MemoryImage(
                      image,
                    ),
                    height: 400,
                    fit: pw.BoxFit.fitHeight)
              ]));
    }).toList();
    pdf.addPage(pw.MultiPage(
        margin: pw.EdgeInsets.all(10),
        pageFormat: PdfPageFormat.a4,
        build: (pw.Context context) {
          return <pw.Widget>[
            pw.Column(
                crossAxisAlignment: pw.CrossAxisAlignment.center,
                mainAxisSize: pw.MainAxisSize.min,
                children: [
                  pw.Text('Create a Simple PDF',
                      textAlign: pw.TextAlign.center,
                      style: pw.TextStyle(fontSize: 26)),
                  pw.Divider(),
                ]),
            pw.Column(
                crossAxisAlignment: pw.CrossAxisAlignment.center,
                mainAxisSize: pw.MainAxisSize.max,
                children: pdfImages),
          ];
        }));
  }
```

> 另一个步骤是用 SingleChildScrollView 和 Expanded 小部件包装可见性小部件。

```dart
Expanded(
              child: SingleChildScrollView(
                child: Visibility(
                  visible: pdfFile.isNotEmpty,
                  child: SfPdfViewer.file(File(pdfFile),
                      canShowScrollHead: false, canShowScrollStatus: false),
                ),
              ),
            ),
```

现在如何我们按下按钮最后一次机会，使 PDF 文件与多个图片。当我们运行应用程序时，我们应该得到屏幕的输出，就像下面的屏幕截图一样。

![](2021-06-18-09-06-05.png)

#### 代码:

```dart
import 'dart:io';
import 'dart:typed_data';
import 'package:flutter/services.dart';
import 'package:path_provider/path_provider.dart';
import 'package:pdf/pdf.dart';
import 'package:pdf/widgets.dart' as pw;
import 'package:flutter/material.dart';
import 'package:syncfusion_flutter_pdfviewer/pdfviewer.dart';

class PdfScreenDemo extends StatefulWidget {
  @override
  _PdfScreenDemoState createState() => _PdfScreenDemoState();
}

class _PdfScreenDemoState extends State<PdfScreenDemo> {
  String pdfFile = '';
  var pdf = pw.Document();

  static const List<String> assetImages = [
    'assets/images/null_safety.png',
    'assets/images/stream.png',
    'assets/images/error_handling.jpg'
  ];
  List<Uint8List> imagesUint8list = [];

  @override
  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: Scaffold(
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Expanded(
                child: SingleChildScrollView(
                  child: Visibility(
                    visible: pdfFile.isNotEmpty,
                    child: SfPdfViewer.file(File(pdfFile),
                        canShowScrollHead: false, canShowScrollStatus: false),
                  ),
                ),
              ),
              RaisedButton(
                  color: Colors.tealAccent,
                  onPressed: () async {
                    await createPdfFile();
                    savePdfFile();
                  },
                  child: Text('Create a Pdf File')),
            ],
          ),
        ),
      ),
    );
  }

  getImageBytes(String assetImage) async {
    final ByteData bytes = await rootBundle.load(assetImage);
    final Uint8List byteList = bytes.buffer.asUint8List();
    imagesUint8list.add(byteList);
    print(imagesUint8list.length);
  }

  createPdfFile() async {
    for (String image in assetImages) await getImageBytes(image);
    final List<pw.Widget> pdfImages = imagesUint8list.map((image) {
      return pw.Padding(
          padding: pw.EdgeInsets.symmetric(vertical: 50, horizontal: 10),
          child: pw.Column(
              crossAxisAlignment: pw.CrossAxisAlignment.center,
              mainAxisSize: pw.MainAxisSize.max,
              children: [
                pw.Text(
                    'Image'
                            ' ' +
                        (imagesUint8list
                                    .indexWhere((element) => element == image) +
                                1)
                            .toString(),
                    style: pw.TextStyle(fontSize: 22)),
                pw.SizedBox(height: 30),
                pw.Image(
                    pw.MemoryImage(
                      image,
                    ),
                    height: 300,
                    fit: pw.BoxFit.fitHeight)
              ]));
    }).toList();

    pdf.addPage(pw.MultiPage(
        margin: pw.EdgeInsets.all(10),
        pageFormat: PdfPageFormat.a4,
        build: (pw.Context context) {
          return <pw.Widget>[
            pw.Column(
                crossAxisAlignment: pw.CrossAxisAlignment.center,
                mainAxisSize: pw.MainAxisSize.min,
                children: [
                  pw.Text('Flutter Pdf File with Multiple Image',
                      textAlign: pw.TextAlign.center,
                      style: pw.TextStyle(fontSize: 26)),
                  pw.Divider(),
                ]),
            pw.Column(
                crossAxisAlignment: pw.CrossAxisAlignment.center,
                mainAxisSize: pw.MainAxisSize.max,
                children: pdfImages),
          ];
        }));
  }

  savePdfFile() async {
    Directory documentDirectory = await getApplicationDocumentsDirectory();

    String documentPath = documentDirectory.path;

    String id = DateTime.now().toString();

    File file = File("$documentPath/$id.pdf");

    file.writeAsBytesSync(await pdf.save());
    setState(() {
      pdfFile = file.path;
      pdf = pw.Document();
    });
  }
}
```

#### Conclusion

在这篇文章中，我解释了创建 PDF 文件的基本结构与多重图像 Flutter; 您可以修改这个代码根据您的选择。这是一个小的介绍创建 PDF 文件与多图像用户交互从我这边，它的工作使用 Flutter。

我希望这个博客将提供您在尝试创建 PDF 文件与多个图像在您的扑项目充分的信息。我们将向您展示介绍是什么？.制作一个演示程序为工作创建 PDF 文件与多个图像和显示当用户点击一个创建按钮，然后 PDF 发生，与多个图像根据页面使用所有三个软件包在您的 Flutter 应用程序。所以请尝试一下。

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
