---
title: Flutter GetX + Material Design 3 Theme 样式自定义
tags: flutter
categories: flutter
date: 2022-02-23 10:00:00
---

![tips-getx-material-design3](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/tips-getx-material-design3.png)

## 前言

Flutter 说到底是谷歌的产物，在样式配置上，肯定和自家 Material Design 是有依赖的

这篇文章就是说说这个 Material Design 3 如何适配，以及 GetX 的配合

开始前需要思考一个问题，每家公司都有自己的样式标准，作为 Flutter 开发，如何快速便捷的融合 Material Design 设计思想，如何和设计师配合

## 本节目标

- 了解 Material Design
  - Material Design 3
  - Material Design 3 Color 颜色系统
  - Dynamic color
  - typography 字体定义
- Flutter 全局、局部 定义样式
- GetX 适配

## 视频

## 代码

https://github.com/ducafecat/flutter_develop_tips/tree/main/flutter_material_design3_theme_getx

## 参考

- Material Design 2
  https://material.io

- Material Design 2
  - https://m3.material.io
  - https://m3.material.io/styles/color/overview
  - https://m3.material.io/styles/typography/overview
  - https://m3.material.io/foundations/design-tokens/overview
  - https://www.figma.com/community/file/1035203688168086460

## 正文

### 样式表是关键

如果你和后端的接口是 api 定义，那么和设计师的接口就是样式表。

一套样式规范各个 APP、平台、语言。

https://www.figma.com/community/file/1035203688168086460

![image-20220223151441694](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223151441694.png)

https://m3.material.io/foundations/design-tokens/overview

![image-20220223145109406](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223145109406.png)

![image-20220223145142460](/Users/ducafecat/Library/Application Support/typora-user-images/image-20220223145142460.png)

https://ant.design/docs/spec/introduce-cn

![image-20220223145636593](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223145636593.png)

https://tdesign.tencent.com/

![image-20220223150757928](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223150757928.png)

### 了解 Material Design

https://material.io/

![image-20220223145847387](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223145847387.png)

https://m3.material.io

![image-20220223145911897](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223145911897.png)

#### 色彩系统

https://m3.material.io/styles/color/overview

![image-20220223145956159](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223145956159.png)

https://material-foundation.github.io/material-theme-builder/#/custom

![image-20220223150048765](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223150048765.png)

#### 动态颜色

https://m3.material.io/styles/color/dynamic-color/overview

https://material-foundation.github.io/material-theme-builder/#/dynamic

![image-20220223150231448](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223150231448.png)

#### 颜色使用规则

https://m3.material.io/styles/color/the-color-system/color-roles

![image-20220223154222318](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223154222318.png)

#### 字体

https://m3.material.io/styles/typography/overview

![image-20220223150506741](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223150506741.png)

#### 组件定义

https://m3.material.io/components/all-buttons

![image-20220223151755951](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223151755951.png)

### Flutter 配置样式

#### ColorScheme 颜色架构

- 第一步：使用 material theme buuilder 工具导出 Dart 颜色表
  https://material-foundation.github.io/material-theme-builder/#/custom

  ![image-20220223151837235](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223151837235.png)

  文件 lib/style/color_schemes.dart

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

  > 可以发现其中包含了 明亮 黑暗 两套定义。

- 第二步：定义 ThemeData

  文件 lib/style/theme.dart

  ```dart
  import 'package:flutter/material.dart';

  import 'index.dart';

  class AppTheme {
    static ThemeData light = ThemeData(
      colorScheme: lightColorScheme,
    );
    static ThemeData dark = ThemeData(
      colorScheme: darkColorScheme,
    );
  }

  ```

- 第三步：设置 Theme
  文件 lib/main.dart

  ```dart
    runApp(
      GetMaterialApp(
        ...
        theme:
            GlobalService.to.isDarkModel == true ? AppTheme.dark : AppTheme.light,
        ...
      ),
    );
  ```

#### TextTheme 字体

https://m3.material.io/styles/typography/overview

- 查阅当前字体定义
  我写了个程序进行打印 ，文件 lib/pages/typography
  ![image-20220223153124112](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223153124112.png)
  代码参考

  ```dart
  // displayLarge
  KeyValueModel<TextStyle?>("displayLarge", tt.displayLarge),
  KeyValueModel<TextStyle?>("displayMedium", tt.displayMedium),
  KeyValueModel<TextStyle?>("displaySmall", tt.displaySmall),
  // headlineLarge
  KeyValueModel<TextStyle?>("headlineLarge", tt.headlineLarge),
  KeyValueModel<TextStyle?>("headlineMedium", tt.headlineMedium),
  KeyValueModel<TextStyle?>("headlineSmall", tt.headlineSmall),
  // titleLarge
  KeyValueModel<TextStyle?>("titleLarge", tt.titleLarge),
  KeyValueModel<TextStyle?>("titleMedium", tt.titleMedium),
  KeyValueModel<TextStyle?>("titleSmall", tt.titleSmall),
  // bodyLarge
  KeyValueModel<TextStyle?>("bodyLarge", tt.bodyLarge),
  KeyValueModel<TextStyle?>("bodyMedium", tt.bodyMedium),
  KeyValueModel<TextStyle?>("bodySmall", tt.bodySmall),
  // labelLarge
  KeyValueModel<TextStyle?>("labelLarge", tt.labelLarge),
  KeyValueModel<TextStyle?>("labelMedium", tt.labelMedium),
  KeyValueModel<TextStyle?>("labelMedium", tt.labelMedium),
  // headline 1~6
  KeyValueModel<TextStyle?>("headline1", tt.headline1),
  KeyValueModel<TextStyle?>("headline2", tt.headline2),
  KeyValueModel<TextStyle?>("headline3", tt.headline3),
  KeyValueModel<TextStyle?>("headline4", tt.headline4),
  KeyValueModel<TextStyle?>("headline5", tt.headline5),
  KeyValueModel<TextStyle?>("headline6", tt.headline6),
  // subtitle
  KeyValueModel<TextStyle?>("subtitle1", tt.subtitle1),
  KeyValueModel<TextStyle?>("subtitle2", tt.subtitle2),
  // bodyText
  KeyValueModel<TextStyle?>("bodyText1", tt.bodyText1),
  KeyValueModel<TextStyle?>("bodyText2", tt.bodyText2),
  // other
  KeyValueModel<TextStyle?>("caption", tt.caption),
  KeyValueModel<TextStyle?>("button", tt.button),
  KeyValueModel<TextStyle?>("overline", tt.overline),
  ```

- 自定义修改
  lib/style/theme.dart

  ```dart
    static ThemeData light = ThemeData(
      ...
      textTheme: TextTheme(
        displayLarge: ThemeData.light().textTheme.displayLarge?.copyWith(
              color: Colors.blue,
            ),
      ),
  ```

  ![image-20220223153830560](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223153830560.png)

#### Material Color 色阶

设置 ThemeData 的 primarySwatch ，需要 MaterialColor 类型，Color 类型是不行。

```dart
static ThemeData light = ThemeData(
    primarySwatch: newColor,
    ...
```

![image-20220223165823438](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223165823438.png)

MaterialColor 是一个 10 层的色彩阶梯

```dart
class MaterialColor extends ColorSwatch<int> {
  /// Creates a color swatch with a variety of shades.
  ///
  /// The `primary` argument should be the 32 bit ARGB value of one of the
  /// values in the swatch, as would be passed to the [new Color] constructor
  /// for that same color, and as is exposed by [value]. (This is distinct from
  /// the specific index of the color in the swatch.)
  const MaterialColor(int primary, Map<int, Color> swatch) : super(primary, swatch);

  /// The lightest shade.
  Color get shade50 => this[50]!;

  /// The second lightest shade.
  Color get shade100 => this[100]!;

  /// The third lightest shade.
  Color get shade200 => this[200]!;

  /// The fourth lightest shade.
  Color get shade300 => this[300]!;

  /// The fifth lightest shade.
  Color get shade400 => this[400]!;

  /// The default shade.
  Color get shade500 => this[500]!;

  /// The fourth darkest shade.
  Color get shade600 => this[600]!;

  /// The third darkest shade.
  Color get shade700 => this[700]!;

  /// The second darkest shade.
  Color get shade800 => this[800]!;

  /// The darkest shade.
  Color get shade900 => this[900]!;
}
```

![image-20220223165928940](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223165928940.png)

创建 MaterialColor 类型，这里我用了 extension 方式

```dart
  static ThemeData light = ThemeData(
    primarySwatch: "#B36200".toColor().materialColor,
    brightness: Brightness.light,
    ...
```

![image-20220223170650194](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223170650194.png)

这里我写了个色彩阶梯的调试界面，可以直观的查阅

lib/pages/material_color

```dart
      KeyValueModel<Color>("50", myColor.shade50),
      KeyValueModel<Color>("100", myColor.shade100),
      KeyValueModel<Color>("200", myColor.shade200),
      KeyValueModel<Color>("300", myColor.shade300),
      KeyValueModel<Color>("400", myColor.shade400),
      KeyValueModel<Color>("500", myColor.shade500),
      KeyValueModel<Color>("600", myColor.shade600),
      KeyValueModel<Color>("700", myColor.shade700),
      KeyValueModel<Color>("800", myColor.shade800),
      KeyValueModel<Color>("900", myColor.shade900),
```

> .shadeXX 的方式访问 10 个阶梯的属性

![image-20220223170853046](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223170853046.png)

#### 组件局部定义样式

之前说的都是全局定义，如果我想某个界面的 AppBar 背景是 辅助色，可以这样操作

文件 lib/pages/local_appbar/view.dart

```dart
  @override
  Widget build(BuildContext context) {
    return GetBuilder<LocalAppbarController>(
      init: LocalAppbarController(),
      id: "local_appbar",
      builder: (_) {
        ThemeData theme = Get.theme;
        return Theme(
          data: theme.copyWith(
            appBarTheme: theme.appBarTheme.copyWith(
              backgroundColor: theme.colorScheme.tertiary,
            ),
          ),
          child: Scaffold(
            appBar: AppBar(
              title: Text(controller.viewTitle),
            ),
            body: SafeArea(
              child: _buildView(),
            ),
          ),
        );
      },
    );
  }
```

> 通过 Theme 组件，局部定义一个样式
>
> 这里把导航栏颜色改成了 tertiary ，注意需要 copyWith 方式来继承其它样式属性

![image-20220223181354464](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/image-20220223181354464.png)

### GetX 的相关操作

#### 切换主题 changeTheme

文件 lib/services/global.dart

```dart
  // 切换模式
  void switchThemeModel() {
    _isDarkModel.value = !_isDarkModel.value;
    Get.changeTheme(
      _isDarkModel.value == true ? AppTheme.dark : AppTheme.light,
    );
  }
```

> Get.changeTheme 传入你的 ThemeData 切换

#### 切换主题 changeThemeMode

```dart
Get.changeThemeMode(ThemeMode.dark);
```

> 直接按枚举类型 light dark 切换

枚举类型 ThemeMode 参考

```dart
enum ThemeMode {
  /// Use either the light or dark theme based on what the user has selected in
  /// the system settings.
  system,

  /// Always use the light mode regardless of system preference.
  light,

  /// Always use the dark mode (if available) regardless of system preference.
  dark,
}
```

#### 读取当前 ThemeData 数据

```dart
ThemeData theme = Get.theme;
```

## 总结

本文介绍了 Material Design 3 包含了哪些东西，样式表中 字体、颜色的重要性，在 Flutter 中如何修改全局、局部的样式，MaterialCorlor 和 Color 的区别以及如何使用。

我们可以发现如果和设计师协调好 颜色、字体、组件交互的样式标准，然后再适配下 Material Design ，样式配置将会快很多，很多时候样式调整给客户的是一种新鲜感，比如节假日、特殊日子里色调改下。

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
