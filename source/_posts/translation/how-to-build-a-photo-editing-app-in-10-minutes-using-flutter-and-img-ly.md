---
title: Flutter 如何在10分钟内快速的创建图片编辑器
tags: flutter
categories: 译文
date: 2021-09-29 00:00:00
---

## 猫哥说

我最近在做一个社交 APP，里面需要图片、视频的编辑器，如果你和我一样有这样的需求

你可以试试这款 https://img.ly

## 原文

https://promise-amadi.medium.com/how-to-build-a-photo-editing-app-in-10-minutes-using-flutter-and-img-ly-5d9601173822

## 源码

https://github.com/Wizpna/photo_editor.git

## 参考

- https://img.ly/photo-sdk
- https://pub.dev/packages/photo_editor_sdk
- https://pub.dev/packages/image_picker

## 正文

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bc6eb61a67ada8af5036f74db19400c341317221d60e0e09abdce7329a87d53f.png)

大家好，在今天的文章中，你将学习如何使用 Flutter 和 Img.ly 构建一个照片编辑应用程序。

但是，在我深入本教程的技术方面之前，我想对 IMG.LY 做一个简单的解释。

IMG.LY 是一家总部位于德国的公司，通过其图片和视频编辑 SDK 提供最先进的图像和视频处理解决方案。

主要用于照片编辑目的，而且 SDK 很容易在移动应用程序上实现。

### 那么让我们开始吧

使用 Visual Studio、 IntelliJ 或 Android Studio 创建一个新的 Flutter 项目。

成功创建一个新的 flutter 项目后，打开“ pubspec.yaml”，并安装 [photo_editor_sdk](https://pub.dev/packages/photo_editor_sdk) 和 [image_picker](https://pub.dev/packages/image_picker) 插件。

```dart
dependencies:
  photo_editor_sdk: ^2.0.0
  image_picker: ^0.8.1+3
```

注意: `image_picker` 这个插件将用于从设备中获取照片，而 `photo_editor_sdk` 将用于照片编辑。

### 为 Android 配置 PhotoEditor SDK

SDK 相当大，因此你需要为你的项目启用 Multidex 如下:

1. 编辑 `android/build.gradle` 并在顶部添加以下行

```dart
buildscript {
    repositories {
        google()
        jcenter()
        maven { url "https://artifactory.img.ly/artifactory/imgly" }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.1.0' classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version" classpath 'ly.img.android.sdk:plugin:8.3.1'
    }
}
```

> 请注意: 为了更新 Android 版本的 PhotoEditor SDK，将版本字符串 version 8.3.1 替换为更新的版本 [newer release](https://github.com/imgly/pesdk-android-demo/releases) 。

2. 打开 `**android/app/build.gradle** file` (not `android/build.gradle`) 并在末尾添加以下代码行:

```dart
android {
  defaultConfig {
      applicationId "com.example.photo_editor" minSdkVersion 16
      targetSdkVersion 30
      versionCode flutterVersionCode.toInteger()
      versionName flutterVersionName
      multiDexEnabled true
  }
}
dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'androidx.multidex:multidex:2.0.1'
}
```

3. 打开 `android/app/build.gradle` file (not `android/build.gradle`) 在 apply plugin 下面添加以下行 `apply plugin: "com.android.application"`:

```dart
apply plugin: 'ly.img.android.sdk'
apply plugin: 'kotlin-android'

apply plugin: 'ly.img.android.sdk'apply from: "$flutterRoot/packages/flutter_tools/gradle/flutter.gradle"// Comment out the modules you don't need, to save size.
imglyConfig {
    modules {
        include 'ui:text'
        include 'ui:focus'
        include 'ui:frame'
        include 'ui:brush'
        include 'ui:filter'
        include 'ui:sticker'
        include 'ui:overlay'
        include 'ui:transform'
        include 'ui:adjustment'
        include 'ui:text-design'// This module is big, remove the serializer if you don't need that feature.
        include 'backend:serializer'// Remove the asset packs you don't need, these are also big in size.
        include 'assets:font-basic'
        include 'assets:frame-basic'
        include 'assets:filter-basic'
        include 'assets:overlay-basic'
        include 'assets:sticker-shapes'
        include 'assets:sticker-emoticons'include 'backend:sticker-smart'
    }
}
```

### 为 iOS 设置 ImagePicker

打开 `<project root>/ios/Runner/Info.plist` 并将以下键添加到 **Info.plist** 文件中

```dart
<key>NSPhotoLibraryUsageDescription</key>
           <string>app needs permission for the photo library</string>
           <key>NSCameraUsageDescription</key>
           <string>app needs access to the camera.</string>
           <key>NSMicrophoneUsageDescription</key>
           <string>app needs access to the microphone, if you intend to record videos.</string>
```

打开你的 **main.dart** 文件，像下面的代码片段一样更新你的代码:

您必须创建一个名为 **imgFromGallery** 的方法

> 当调用 **imgFromGallery** 方法时，它将打开设备上的图像目录。

下一步将创建另一个名为 **imglyEditor.** 的方法。

> 当调用 **imglyEditor** 方法时，它将打开 Img.ly 编辑器。

使用物理设备或模拟器测试运行应用程序。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/3b2f00c0eb5e1790c9e8cd5033a0c9a258a9668088a0989c3c5568598ddb2484.png)

P.S: 这是源码 [**source code**](https://github.com/Wizpna/photo_editor.git)

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
