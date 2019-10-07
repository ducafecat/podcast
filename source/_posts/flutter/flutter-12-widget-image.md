---
title: Flutter 零基础入门中文教学 - 12 基础组件 Image
date: 2019-10-7 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

![](2019-10-07-16-37-56.png)

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

- 五种构造方式

| 构造                  | 说明                    |
| --------------------- | ----------------------- |
| Image()               | ImageProvider 适配图片  |
| Image.asset           | 加载资源图片            |
| Image.file            | 加载本地图片            |
| Image.network         | 加载网络图片            |
| Image.memory          | 加载 Uint8List 资源图片 |

- 构造参数

```dart
//通过ImageProvider来加载图片
const Image({
    Key key,
    // ImageProvider，图像显示源
    @required this.image,
    this.semanticLabel,
    this.excludeFromSemantics = false,
    //显示宽度
    this.width,
    //显示高度
    this.height,
    //图片的混合色值
    this.color,
    //混合模式
    this.colorBlendMode,
    //缩放显示模式
    this.fit,
    //对齐方式
    this.alignment = Alignment.center,
    //重复方式
    this.repeat = ImageRepeat.noRepeat,
    //当图片需要被拉伸显示的时候，centerSlice定义的矩形区域会被拉伸，类似.9图片
    this.centerSlice,
    //类似于文字的显示方向
    this.matchTextDirection = false,
    //图片发生变化后，加载过程中原图片保留还是留白
    this.gaplessPlayback = false,
    //图片显示质量
    this.filterQuality = FilterQuality.low,
  })

// 加载网络图片，封装类：NetworkImage
Image.network(
    //路径
    String src,
   {
    Key key,
    //缩放
    double scale = 1.0,
    this.semanticLabel,
    this.excludeFromSemantics = false,
    this.width,
    this.height,
    this.color,
    this.colorBlendMode,
    this.fit,
    this.alignment = Alignment.center,
    this.repeat = ImageRepeat.noRepeat,
    this.centerSlice,
    this.matchTextDirection = false,
    this.gaplessPlayback = false,
    this.filterQuality = FilterQuality.low,
    Map<String, String> headers,
  })

// 加载本地File文件图片，封装类：FileImage
Image.file(
    //File对象
    File file,
  {
    Key key,
    double scale = 1.0,
    this.semanticLabel,
    this.excludeFromSemantics = false,
    this.width,
    this.height,
    this.color,
    this.colorBlendMode,
    this.fit,
    this.alignment = Alignment.center,
    this.repeat = ImageRepeat.noRepeat,
    this.centerSlice,
    this.matchTextDirection = false,
    this.gaplessPlayback = false,
    this.filterQuality = FilterQuality.low,
  })

// 加载本地资源图片,例如项目内资源图片
// 需要把图片路径在pubspec.yaml文件中声明一下，如：
// assets:
//      - packages/fancy_backgrounds/backgrounds/background1.png
// 封装类有：AssetImage、ExactAssetImage
Image.asset(
    //文件名称，包含路径
    String name,
  {
    Key key,
    // 用于访问资源对象
    AssetBundle bundle,
    this.semanticLabel,
    this.excludeFromSemantics = false,
    double scale,
    this.width,
    this.height,
    this.color,
    this.colorBlendMode,
    this.fit,
    this.alignment = Alignment.center,
    this.repeat = ImageRepeat.noRepeat,
    this.centerSlice,
    this.matchTextDirection = false,
    this.gaplessPlayback = false,
    String package,
    this.filterQuality = FilterQuality.low,
  })

// 加载Uint8List资源图片/从内存中获取图片显示
// 封装类：MemoryImage
Image.memory(
    // Uint8List资源图片
    Uint8List bytes,
  {
    Key key,
    double scale = 1.0,
    this.semanticLabel,
    this.excludeFromSemantics = false,
    this.width,
    this.height,
    this.color,
    this.colorBlendMode,
    this.fit,
    this.alignment = Alignment.center,
    this.repeat = ImageRepeat.noRepeat,
    this.centerSlice,
    this.matchTextDirection = false,
    this.gaplessPlayback = false,
    this.filterQuality = FilterQuality.low,
  })
```

29 种混合模式

```dart
enum BlendMode {
  clear,src,dst,srcOver,dstOver,srcIn,dstIn,srcOut,dstOut,srcATop,dstATop,xor,plus，modulate,screen,overlay,darken,lighten,colorDodge,colorBurn,hardLight,softLight,difference,exclusion,multiply,hue,saturation,color,luminosity,
}
```

- 主要的混合模式效果如下

![](image-20190523170707924.png)

- 缩放

enum BoxFit 枚举对象

| 名称      | 说明                                                                                                                                                                                                                         |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fill      | 图片按照指定的大小在 Image 中显示，拉伸显示图片，不保持原比例，填满 Image。![](image-20190523170951568.png)                                                                                                                  |
| contain   | 以原图正常显示为目的，如果原图大小大于 Image 的 size，就按照比例缩小原图的宽高，居中显示在 Image 中。如果原图 size 小于 Image 的 size，则按比例拉升原图的宽和高，填充 Image 一边并居中显示。![](image-20190523171033329.png) |
| cover     | 以原图填满 Image 为目的，如果原图 size 大于 Image 的 size，按比例缩小，居中显示在 Image 上。如果原图 size 小于 Image 的 size，则按比例拉升原图的宽和高，填充 Image 居中显示。![](image-20190523172626391.png)                |
| fitWidth  | 以原图正常显示为目的，如果原图宽大小大于（小于）Image 的宽，就缩小（放大）原图的宽与 Image 一致，居中显示在 Image 中。![](image-20190523172716044.png)                                                                       |
| fitHeight | 以原图正常显示为目的，如果原图高大小大于（小于）Image 的高，就缩小（放大）原图的高与 Image 一致，居中显示在 Image 中。![](image-20190523172757661.png)                                                                       |
| none      | 保持原图的大小，显示在 Image 的中心。当原图的 size 大于 Image 的 size 时，多出来的部分被截掉。![](image-20190523172853657.png)                                                                                               |
| scaleDown | 以原图正常显示为目的，如果原图大小大于 Image 的 size，就按照比例缩小原图的宽高，居中显示在 Image 中。如果原图 size 小于 Image 的 size，则不做处理居中显示图片。![](image-20190523172919652.png)                              |

## 示例

- pubspec.yaml

```yaml
assets:
  - assets/images/
```

- main.dart

```dart
// assets
Text('assets'),
Image.asset(_assetImg),

// 网络读取
Text('网络读取'),
Image.network(_imgUrl),

// NetworkImage
Text('NetworkImage'),
Image(image: NetworkImage(_imgUrl)),

// 占位图
Text('占位图'),
FadeInImage(
    fadeInCurve: Curves.bounceIn,
    placeholder: AssetImage(_assetImg),
    image: NetworkImage(_imgUrl)),

// 原型头像
Text('原型头像'),
CircleAvatar(
    backgroundColor: Colors.brown.shade800,
    child: Text('圆角图片'),
    backgroundImage: AssetImage(_assetHeaderImg),
    radius: 50.0),

// 图标
Text('图标'),
ImageIcon(
  NetworkImage(_imgUrl),
  size: 100,
),

// ClipRRect 圆角
Text('ClipRRect 圆角'),
ClipRRect(
  child: Image.network(_imgUrl),
  borderRadius: BorderRadius.all(Radius.circular(20)),
),

// 圆角矩形框
Text('圆角矩形框'),
Container(
  width: 200,
  height: 80,
  decoration: BoxDecoration(
    shape: BoxShape.rectangle,
    borderRadius: BorderRadius.circular(10.0),
    image: DecorationImage(
        image: NetworkImage(_imgUrl), fit: BoxFit.cover),
  ),
),

// 椭圆图
Text('椭圆图'),
ClipOval(
  child: Image.network(
    _imgUrl,
    scale: 8.5,
  ),
),

// 混色
Text('混色'),
Image.asset(
  _assetHeaderImg,
  color: Colors.amber,
  colorBlendMode: BlendMode.dstATop,
),

// 裁剪
Text('裁剪'),
Image.asset(
  _assetImg,
  width: 400,
  height: 50,
  fit: BoxFit.cover,
),
```

## 代码

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
