---
title: Flutter 百度地图插件开发 - 01 编写设备端组件的正确姿势!
date: 2020-07-30 00:00:00
tags: flutter
categories: Flutter见闻
---

![](2020-07-30-21-51-56.png)

# 本节目标

- 闲鱼手册中提到的混合开发
- 编写到使用一个 flutter 组件的完整过程

## 视频

https://www.bilibili.com/video/bv1iT4y1j72t

<iframe src="//player.bilibili.com/player.html?bvid=bv1iT4y1j72t&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

## 代码

https://github.com/ducafecat/flutter_baidu_plugin_ducafecat/releases/tag/1.0.1

## 正文

### 从古至今、移动开发不可回避的问题

- flutter、weex、React Native、Cordova

![](2020-07-30-17-35-34.png)

- 开发模式

![](2020-07-30-17-48-41.png)

## 聊聊 Flutter in Action 闲鱼最佳实践

电子书下载

https://c.tb.cn/I3.ZZpRl

### 第二代混合技术方案 FlutterBoost

- 项目开源地址

https://github.com/alibaba/flutter_boost

- 架构图

![](2020-07-30-20-19-21.png)

- Native 层概念

● Container：Native 容器，平台 Controller，Activity，ViewController
● Container Manager：容器的管理者
● Adaptor：Flutter 是适配层
● Messaging：基于 Channel 的消息通信

- Dart 层概念

● Container：Flutter 用来容纳 Widget 的容器，具体实现为 Navigator 的派生类
● Container Manager：Flutter 容器的管理，提供 show，remove 等 Api
● Coordinator: 协调器，接受 Messaging 消息，负责调用 Container Manager 的状态管理。
● Messaging：基于 Channel 的消息通信

### Flutter & FaaS 云端一体化

![](2020-07-30-20-49-45.png)

### Flutter 会为以下团队带来较大的收益

![](2020-07-30-21-08-03.png)

### Flutter Redux

![](2020-07-30-21-15-03.png)

### 混合工程下的 Flutter 研发结构

![](2020-07-30-21-25-14.png)

## 动手写第一个 Flutter 组件

### 创建 flutter 组件工程

```sh
$ flutter create --org tech.ducafecat --template=plugin -a java -i objc flutter-baidu-plugin-ducafecat
```

### 加入 加法 函数

- dart 代码

lib/flutter_baidu_plugin_ducafecat.dart

```dart
import 'dart:async';

import 'package:flutter/services.dart';

class FlutterBaiduPluginDucafecat {
  static const MethodChannel _channel =
      const MethodChannel('flutter_baidu_plugin_ducafecat');

  static Future<String> get platformVersion async {
    final String version = await _channel.invokeMethod('getPlatformVersion');
    return version;
  }

  static Future<int> duAddOne(int num) async {
    final int val = await _channel.invokeMethod('duAddOne', {"num": num});
    return val;
  }
}

```

- android 代码

android/src/main/java/tech/ducafecat/flutter_baidu_plugin_ducafecat/FlutterBaiduPluginDucafecatPlugin.java

```java
  @Override
  public void onMethodCall(@NonNull MethodCall call, @NonNull Result result) {
    if (call.method.equals("getPlatformVersion")) {
      result.success("Android " + android.os.Build.VERSION.RELEASE);
    }
    else if (call.method.equals("duAddOne")) {
      int val = 100;
      val += Integer.valueOf(call.argument("num").toString());
      result.success(val);
    }
    else {
      result.notImplemented();
    }
  }
```

- ios 代码

ios/Classes/FlutterBaiduPluginDucafecatPlugin.m

```obj-c
#import "FlutterBaiduPluginDucafecatPlugin.h"

@implementation FlutterBaiduPluginDucafecatPlugin
+ (void)registerWithRegistrar:(NSObject<FlutterPluginRegistrar>*)registrar {
  FlutterMethodChannel* channel = [FlutterMethodChannel
      methodChannelWithName:@"flutter_baidu_plugin_ducafecat"
            binaryMessenger:[registrar messenger]];
  FlutterBaiduPluginDucafecatPlugin* instance = [[FlutterBaiduPluginDucafecatPlugin alloc] init];
  [registrar addMethodCallDelegate:instance channel:channel];
}

- (void)handleMethodCall:(FlutterMethodCall*)call result:(FlutterResult)result {
  if ([@"getPlatformVersion" isEqualToString:call.method]) {
    result([@"iOS " stringByAppendingString:[[UIDevice currentDevice] systemVersion]]);
  }
  else if ([@"duAddOne" isEqualToString:call.method]) {
      NSInteger val = 100;
      val += [[call.arguments objectForKey:@"num"] intValue];
      result([NSNumber numberWithLong:val]);
  }
  else {
    result(FlutterMethodNotImplemented);
  }
}

@end

```

### 新建工程调用

- pubspec.yaml

```yaml
flutter_baidu_plugin_ducafecat:
  git:
    url: https://github.com/ducafecat/flutter_baidu_plugin_ducafecat
    version: ^0.0.1
```

- 调用 加法

```dart
  void _incrementCounter() async {
    _counter = await FlutterBaiduPluginDucafecat.duAddOne(20);
    setState(() {});
  }
```

## 参考

- https://cordova.apache.org/
- https://reactnative.dev/
- https://flutter.dev/
- https://weex.apache.org/

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
