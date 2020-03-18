---
title: Flutter 实战从零开始 新闻客户端 - 02 设计稿适配、加入图片字体资源、欢迎界面
date: 2020-02-27 00:00:00
tags: flutter
categories: Flutter实战从零开始
---

## 本节目标

- 加入图片资源
- 加入字体资源
- 设计稿适配
- 编写界面代码的逻辑和组织

## 1 加入图片资源

### 1.1 flutter 图片资源规则

- 官方说明

https://flutter.dev/docs/development/ui/assets-and-images

![](2020-03-02-11-29-01.png)

按这个规则编排，flutter 自动适配分辨率图片

- assets 目录

![](2020-03-02-11-29-58.png)

- yaml 配置

```yaml
assets:
  - assets/images/
```

- 代码调用

```dart
Image.asset("assets/images/logo.png")
```

### 1.2 蓝湖切图

![](2020-03-02-11-32-56.png)

注意选着下 ios 目标，这样会自动切图 1x 2x 3x 三种格式

## 2 加入字体资源

- 官方说明

https://flutter.dev/docs/cookbook/design/fonts

![](2020-03-02-11-36-13.png)

- assets 目录

![](2020-03-02-11-36-48.png)

只上传用到的 ttf 字体，这样能控制打包大小

- yaml 配置

```yaml
fonts:
  - family: Avenir
    fonts:
      - asset: assets/fonts/Avenir-Book.ttf
        weight: 400
  - family: Montserrat
    fonts:
      - asset: assets/fonts/Montserrat-SemiBold.ttf
        weight: 600
```

- 代码调用

![](2020-03-02-11-37-48.png)

## 3 编写欢迎界面

### 3.1 从上到下、从左到右、由大到小

![](2020-03-02-13-38-19.png)

### 3.2 设计稿适配

插件 flutter_screenutil

https://pub.flutter-io.cn/packages/flutter_screenutil

![](2020-03-02-13-41-31.png)

按设计稿比例适配

### 3.3 工具函数

![](2020-03-03-14-11-29.png)

- `screen.dart` 设计稿适配函数

```dart
import 'package:flutter_screenutil/flutter_screenutil.dart';

/// 设置宽度
double duSetWidth(double width) {
  return ScreenUtil().setWidth(width);
}

/// 设置宽度
double duSetHeight(double height) {
  return ScreenUtil().setHeight(height);
}

/// 设置字体尺寸
double duSetFontSize(double fontSize) {
  return ScreenUtil().setSp(fontSize);
}
```

- `utils.dart` 导出类库

```dart
library utils;

export 'screen.dart';
```

### 3.4 常量配置

![](2020-03-03-14-14-55.png)

- `colors.dart` 颜色

```dart
import 'dart:ui';

class AppColors {
  /// 主文本
  static const Color primaryText = Color.fromARGB(255, 45, 45, 47);

  /// 主控件-背景
  static const Color primaryElement = Color.fromARGB(255, 41, 103, 255);

  /// 主控件-文本
  static const Color primaryElementText = Color.fromARGB(255, 255, 255, 255);
}
```

- `values.dart` 导出类库

```dart
library values;

export 'colors.dart';
```

### 3.5 代码拆分

![](2020-03-02-13-43-25.png)

尽可能的拆分到不同的函数，方便维护

再复杂的业务，可以拆分到不同的组件文件，如 `welcome_header_widget.dart` `welcome_feature_widget.dart` `welcome_buttons_widget.dart`

## git 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.1

## 蓝湖设计稿

https://lanhuapp.com/url/wbhGq

## 视频

- [b 站](https://www.bilibili.com/video/av93660835)
- [油管镜像](https://www.youtube.com/watch?v=equsSqqwl9E)

## 参考

- [flutter_screenutil](https://pub.flutter-io.cn/packages/flutter_screenutil)
- [Adding assets and images](https://flutter.dev/docs/development/ui/assets-and-images)
- [Use a custom font](https://flutter.dev/docs/cookbook/design/fonts)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
