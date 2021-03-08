---
title: 2021 Flutter v2 重要的 16 个特性
date: 2021-3-8 00:00:00
tags: flutter
categories: Flutter见闻
---

![](2021-03-08-17-43-31.png)

![](2021-03-08-17-41-36.png)

## 7 大平台 -> windows、macos、linux、web、embedded、ios、android

![](2021-03-08-09-43-33.png)

![](2021-03-08-11-24-37.png)

- https://flutter.gskinner.com/
- https://github.com/gskinnerTeam/flutter-folio

## web 平台优化、进入稳定版

- 3 个方向

  - pwa 缓存、推送、移动功能
  - spa 单页程序类似 vue rect
  - expanding mobile 快速迁移 app、复用代码

- irobot 构建基于 flutter

![](2021-03-08-11-30-53.png)

https://edu.irobot.com/the-latest/building-a-coding-experience-for-all

- 技术架构

![](2021-03-08-11-31-16.png)

2D 3D 渲染 WebGL Skia WebAssembly Canvas

- 稳定的版本

![](2021-03-08-11-33-05.png)

- 性能

  - HTML renderer: HTML 渲染器: Uses a combination of HTML elements, CSS, Canvas elements, and SVG elements. This renderer has a smaller download size. 使用 HTML 元素、 CSS、 Canvas 元素和 SVG 元素的组合

  - CanvasKit renderer: CanvasKit 渲染器: This renderer is fully consistent with Flutter mobile and desktop, has faster performance with higher widget density, but adds about 2MB in download size.

- 试水项目

  - 编辑器
    https://rive.app/

  - 动画
    https://flutterplasma.dev/

  - invoice
    - https://www.invoiceninja.com/

## canonical 支持

https://medium.com/flutter/announcing-flutter-linux-alpha-with-canonical-19eb824590a9

- why canonical 大力推 flutter !，主要以下几点

  - 快速增长的 flutter 应用
  - 多品台支持
  - 设备优化的好
  - 丰富的组件库
  - IDE 环境成熟 Visual Studio Code, Android Studio, and IntelliJ

- 简易安装

https://snapcraft.io/flutter

```
$ snap install --classic flutter
$ snap install --classic code
$ code --install-extension dart-code.flutter
```

- 快速模板

![](2021-03-08-13-42-11.png)

```
$ flutter channel dev
$ flutter upgrade
$ flutter config --enable-linux-desktop

$ flutter create counter
$ cd counter
$ flutter run -d linux
```

现有项目升级

```
$ cd my_flutter_app
$ flutter create .
```

- 代码示例

  - https://github.com/flutter/samples/tree/master/experimental/desktop_photo_search

  ![](2021-03-08-13-43-29.png)

  - https://github.com/flutter/gallery

  ![](2021-03-08-13-44-50.png)

- 教程 Write a Flutter desktop application

https://codelabs.developers.google.com/codelabs/flutter-github-graphql-client/index.html#0

## 组件库升级、对 ios 支持加强

- 新增 iOS 功能

  - CupertinoSearchTextField
    https://api.flutter-io.cn/flutter/cupertino/CupertinoSearchTextField-class.html
    ![](2021-03-08-13-47-15.png)

  - CupertinoFormSection、CupertinoFormRow 和 CupertinoTextFormFieldRow
    https://api.flutter.cn/flutter/cupertino/CupertinoFormSection-class.html
    https://api.flutter.cn/flutter/cupertino/CupertinoFormRow-class.html
    https://api.flutter.cn/flutter/cupertino/CupertinoTextFormFieldRow-class.html
    ![](2021-03-08-13-47-20.png)

  - 整体性能优化
    https://github.com/flutter/flutter/issues/60267#issuecomment-762786388

- 新增 Widget: Autocomplete 和 ScaffoldMessenger

  - AutocompleteCore
    https://github.com/flutter/flutter/pull/62927
    ![](2021-03-08-13-48-11.png)

  - ScaffoldMessenger
    https://github.com/flutter/flutter/pull/64101
    ![](2021-03-08-13-49-36.png)

## Flutter for Surface Duo & 折叠屏

![](2021-03-08-17-38-42.png)

![](2021-03-08-17-39-36.png)

- https://docs.microsoft.com/zh-cn/dual-screen/flutter/

- https://docs.microsoft.com/zh-cn/dual-screen/flutter/mediaquery

## 混合编程

https://flutter.cn/docs/development/add-to-app

![](2021-03-08-13-51-06.png)

过去，额外 Flutter 实例的内存占用量与第一个 Flutter 实例相同。在 Flutter 2 中，我们将创建额外 Flutter 引擎的静态内存占用量降低了约 99%，使每个实例的占用量大约为 180kB。

## Dart Null Safety

Dart 是一种类型安全的语言，这意味着当开发者获取某种类型的变量时，编译器可以保证它是该类型，但是类型安全本身不能保证变量不是 null。

Null errors 非常常见的问题，在 GitHub 上 可以搜索到成千上万由于 null 导致 Dart 代码出现异常的问题，甚至有成千上万的 commits 试图解决这些问题。

- https://dartpad.dev/
- https://nullsafety.dartpad.dev/

```dart
void main() {
  ps(null);
}

void ps(List<String> files) {
  for (var file in files) {
    print(file.isEmpty());
  }
}
```

最后，个人的额外提醒，目前在根目录的 analysis_options.yaml 添加如下配置就可以开启 null safety，另外 Flutter 需要 dart sdk 2.9 。

```
analyzer:
 enable-experiment:
 - non-nullable
```

## flutter fix

- 统计

```
dart fix --dry-run
```

- 应用

```
dart fix --apply
```

## flutter DevTools 开发工具升级

- 性能监控

https://flutter.dev/docs/perf/rendering/ui-performance

.vscode/launch.json

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "flutter_learn_news-1.0.15",
      "request": "launch",
      "type": "dart"
    },
    {
      "name": "profile",
      "request": "launch",
      "type": "dart",
      "flutterMode": "profile"
    }
  ]
}
```

![](2021-03-08-15-22-54.png)

- Invert Oversized Images

DevTools 的另一个新功能是能够轻松发现所显示的分辨率低于其实际分辨率的图像，这有助于追踪应用过大和内存占用过多等情况。若要启用此功能，请在 Flutter Inspector 中启用 Invert Oversized Images。

![](2021-03-08-15-26-36.png)

![](2021-03-08-15-26-42.png)

- 弹性布局

![](2021-03-08-15-27-44.png)

## Android Studio/IntelliJ 扩展

我们也为 IntelliJ 系列 IDE 的 Flutter 插件添加了一些适用于 Flutter 2 的新功能。首先，我们在其中新增了一个项目向导，该向导与 IntelliJ 中的新向导风格一致。

![](2021-03-08-15-28-13.png)

![](2021-03-08-15-28-19.png)

## Visual Studio Code 扩展

适用于 Visual Studio Code 的 Flutter 扩展也针对 Flutter 2 进行了优化，我们首先引入了一些测试增强功能，例如重新运行失败测试的能力。

![](2021-03-08-15-29-10.png)

经过两年的逐步发展，对 Dart 的 LSP (语言服务器协议) 支持已经成为在 Flutter 扩展中将 Dart 分析器集成到 Visual Studio Code 中的默认方式。LSP 支持为 Flutter 开发带来了许多改进，包括在当前的 Dart 文件中应用特定的所有修复，以及能够补全代码以生成完整函数调用 (包括括号和所需参数) 的能力。

![](2021-03-08-15-29-44.png)

![](2021-03-08-15-29-49.png)

LSP 支持不仅限于 Dart，它还支持 pubspec.yaml 及 analysis_options.yaml 文件中的代码补全。

![](2021-03-08-15-30-05.png)

## sentry 升级对 flutter 的支持

https://docs.sentry.io/platforms/flutter/

整合了对设备端错误的收集

## upgraded firebase plugins for flutter

https://firebase.flutter.dev/

![](2021-03-08-09-50-52.png)

## Flutter Community Plus Plugins

https://plus.fluttercommunity.dev/

## google mobile ads for flutter

![](2021-03-08-09-51-39.png)

## DartPad 升级到支持 Flutter 2

![](2021-03-08-15-30-38.png)

https://dartpad.dev/

## 配置 flutter 2

- 下载 Dev channel (macOS)

https://flutter.dev/docs/development/tools/sdk/releases?tab=macos

- fvm 切换

https://github.com/leoafarias/fvm

复制 sdk 到 `/Users/{youname}/.fvm/versions`

```
fvm list
fvm use 2.1.0
```

- 启用特性

```
flutter config --enable-macos-desktop
flutter config --enable-windows-desktop
flutter config --enable-linux-desktop
```

- 编译

```
flutter run -d windows
flutter run -d macos
flutter run -d linux
flutter run -d android
flutter run -d ios
flutter run -d web
```

## 参考

- https://flutter.gskinner.com/
- https://github.com/gskinnerTeam/flutter-folio
- https://mp.weixin.qq.com/s/EzS3dtpZB_i9p358qqlBpg
- https://docs.sentry.io/platforms/flutter/
- https://snapcraft.io/flutter
- https://plus.fluttercommunity.dev/
- https://medium.com/flutter/flutter-web-support-hits-the-stable-milestone-d6b84e83b425
- https://rive.app/
- https://medium.com/flutter/announcing-flutter-linux-alpha-with-canonical-19eb824590a9
- https://www.windowscentral.com/surface-duo

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
