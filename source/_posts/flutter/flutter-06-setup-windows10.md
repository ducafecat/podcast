---
title: Flutter 零基础入门中文教学 - 06 Windows10 下配置 Flutter 开发环境
date: 2019-08-11 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- 安装 JDK 1.8
- 安装 Flutter SDK
- 安装 Android Studio
- 安装 VSCode
- 配置 VSCode 插件
- 配置 Android 插件
- 配置 Android 模拟器

## 环境介绍

- window10 专业版
- jdk1.8
- flutter 1.7.8
- vscode 1.37.1
- android studio 3.5

## 1. 安装 JDK 1.8

- 下载地址

https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

- 选取 windows x64

![](2019-08-26-09-31-32.png)

## 2. 安装 Flutter SDK

- 下载地址

https://flutter.dev/docs/development/tools/sdk/releases?tab=windows#windows

- 解压

我放在了 c:\sdk\flutter

- 配置环境变量

```sh
# Path
C:\sdk\flutter\bin

# FLUTTER_STORAGE_BASE_URL
https://storage.flutter-io.cn

# PUB_HOSTED_URL
https://pub.flutter-io.cn
```

- 执行检查

```sh
Flutter doctor
```

## 3. 安装 Android Studio

- 下载

https://developer.android.com/studio/

![](2019-08-26-10-03-40.png)

- 配置 SDK 包

![](2019-08-26-09-50-23.png)

- 配置 SDK Tools

![](2019-08-26-09-51-19.png)

- 配置环境变量

```sh
# Path
C:\Users\ducaf\AppData\Local\Android\Sdk\tools
C:\Users\ducaf\AppData\Local\Android\Sdk\platform-tools

# ANDROID_HOME
C:\Users\ducaf\AppData\Local\Android\Sdk
```

- 安装 Android 证书

```sh
flutter doctor --android-licenses

一路按 Y
```

## 4. 安装 VSCode

- 下载地址

https://code.visualstudio.com/

## 5. 配置 VSCode 插件

- Flutter 必装

![](2019-08-26-09-55-56.png)

- Awesome Flutter Snippets

![](2019-08-26-09-56-41.png)

- Paste JSON as Code

![](2019-08-26-09-57-36.png)

- bloc

![](2019-08-26-09-58-58.png)

## 6. 配置 Android 插件

- flutter

![](2019-08-26-10-00-53.png)

## 7. 配置 Android 模拟器

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
