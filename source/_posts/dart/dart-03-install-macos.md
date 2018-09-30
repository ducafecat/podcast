---
title: Dart语言学习 - 03 MacOS 下安装 SDK
top: false
tags: dart
categories: Dart语言学习
---

# 环境

- MacOS
- Dart SDK 2.0.0

# 下载 SDK

![](2018-09-30-16-07-22.png)

下载 URL

```sh
https://storage.googleapis.com/dart-archive/channels/stable/release/2.0.0/sdk/dartsdk-macos-x64-release.zip
```

** 墙内请替换域名 `storage.flutter-io.cn` **

替换后 URL

```sh
https://storage.flutter-io.cn/dart-archive/channels/stable/release/2.0.0/sdk/dartsdk-macos-x64-release.zip
```

# 解压到磁盘

![](2018-09-30-16-09-28.png)

磁盘位置 `~/Documents/sdk/dart`

# 设置环境变量

```sh
# 打开配置文件
vim ~/.bash_profile

# 尾部加入配置
export PATH=~/Documents/sdk/dart/bin:$PATH

# 重载配置文件
source ~/.bash_profile
```

# 测试

![](2018-09-30-16-23-57.png)

新开命令行窗口

```sh
dart --version
Dart VM version: 2.0.0 (Fri Aug 3 10:53:23 2018 +0200) on "macos_x64"
```

# 参考

- [Dart SDK Archive](https://webdev.dartlang.org/tools/sdk/archive)
- [Using-Flutter-in-China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)

----

© 猫哥
微信 ducafecat
[ducafecat.tech](https://ducafecat.tech/)
