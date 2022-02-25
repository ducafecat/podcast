---
title: Flutter GetX + Material Design 3 Theme 样式自定义
tags: flutter
categories: flutter
date: 2022-02-23 10:00:00
---

![tips-getx-material-design3](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/tips-getx-material-design3.png)

## 效果展示

- 样式自定义

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220225123657.png)

- Material 样式调试工具

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220225123720.png)
![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220225123759.png)
![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220225123818.png)
![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220225123837.png)

## 本节目标

- 了解 Material Design 设计样式表
- Flutter 中自定义样式（全局、局部）
  - 颜色
  - 字体
  - 组件

## 视频

## 代码

- Flutter GetX 中定制你的样式
  https://github.com/ducafecat/flutter_develop_tips/tree/main/flutter_theme_custom_getx

- 调试你的样式
  https://github.com/ducafecat/flutter_develop_tips/tree/main/flutter_material_design3_theme_getx

## 参考

- Material Design 2
  https://material.io

- Material Design 3
  https://m3.material.io
  https://m3.material.io/styles/color/overview
  https://m3.material.io/styles/typography/overview
  https://m3.material.io/foundations/design-tokens/overview

- material 样式文件
  https://www.figma.com/community/file/1035203688168086460

- material 颜色样式生成器
  https://material-foundation.github.io/material-theme-builder

## 正文

### 了解 Material 样式表

- material 样式文件, figma 格式
  https://www.figma.com/community/file/1035203688168086460

> 导入你的 figma

- 颜色、字体

![figma颜色字体](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220225102123.png)

- 组件

![figma组件](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220225102230.png)

### Flutter 中一步步定制你的样式

#### 第一步: 生成 colorScheme，创建 ThemeData

- material-theme-builder 编辑器
  https://material-foundation.github.io/material-theme-builder/#/custom

> 导出 dart 格式导入你的项目

- lib/color_schemes.dart

```dart
import 'package:flutter/material.dart';

const lightColorScheme = ColorScheme(
  brightness: Brightness.light,
  primary: Color(0xFF6750A4),
  onPrimary: Color(0xFFFFFFFF),
  primaryContainer: Color(0xFFEADDFF),
  onPrimaryContainer: Color(0xFF21005D),
  secondary: Color(0xFF625B71),
  onSecondary: Color(0xFFFFFFFF),
  secondaryContainer: Color(0xFFE8DEF8),
  onSecondaryContainer: Color(0xFF1D192B),
  tertiary: Color(0xFF7D5260),
  onTertiary: Color(0xFFFFFFFF),
  tertiaryContainer: Color(0xFFFFD8E4),
  onTertiaryContainer: Color(0xFF31111D),
  error: Color(0xFFB3261E),
  errorContainer: Color(0xFFF9DEDC),
  onError: Color(0xFFFFFFFF),
  onErrorContainer: Color(0xFF410E0B),
  background: Color(0xFFFFFBFE),
  onBackground: Color(0xFF1C1B1F),
  surface: Color(0xFFFFFBFE),
  onSurface: Color(0xFF1C1B1F),
  surfaceVariant: Color(0xFFE7E0EC),
  onSurfaceVariant: Color(0xFF49454F),
  outline: Color(0xFF79747E),
  onInverseSurface: Color(0xFFF4EFF4),
  inverseSurface: Color(0xFF313033),
  inversePrimary: Color(0xFFD0BCFF),
  shadow: Color(0xFF000000),
);

const darkColorScheme = ColorScheme(
  brightness: Brightness.dark,
  primary: Color(0xFFD0BCFF),
  onPrimary: Color(0xFF381E72),
  primaryContainer: Color(0xFF4F378B),
  onPrimaryContainer: Color(0xFFEADDFF),
  secondary: Color(0xFFCCC2DC),
  onSecondary: Color(0xFF332D41),
  secondaryContainer: Color(0xFF4A4458),
  onSecondaryContainer: Color(0xFFE8DEF8),
  tertiary: Color(0xFFEFB8C8),
  onTertiary: Color(0xFF492532),
  tertiaryContainer: Color(0xFF633B48),
  onTertiaryContainer: Color(0xFFFFD8E4),
  error: Color(0xFFF2B8B5),
  errorContainer: Color(0xFF8C1D18),
  onError: Color(0xFF601410),
  onErrorContainer: Color(0xFFF9DEDC),
  background: Color(0xFF1C1B1F),
  onBackground: Color(0xFFE6E1E5),
  surface: Color(0xFF1C1B1F),
  onSurface: Color(0xFFE6E1E5),
  surfaceVariant: Color(0xFF49454F),
  onSurfaceVariant: Color(0xFFCAC4D0),
  outline: Color(0xFF938F99),
  onInverseSurface: Color(0xFF1C1B1F),
  inverseSurface: Color(0xFFE6E1E5),
  inversePrimary: Color(0xFF6750A4),
  shadow: Color(0xFF000000),
);
```

> 分别定义了 `ColorScheme` 类型的，`明亮` 和 `暗色` 两种色彩定义

- lib/theme.dart

```dart
import 'package:flutter/material.dart';

import 'color_schemes.dart';

class AppTheme {
  static ThemeData light = ThemeData(
    colorScheme: lightColorScheme,
  );

  static ThemeData dark = ThemeData(
    colorScheme: darkColorScheme,
  );
}
```

> 创建 `明亮`和`暗色`两种`ThemeData`样式对象
> 分别设置 `colorScheme` 属性为上面的 `lightColorScheme` `darkColorScheme` 色彩定义

#### 第二步: 切换 theme 效果

- 加入 `GetX` pub 包

https://pub.flutter-io.cn/packages/get

```sh
> flutter pub add get
```

- 新增 `lib/global.dart` 全局变量管理

```dart
import 'package:get/get.dart';

import 'theme.dart';

class GlobalService extends GetxService {
  static GlobalService get to => Get.find();

  // 是否黑暗模式
  final RxBool _isDarkModel = Get.isDarkMode.obs;
  bool get isDarkModel => _isDarkModel.value;

  // 切换模式
  void switchThemeModel() {
    _isDarkModel.value = !_isDarkModel.value;
    Get.changeTheme(
      _isDarkModel.value == true ? AppTheme.dark : AppTheme.light,
    );
  }
}
```

- 修改 `lib/main.dart` main 函数

```dart
void main() {
  appInit();
}

appInit() async {
  // 初始 fluuter 引擎
  WidgetsFlutterBinding.ensureInitialized();

  // 初始
  Get.put<GlobalService>(GlobalService());

  // 启动
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Flutter Demo',

      // 初始样式
      theme:
          GlobalService.to.isDarkModel == true ? AppTheme.dark : AppTheme.light,

      // home 界面
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

> `ensureInitialized` 会先绑定 Flutter 引擎和原生端的接口，为后续的组件初始做准备

- 加入 `lib/main.dart` 切换按钮

```dart
floatingActionButton: Obx(() {
  return FloatingActionButton(
    onPressed: () {
      GlobalService.to.switchThemeModel();
    },
    child: Icon(
      GlobalService.to.isDarkModel == true
          ? Icons.dark_mode
          : Icons.light_mode,
    ),
  );
}),
```

> 使用 `Obx` 方式响应按钮切换

#### 第三步: 自定义主题色 MaterialColor

- 新增 `lib/string_to_material_color.dart` 字符串转 `MaterialColor` 函数

```dart
/// 字符串转 Color 类型
/// Color color = stringToColor("40c254");
Color stringToColor(String source) {
  return Color(int.parse(source, radix: 16) | 0xFF000000);
}

/// 字符串转 MaterialColor
/// MaterialColor materialColor = stringToMaterialColor("40c254");
MaterialColor stringToMaterialColor(String source) {
  Color color = stringToColor(source);

  List<double> strengths = <double>[.05];
  Map<int, Color> swatch = <int, Color>{};
  final int r = color.red, g = color.green, b = color.blue;

  for (int i = 1; i < 10; i++) {
    strengths.add(0.1 * i);
  }
  for (var strength in strengths) {
    final double ds = 0.5 - strength;
    swatch[(strength * 1000).round()] = Color.fromRGBO(
      r + ((ds < 0 ? r : (255 - r)) * ds).round(),
      g + ((ds < 0 ? g : (255 - g)) * ds).round(),
      b + ((ds < 0 ? b : (255 - b)) * ds).round(),
      1,
    );
  }
  return MaterialColor(color.value, swatch);
}
```

- 选取颜色

https://www.color-hex.com/

- 设置 `lib/theme.dart` `light` 对象的属性 `primarySwatch`

```dart
class AppTheme {
  // 自定义主色
  static MaterialColor myColor = stringToMaterialColor("008744");

  static ThemeData light = ThemeData(
    primarySwatch: myColor,
  );
```

#### 第四步: 全局定义组件样式

- 修改 `lib/theme.dart` , `appBarTheme` 属性

```dart
class AppTheme {

  static ThemeData light = ThemeData(
    colorScheme: lightColorScheme,

    // 全局 - 自定义 AppBarTheme
    appBarTheme: ThemeData.light().appBarTheme.copyWith(
          backgroundColor: Colors.yellow,
          titleTextStyle: ThemeData.light().textTheme.titleLarge?.copyWith(
                color: Colors.black,
                fontWeight: FontWeight.bold,
                fontSize: 20,
              ),
        ),
  );
```

> `copyWith` 方法将会把其它属性一起复制

#### 第五步: 局部定义组件样式

- 修改 `lib/main.dart`, `MyHomePage` 界面的 `build` 方法

```dart
  // 通过 GetX 的 theme 属性，读取当前样式 `ThemeData`
  ThemeData theme = Get.theme;

  return Theme(
    data: theme.copyWith(
      appBarTheme: theme.appBarTheme.copyWith(
        backgroundColor: theme.colorScheme.tertiary,
      ),
    ),
    child: Scaffold(
```

> 用 `Theme` 包裹需要局部使用的组件, 设置 `data` 属性

## 总结

本文介绍了 Material Design 3 中的样式表，以及如何在 Flutter 中如何修改全局、局部的样式，MaterialCorlor 和 Color 的区别以及如何使用。

我们可以发现如果和设计师协调好 颜色、字体、组件交互的样式标准，然后再适配下 Material Design ，样式配置将会快很多，很多时候样式调整给客户的是一种新鲜感，比如节假日、特殊日子里色调改下就行。

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
