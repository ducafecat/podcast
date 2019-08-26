---
title: Flutter移动开发 - 04 基础控件 image
date: 2019-05-22 00:39:12
tags: flutter
categories: Flutter移动开发
---

## 本节目标

- image 构造函数的 5 种方式
- 加载图片 Asset、NetworkImage
- 占位图 FadeInImage
- 头像 CircleAvatar
- 圆角 ClipRRect
- 图片 fit 方式

## 1. Image

图片显示组件

支持图像格式 JPEG，PNG，GIF，动画 GIF，WebP，动画 WebP，BMP 和 WBMP

### 1.1 五种构造方式

| 构造                  | 说明                    |
| --------------------- | ----------------------- |
| Image()               | ImageProvider 适配图片  |
| Image.asset           | 加载资源图片            |
| Image.file            | 加载本地图片            |
| Image.network         | 加载网络图片            |
| Image.memory          | 加载 Uint8List 资源图片 |

### 1.2 构造参数

| 参数                              | 说明                                     |
| --------------------------------- | ---------------------------------------- |
| double scale = 1.0                | 缩放                                     |
| semanticLabel                     | 图像的语义描述                           |
| excludeFromSemantics = false      | 是否启用图像的语义描述                   |
| width                             | 宽度                                     |
| height                            | 高度                                     |
| color                             | 使用 colorBlendMode 颜色图像像素混合     |
| colorBlendMode                    | 用于将 color 与此图像组合                |
| fit                               | 图片如何在 Image 控件中显示              |
| alignment = Alignment.center      | 对齐                                     |
| repeat = ImageRepeat.noRepeat     | 重复                                     |
| centerSlice                       | 指定中心区域进行九个补丁图像             |
| matchTextDirection = false        | 是否在 TextDirection 的方向上绘制图像    |
| gaplessPlayback = false           | 当图像提供者发生变化时，是继续显示旧图像 |
| filterQuality = FilterQuality.low | 图像过滤器的质量级别。(渲染模式的质量)   |

### 1.3 混色

- 29 种混合模式

```dart
enum BlendMode {
  clear,src,dst,srcOver,dstOver,srcIn,dstIn,srcOut,dstOut,srcATop,dstATop,xor,plus，modulate,screen,overlay,darken,lighten,colorDodge,colorBurn,hardLight,softLight,difference,exclusion,multiply,hue,saturation,color,luminosity,
}
```

- 主要的混合模式效果如下

![](image-20190523170707924.png)

### 1.4 缩放

enum BoxFit 枚举对象

| 名称    | 说明             |
| ------- | ------------------------------ |
| fill    | 图片按照指定的大小在Image中显示，拉伸显示图片，不保持原比例，填满Image。![](image-20190523170951568.png) |
| contain | 以原图正常显示为目的，如果原图大小大于Image的size，就按照比例缩小原图的宽高，居中显示在Image中。如果原图size小于Image的size，则按比例拉升原图的宽和高，填充Image一边并居中显示。![](image-20190523171033329.png) |
| cover   | 以原图填满Image为目的，如果原图size大于Image的size，按比例缩小，居中显示在Image上。如果原图size小于Image的size，则按比例拉升原图的宽和高，填充Image居中显示。![](image-20190523172626391.png) |
| fitWidth | 以原图正常显示为目的，如果原图宽大小大于（小于）Image的宽，就缩小（放大）原图的宽与Image一致，居中显示在Image中。![](image-20190523172716044.png) |
| fitHeight | 以原图正常显示为目的，如果原图高大小大于（小于）Image的高，就缩小（放大）原图的高与Image一致，居中显示在Image中。![](image-20190523172757661.png) |
| none | 保持原图的大小，显示在Image的中心。当原图的size大于Image的size时，多出来的部分被截掉。![](image-20190523172853657.png) |
| scaleDown | 以原图正常显示为目的，如果原图大小大于Image的size，就按照比例缩小原图的宽高，居中显示在Image中。如果原图size小于Image的size，则不做处理居中显示图片。![](image-20190523172919652.png) |

### 1.5 代码示例

#### 1.5.1 加载资源图片

- 复制文件到目录 `assets/images/react.jpeg`
- 修改 pubspec.yaml

```dart
  # To add assets to your application, add an assets section, like this:
  assets:
    - assets/images/
```

- 读取图片

```dart
import 'package:flutter/material.dart';
import 'package:flutter/gestures.dart';

main(List<String> args) {
  runApp(MaterialApp(
      home: Scaffold(
          appBar: AppBar(title: Text('my app')),
          body: Container(
              child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              Image.asset('assets/images/react.jpeg')
            ],
          )))));
}
```

#### 1.5.2 image 加载网络图片

```dart
Image.network(_imgUrl)
```

#### 1.5.3 NetworkImage 加载网络图片

```dart
Image(image: NetworkImage(_imgUrl))
```

#### 1.5.4 占位图加载网络图片

```dart
FadeInImage(
  fadeInCurve: Curves.bounceIn,
  placeholder: AssetImage(_assetImg),
  image: NetworkImage(_imgUrl))
```

#### 1.5.5 头像

```dart
CircleAvatar(
  backgroundColor: Colors.brown.shade800,
  child: Text('圆角图片'),
  backgroundImage: AssetImage(_assetImg),
  radius: 50.0)
```

#### 1.5.6 图标

```dart
ImageIcon(
  NetworkImage(_imgUrl),
  size: 100,
)
```

#### 1.5.7 圆角矩形

```dart
ClipRRect(
  child: Image.network(_imgUrl),
  borderRadius: BorderRadius.all(Radius.circular(20)),
)
```

#### 1.5.8 椭圆形

```dart
ClipOval(
  child: Image.network(
    _imgUrl,
    scale: 8.5,
  ),
)
```

#### 1.5.9 混色

```dart
Image.asset(
  _assetHeaderImg,
  color: Colors.amber,
  colorBlendMode: BlendMode.dstATop,
)
```

#### 1.5.10 缩放

```dart
Image.asset(
  _assetImg,
  width: 400,
  height: 50,
  fit: BoxFit.cover,
)
```

## 代码

https://github.com/ducafecat/flutter-learn/tree/master/p04_image

## 参考

- [Image class](https://api.flutter.dev/flutter/dart-ui/Image-class.html)
- [NetworkImage class](https://api.flutter.dev/flutter/painting/NetworkImage-class.html)
- [FadeInImage class](https://api.flutter.dev/flutter/widgets/FadeInImage-class.html)
- [ClipRRect class](https://api.flutter.dev/flutter/widgets/ClipRRect-class.html)
- [ClipOval class](https://api.flutter.dev/flutter/widgets/ClipOval-class.html)
- [ImageIcon class](https://api.flutter.dev/flutter/widgets/ImageIcon-class.html)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
