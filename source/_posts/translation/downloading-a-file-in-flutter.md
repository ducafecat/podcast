---
title: Flutter 下载文件操作
tags: flutter
categories: 译文
date: 2021-11-03 00:00:00
---

![](2021-11-02-21-06-58.png)

## 原文

> https://medium.com/halkbank-mobile-tech/downloading-a-file-in-flutter-d6762825c0a4

## 代码

https://github.com/deremakif/FlowderSample

## 参考

- https://pub.dev/packages/path_provider
- https://pub.dev/packages/flutter_downloader
- https://pub.dev/packages/flowder
- https://pub.dev/packages/open_file
- https://pub.dev/packages/percent_indicator

## 正文

今天我要写一篇关于 [flowder package](https://pub.dev/packages/flowder) 的文章。我用它从服务器上下载文件。有很多方法可以做到这一点，而且还有更受欢迎的软件包如 [flutter_downloader](https://pub.dev/packages/flutter_downloader) 。但我更喜欢 flowder 软件包，因为它的实现很简单。

首先，如果下载文件夹不存在，我们应该创建它。要做到这一点，我们需要导入 [path_provider package](https://pub.dev/packages/path_provider)。并在当前页的 `initState()` 中调用 `initPlatformState` 方法。

```dart
Future<void> initPlatformState() async {
    _setPath();
    if (!mounted) return;
}void _setPath() async {
    Directory _path = await getApplicationDocumentsDirectory();
    String _localPath = _path.path + Platform.pathSeparator + 'Download';
    final savedDir = Directory(_localPath);
    bool hasExisted = await savedDir.exists();
    if (!hasExisted) {
        savedDir.create();
    }
    path = _localPath;
}
```

现在，我们有下载文件夹来保存文件。包的下载方法需要两个参数: URL 和选项。您可以根据需要自定义选项。

```dart
ElevatedButton(
    onPressed: () async {
      options = DownloaderUtils(
          progressCallback: (current, total) {
              final progress = (current / total) * 100;
              print('Downloading: $progress');
          },
          file: File('$path/loremipsum.pdf'),
          progress: ProgressImplementation(),
          onDone: () {
              OpenFile.open('$path/loremipsum.pdf');
          },
          deleteOnCancel: true,
      ); core = await Flowder.download(
             "https://assets.website-files.com/603d0d2db8ec32ba7d44fffe/603d0e327eb2748c8ab1053f_loremipsum.pdf",
             options,
           );
},
```

我使用 [OpenFile package](https://pub.dev/packages/open_file) 包在文件完成下载过程时打开它。我还使用了 [percent_indicator package](https://pub.dev/packages/percent_indicator) 包来显示进展。

如果以后不需要使用该文件，可以在关闭文档后删除该文件。重要的是不要增加应用程序的大小。

```dart
OpenFile.open('$path/loremipsum.pdf').then((value) {
    File f = File('$path/loremipsum.pdf');
    f.delete();
});
```

- 应用程序演示

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a7dabc79787f86906610a052af8f625e8006547735c5a80e207f651e6cd1874b.png)

## 示例项目的源代码。

- GitHub - deremakif/FlowderSample
  https://github.com/deremakif/FlowderSample

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
