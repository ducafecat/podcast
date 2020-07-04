---
title: Flutter 实战从零开始 新闻客户端 - 11 APP升级、android动态授权
date: 2020-05-16 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

# 本节目标

- app 升级策略
- android 动态授权
- android 设备目录
- ios 支持 swift 语言
- 快速提示框

## 视频

<iframe src="//player.bilibili.com/player.html?bvid=BV1Gi4y147zG&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

<iframe src="//player.bilibili.com/player.html?bvid=BV17t4y117ua&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

## 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.11

## 正文

### ios 支持 swift 语言

- 出发点

社区第三方包都在用 swift 开发，打包的时候需要加入 swift 语言包。

- 操作

创建一个支持 swift 的新项目，然后把 lib assets pubspec.yaml 覆盖即可。

### app 升级策略

![](2020-05-16-15-18-48.png)

### 代码实现

#### 定义接口

- post /app/update

![](2020-05-16-16-02-33.png)

#### 加入依赖包

- pubspec.yaml

```yaml
dependencies:
  # 设备信息
  device_info: ^0.4.2+3

  # 包信息
  package_info: ^0.4.0+18

  # 路径查询
  path_provider: ^1.6.8

  # permission 权限
  permission_handler: ^5.0.0+hotfix.6

  # 安装
  install_plugin: ^2.0.1

  # 对话框
  easy_dialog: ^1.0.5
```

#### 升级工具类

- lib/common/utils/update.dart

```dart
import 'dart:io';

import 'package:dio/dio.dart';
import 'package:easy_dialog/easy_dialog.dart';
import 'package:flutter/material.dart';
import 'package:flutter_ducafecat_news/common/apis/app.dart';
import 'package:flutter_ducafecat_news/common/entitys/entitys.dart';
import 'package:flutter_ducafecat_news/common/widgets/toast.dart';
import 'package:flutter_ducafecat_news/global.dart';
import 'package:install_plugin/install_plugin.dart';
import 'package:path_provider/path_provider.dart';

/// app 升级
class AppUpdateUtil {
  static AppUpdateUtil _instance = AppUpdateUtil._internal();
  factory AppUpdateUtil() => _instance;

  BuildContext _context;
  AppUpdateResponseEntity _appUpdateInfo;

  AppUpdateUtil._internal();

  /// 获取更新信息
  Future run(BuildContext context) async {
    _context = context;

    // 提交 设备类型、发行渠道、架构、机型
    AppUpdateRequestEntity requestDeviceInfo = AppUpdateRequestEntity(
      device: Global.isIOS == true ? "ios" : "android",
      channel: Global.channel,
      architecture: Global.isIOS == true
          ? Global.iosDeviceInfo.utsname.machine
          : Global.androidDeviceInfo.device,
      model: Global.isIOS == true
          ? Global.iosDeviceInfo.name
          : Global.androidDeviceInfo.brand,
    );
    _appUpdateInfo =
        await AppApi.update(context: context, params: requestDeviceInfo);

    _runAppUpdate();
  }

  /// 检查是否有新版
  Future _runAppUpdate() async {
    // 比较版本
    final isNewVersion =
        (_appUpdateInfo.latestVersion.compareTo(Global.packageInfo.version) ==
            1);

    // 安装
    if (isNewVersion == true) {
      _appUpdateConformDialog(() {
        Navigator.of(_context).pop();
        if (Global.isIOS == true) {
          // 去苹果店
          InstallPlugin.gotoAppStore(_appUpdateInfo.shopUrl);
        } else {
          // apk 下载安装
          toastInfo(msg: "开始下载升级包");
          _downloadAPKAndSetup(_appUpdateInfo.fileUrl);
        }
      });
    }
  }

  /// 下载文件 & 安装
  Future _downloadAPKAndSetup(String fileUrl) async {
    // 下载
    Directory externalDir = await getExternalStorageDirectory();
    String fullPath = externalDir.path + "/release.apk";

    Dio dio = Dio(BaseOptions(
        responseType: ResponseType.bytes,
        followRedirects: false,
        validateStatus: (status) {
          return status < 500;
        }));
    Response response = await dio.get(
      fileUrl,
    );

    File file = File(fullPath);
    var raf = file.openSync(mode: FileMode.write);
    raf.writeFromSync(response.data);
    await raf.close();

    // 安装
    await InstallPlugin.installApk(fullPath, Global.packageInfo.packageName);
  }

  /// 升级确认对话框
  void _appUpdateConformDialog(VoidCallback onPressed) {
    EasyDialog(
        title: Text(
          "发现新版本 ${_appUpdateInfo.latestVersion}",
          style: TextStyle(fontWeight: FontWeight.bold),
          textScaleFactor: 1.2,
        ),
        description: Text(
          _appUpdateInfo.latestDescription,
          textScaleFactor: 1.1,
          textAlign: TextAlign.center,
        ),
        height: 220,
        contentList: [
          Row(
            mainAxisAlignment: MainAxisAlignment.end,
            children: <Widget>[
              new FlatButton(
                padding: const EdgeInsets.only(top: 8.0),
                textColor: Colors.lightBlue,
                onPressed: onPressed,
                child: new Text(
                  "同意",
                  textScaleFactor: 1.2,
                ),
              ),
              new FlatButton(
                padding: const EdgeInsets.only(top: 8.0),
                textColor: Colors.lightBlue,
                onPressed: () {
                  Navigator.of(_context).pop();
                },
                child: new Text(
                  "取消",
                  textScaleFactor: 1.2,
                ),
              ),
            ],
          )
        ]).show(_context);
  }
}

```

#### 读取设备信息

- 插件

https://pub.flutter-io.cn/packages/device_info

- 全局信息

lib/global.dart

```dart
  /// 是否 ios
  static bool isIOS = Platform.isIOS;

  /// android 设备信息
  static AndroidDeviceInfo androidDeviceInfo;

  /// ios 设备信息
  static IosDeviceInfo iosDeviceInfo;

  /// 包信息
  static PackageInfo packageInfo;

  /// init
  static Future init() async {
    ...

    // 读取设备信息
    DeviceInfoPlugin deviceInfoPlugin = DeviceInfoPlugin();
    if (Global.isIOS) {
      Global.iosDeviceInfo = await deviceInfoPlugin.iosInfo;
    } else {
      Global.androidDeviceInfo = await deviceInfoPlugin.androidInfo;
    }

    // 包信息
    Global.packageInfo = await PackageInfo.fromPlatform();

    ...
```

- 定义升级信息 entity

lib/common/entitys/app.dart

```dart
class AppUpdateRequestEntity {
  String device;
  String channel;
  String architecture;
  String model;

  AppUpdateRequestEntity({
    this.device,
    this.channel,
    this.architecture,
    this.model,
  });

  factory AppUpdateRequestEntity.fromJson(Map<String, dynamic> json) =>
      AppUpdateRequestEntity(
        device: json["device"],
        channel: json["channel"],
        architecture: json["architecture"],
        model: json["model"],
      );

  Map<String, dynamic> toJson() => {
        "device": device,
        "channel": channel,
        "architecture": architecture,
        "model": model,
      };
}

class AppUpdateResponseEntity {
  String shopUrl;
  String fileUrl;
  String latestVersion;
  String latestDescription;

  AppUpdateResponseEntity({
    this.shopUrl,
    this.fileUrl,
    this.latestVersion,
    this.latestDescription,
  });

  factory AppUpdateResponseEntity.fromJson(Map<String, dynamic> json) =>
      AppUpdateResponseEntity(
        shopUrl: json["shopUrl"],
        fileUrl: json["fileUrl"],
        latestVersion: json["latestVersion"],
        latestDescription: json["latestDescription"],
      );

  Map<String, dynamic> toJson() => {
        "shopUrl": shopUrl,
        "fileUrl": fileUrl,
        "latestVersion": latestVersion,
        "latestDescription": latestDescription,
      };
}

```

- api 请求

lib/common/apis/app.dart

```dart
/// 系统相关
class AppApi {
  /// 获取最新版本信息
  static Future<AppUpdateResponseEntity> update({
    @required BuildContext context,
    AppUpdateRequestEntity params,
  }) async {
    var response = await HttpUtil().post(
      '/app/update',
      context: context,
      params: params,
    );
    return AppUpdateResponseEntity.fromJson(response);
  }
}
```

- 提交信息 获取版本

lib/common/utils/update.dart

```dart
  /// 获取更新信息
  Future run(BuildContext context) async {
    _context = context;

    // 提交 设备类型、发行渠道、架构、机型
    AppUpdateRequestEntity requestDeviceInfo = AppUpdateRequestEntity(
      device: Global.isIOS == true ? "ios" : "android",
      channel: Global.channel,
      architecture: Global.isIOS == true
          ? Global.iosDeviceInfo.utsname.machine
          : Global.androidDeviceInfo.device,
      model: Global.isIOS == true
          ? Global.iosDeviceInfo.name
          : Global.androidDeviceInfo.brand,
    );
    _appUpdateInfo =
        await AppApi.update(context: context, params: requestDeviceInfo);

    _runAppUpdate();
  }

  /// 检查是否有新版
  Future _runAppUpdate() async {
    // 比较版本
    final isNewVersion =
        (_appUpdateInfo.latestVersion.compareTo(Global.packageInfo.version) ==
            1);

    // 安装
    if (isNewVersion == true) {
      _appUpdateConformDialog(() {
        Navigator.of(_context).pop();
        if (Global.isIOS == true) {
          // 去苹果店
          InstallPlugin.gotoAppStore(_appUpdateInfo.shopUrl);
        } else {
          // apk 下载安装
          toastInfo(msg: "开始下载升级包");
          _downloadAPKAndSetup(_appUpdateInfo.fileUrl);
        }
      });
    }
  }
```

#### android 动态授权

- 插件

https://pub.flutter-io.cn/packages/permission_handler

- 官方文章

https://developer.android.com/training/permissions/requesting

https://developer.android.com/training/permissions/usage-notes

- AndroidManifest.xml 中加入权限

android/app/src/main/AndroidManifest.xml

```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

- flutter 启动页中执行授权

lib/pages/index/index.dart

在 initState 是执行

延迟 3 秒，用户体验好些

```dart
class _IndexPageState extends State<IndexPage> {
  @override
  void initState() {
    super.initState();

    if (Global.isRelease == true) {
      doAppUpdate();
    }
  }

  Future doAppUpdate() async {
    await Future.delayed(Duration(seconds: 3), () async {
      if (Global.isIOS == false &&
          await Permission.storage.isGranted == false) {
        await [Permission.storage].request();
      }
      if (await Permission.storage.isGranted) {
        AppUpdateUtil().run(context);
      }
    });
  }
```

#### android 目录权限

- 插件

https://pub.flutter-io.cn/packages/path_provider
https://pub.flutter-io.cn/packages/install_plugin

- 文章

https://developer.android.com/reference/androidx/core/content/FileProvider.html

- lib/common/utils/update.dart

```dart
  /// 下载文件 & 安装
  Future _downloadAPKAndSetup(String fileUrl) async {
    // 下载
    Directory externalDir = await getExternalStorageDirectory();
    String fullPath = externalDir.path + "/release.apk";

    Dio dio = Dio(BaseOptions(
        responseType: ResponseType.bytes,
        followRedirects: false,
        validateStatus: (status) {
          return status < 500;
        }));
    Response response = await dio.get(
      fileUrl,
    );

    File file = File(fullPath);
    var raf = file.openSync(mode: FileMode.write);
    raf.writeFromSync(response.data);
    await raf.close();

    // 安装
    await InstallPlugin.installApk(fullPath, Global.packageInfo.packageName);
  }
```

#### EasyDialog 快速提示框

- 插件

https://pub.flutter-io.cn/packages/easy_dialog

- lib/common/utils/update.dart

```dart
  /// 升级确认对话框
  void _appUpdateConformDialog(VoidCallback onPressed) {
    EasyDialog(
        title: Text(
          "发现新版本 ${_appUpdateInfo.latestVersion}",
          style: TextStyle(fontWeight: FontWeight.bold),
          textScaleFactor: 1.2,
        ),
        description: Text(
          _appUpdateInfo.latestDescription,
          textScaleFactor: 1.1,
          textAlign: TextAlign.center,
        ),
        height: 220,
        contentList: [
          Row(
            mainAxisAlignment: MainAxisAlignment.end,
            children: <Widget>[
              new FlatButton(
                padding: const EdgeInsets.only(top: 8.0),
                textColor: Colors.lightBlue,
                onPressed: onPressed,
                child: new Text(
                  "同意",
                  textScaleFactor: 1.2,
                ),
              ),
              new FlatButton(
                padding: const EdgeInsets.only(top: 8.0),
                textColor: Colors.lightBlue,
                onPressed: () {
                  Navigator.of(_context).pop();
                },
                child: new Text(
                  "取消",
                  textScaleFactor: 1.2,
                ),
              ),
            ],
          )
        ]).show(_context);
  }
```

## 资源

### 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

### 蓝湖设计稿（加微信给授权 ducafecat）

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

### YAPI 接口管理

http://yapi.demo.qunar.com/

### 参考

- 文章

https://developer.android.com/training/permissions/requesting
https://developer.android.com/training/permissions/usage-notes
https://developer.android.com/reference/androidx/core/content/FileProvider.html

- flutter 插件

https://pub.flutter-io.cn/packages/device_info
https://pub.flutter-io.cn/packages/path_provider
https://pub.flutter-io.cn/packages/permission_handler
https://pub.flutter-io.cn/packages/install_plugin
https://pub.flutter-io.cn/packages/easy_dialog

### VSCode 插件

- Flutter、Dart
- [Flutter Widget Snippets](https://marketplace.visualstudio.com/items?itemName=alexisvt.flutter-snippets)
- [Awesome Flutter Snippets](https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets)
- [Paste JSON as Code](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)
- [bloc](https://marketplace.visualstudio.com/items?itemName=FelixAngelov.bloc)
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
