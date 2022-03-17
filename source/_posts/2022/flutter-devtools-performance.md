---
title: 2022 Flutter Performance 性能调试工具 devTools
tags: flutter
categories: flutter
date: 2022-03-17 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317232332.png)

## 前言

Flutter Release 发版前肯定需要性能测试的，今天我们就一起来讨论下，这个话题可以聊的很深入，我这里就做个抛砖引玉吧。

## 本节目标

- 调试工具使用
  - Performance
  - CPU Profiler
  - Memory
  - Package Size
  - Inspector Widget 描边
- 性能优化几点建议

## 视频

## 参考

- Flutter 性能分析
  https://flutter.cn/docs/perf/rendering/ui-performance

- 使用性能视图 (Performance view)
  https://flutter.cn/docs/development/tools/devtools/performance

- Using the app size tool
  https://docs.flutter.dev/development/tools/devtools/app-size#generating-size-files

## 正文

### devTools performance 打开方式

> 调试必须真机！

- vscode

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317230021.png)

> "flutterMode": "profile" 这个选项打开

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317230246.png)

> 命令模式打开 `open devTools`

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317230343.png)

> 选择在浏览器中打开

- android studio / intellij

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317233650.png)

> 一键进入 `profile` 模式，很简单

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317233506.png)

> 点击 `Open devTools` 直接进入浏览器

### performance cpu 排查

- 加入测试代码

```dart
  void imBusy() {
    for (var i = 0; i < 9999999; i++) {}
  }

  @override
  Widget build(BuildContext context) {
    imBusy();
    ...
```

- 视图

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317215555.png)

> UI 一般就是 cpu 了
> Raster 可以理解成 gpu
> 颜色偏红就是耗时长
> Timeline 最下面的一条就是你的代码栈

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317215932.png)

> 为了方便排查 勾选这三个项目，剩下的就是你自己的函数
> Cpu Profile > Cpu Flame Chart 图标能查看具体的函数

### CPU Profiler 排查

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317220258.png)

> 需要点击 Record , 然后你操作界面, 记得 Stop

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317220357.png)

> 这边也是过滤掉系统的 ，三个都勾选上

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317220500.png)

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317220612.png)

> 64% 都消耗在了 imBusy 这个函数上，没啥好说了，还告诉你文件的位置

### Memory 视图

我们准备 2 张大图载入

```dart
  Widget _buildBigAssetsPicture() {
    return Image.asset("assets/cafe.jpg");
  }

  Widget _buildBigAssetsPicture2() {
    return Image.asset("assets/love.jpg");
  }

  ...

    body: Center(
    child: Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        // 载入大图
        _buildBigAssetsPicture(),
        _buildBigAssetsPicture2(),

        ...
```

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317223113.png)

> Raster 这块耗时就很厉害了, 性能面板都友好的提示你这是错误了

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317223507.png)

> 点击 Performance Overlay 后 app 界面上会出来两个图标
> 上面的 Raster 就是 gpu
> 下面的 UI 就是你的 cpu
> 可以发现我载入两张图片后，平均每 71 ms / 帧，太耗时了，卡
> cpu 还可以 平均 2.1 ms / 帧

### Memory 分析

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317224205.png)

> 打开 Android Memory 选项，可以看的更详细
> 可以发现 Graphics 占用偏高
> 注意这个 Events 告诉你具体哪个资源，你可以参考

### 包文件大小分析

- 生成 `snapshot.arm64-v8a.json` 文件

```sh
> flutter build apk --analyze-size --target-platform=android-arm64
```

- 分析清单

```
app-release.apk (total compressed)                                          9 MB
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  res/
    mipmap-xxxhdpi-v4                                                       1 KB
  META-INF/
    MANIFEST.MF                                                             2 KB
    kotlinx-coroutines-core.kotlin_module                                   1 KB
    CERT.SF                                                                 3 KB
    kotlin-stdlib.kotlin_module                                             3 KB
    CERT.RSA                                                              1013 B
  lib/
    arm64-v8a                                                               5 MB
    Dart AOT symbols accounted decompressed size                            3 MB
      package:flutter                                                       1 MB
      dart:core                                                           234 KB
      dart:typed_data                                                     193 KB
      dart:ui                                                             170 KB
      dart:collection                                                     117 KB
      dart:async                                                           76 KB
      dart:convert                                                         50 KB
      dart:io                                                              40 KB
      dart:isolate                                                         29 KB
      package:vector_math/
        vector_math_64.dart                                                23 KB
      dart:ffi                                                             11 KB
      dart:developer                                                        8 KB
      package:typed_data/
        src/
          typed_buffer.dart                                                 5 KB
      package:flutter_application_performance/
        main.dart                                                           2 KB
      package:collection/
        src/
          priority_queue.dart                                               2 KB
      dart:vmservice_io                                                    635 B
      dart:mirrors                                                         492 B
      dart:math                                                            475 B
      dart:nativewrappers                                                  186 B
      void/
        <optimized out>                                                     44 B
  assets/
    flutter_assets                                                          3 MB
  kotlin/
    reflect                                                                 1 KB
    collections                                                             1 KB
    kotlin.kotlin_builtins                                                  4 KB
  resources.arsc                                                           24 KB
  AndroidManifest.xml                                                       1 KB
  classes.dex                                                             250 KB
▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒
A summary of your APK analysis can be found at: /Users/ducafecat/.flutter-devtools/apk-code-size-analysis_02.json
```

> 分析结果文件 /Users/ducafecat/.flutter-devtools/apk-code-size-analysis_02.json
> flutter_assets 3 MB , 可以发现这个资源文件有点大了，需要优化
> arm64-v8a 5 MB 核心必须
> Dart AOT symbols accounted decompressed size 3 MB 核心必须

- 上传 `snapshot.arm64-v8a.json` 文件

> 文件位置 `build/flutter_size_04/snapshot.arm64-v8a.json`

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317224706.png)

> 点击分析文件后，可以看到这样的图标，很直观

### Widget Inspector 开启描边功能

> 如果一个 widget 被反复重绘，描边加深

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220317230910.png)

> 可以发现图片的这两张都发紫了
> 你可以采用局部刷新的技巧来优化 `GlobalKey` `状态订阅`

## 总结

通过上面的两个例子，让大家知道如何查看 cpu gpu 的性能，包文件大小分析。

在开发中的一些建议:

- 影响性能分析的原因很多，比如你的手机本身就很卡，内存很小
- 要多次在可疑的地方进行反复测试
- 如果你自己感觉卡的话，可以用排除法，逐步找到原因
- 不要在 build 的时候去做耗时运算
- 资源要瘦身优化，尺寸大小
- 排查的时候要区分清楚 cpu gpu 的问题
- 布局优化用 key 加速性能
- 内存优化 const 实例化，现在 ide 都有提示了
- 高延迟可以 network 查看

end

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
