---
title: Dart语言学习 - 02 Windows 下安装 SDK
date: 2018-09-30 14:41:49
tags: dart
categories: Dart语言学习
---

# 本节目标

- 配置 Dart 开发环境
- 解决墙内问题

# 环境

- Windows 10
- Dart SDK 2.0.0

# 下载 SDK

## [SDK 列表](https://webdev.dartlang.org/tools/sdk/archive)

![archive](2018-09-30-14-54-15.png)

## 源下载 URL

```sh
https://storage.googleapis.com/dart-archive/channels/stable/release/2.0.0/sdk/dartsdk-windows-x64-release.zip
```

** 墙内请替换域名 `storage.flutter-io.cn` **

## 替换后 URL

```sh
https://storage.flutter-io.cn/dart-archive/channels/stable/release/2.0.0/sdk/dartsdk-windows-x64-release.zip
```

# 解压到磁盘

![](2018-09-30-15-24-14.png)

磁盘位置 `C:\sdk\dart`

# 设置环境变量

![](2018-09-30-15-01-44.png)

环境变量 `PATH` 新增 `C:\sdk\dart\bin`

# 测试

![](2018-09-30-15-08-44.png)

新开命令行窗口

```sh
dart --version
Dart VM version: 2.0.0 (Fri Aug 3 10:53:23 2018 +0200) on "windows_x64"
```

# VSCode IDE 配置

## [下载链接](https://code.visualstudio.com/)

## 安装 Dart 插件

# 编写 HelloWord

- 新建目录 `dart-learn`

- 编写文件 `hello.dart`

```dart
void main() {
  print('hello word!');
}
```

## 调试运行

配置文件 `launch.json`

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Dart",
      "program": "${file}",
      "request": "launch",
      "type": "dart"
    }
  ]
}
```

# 参考

- [Dart SDK Archive](https://webdev.dartlang.org/tools/sdk/archive)
- [Using-Flutter-in-China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
