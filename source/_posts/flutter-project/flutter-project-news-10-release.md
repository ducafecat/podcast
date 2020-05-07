---
title: Flutter 实战从零开始 新闻客户端 - 10 编译发布正式版
date: 2020-05-05 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

# 本节目标

- 编译 build releae
- 程序瘦身
- 混淆程序
- 修改程序名称
- 制作图标
- 制作启动画面

## 正文

### 1. APP 图标

#### 规格说明

https://developer.android.com/google-play/resources/icon-design-specifications

https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/app-icon/

#### 图标尺寸

android 512x512

ios 1024x1024

#### 在线工具

https://www.designevo.com/cn/logo-maker/

#### flutter_launcher_icons 插件

https://pub.dev/packages/flutter_launcher_icons

#### pubspec.yaml

```yaml
dev_dependencies:
  # icons
  flutter_launcher_icons: ^0.7.5

flutter_icons:
  android: "launcher_icon"
  ios: true
  image_path: "assets/icons/logo-1024.png"
```

#### 生成图标

```sh
flutter pub run flutter_launcher_icons:main
```

#### 图标目录

android/app/src/main/res

ios/Runner/Assets.xcassets/AppIcon.appiconset

### 2. 启动图片

#### 规格说明

https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/#device-screen-sizes-and-orientations

https://developer.android.com/about/dashboards/index.html#Screens

https://uiiiuiii.com/screen/

#### 图片尺寸

iPhone XS Max 1242px × 2688px

android xxhdpi xhdpi

#### 在线工具

https://hotpot.ai/icon_resizer

### 3. Android 发布

#### 证书签名说明

https://developer.android.com/studio/publish/app-signing?hl=zh-cn

#### 生成证书

```sh
# 进入目录 android/app/
keytool -genkey -v -keystore ./key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key

# 输出文件
android/app/key.jks
```

#### Gradle 配置

- android/gradle.properties

  ```properties
  android.enableAapt2=false # 不检测依赖资源
  ```

* android/key.properties

  ```properties
  storePassword=123456
  keyPassword=123456
  keyAlias=key
  storeFile=./key.jks
  ```

- android/app/build.gradle

  ```gradle
  // 定义属性读取对象，读取 android/key.properties
  def keystoreProperties = new Properties()
  def keystorePropertiesFile = rootProject.file('key.properties')
  if (keystorePropertiesFile.exists()) {
      keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
  }

  android {
      compileSdkVersion 28

      ...

      // 签名配置
      signingConfigs {
          release {
              keyAlias keystoreProperties['keyAlias']
              keyPassword keystoreProperties['keyPassword']
              storeFile file(keystoreProperties['storeFile'])
              storePassword keystoreProperties['storePassword']
          }
      }

      buildTypes {

          // 发布配置
          release {
              signingConfig signingConfigs.release
          }
      }
  }
  ```

#### 修改版本号

- pubspec.yaml

```
version: 1.0.0+1
```

#### 修改程序名称

- android/app/src/main/AndroidManifest.xml

  ```xml
      <application
          android:name="io.flutter.app.FlutterApplication"
          android:label="猫哥新闻"
          android:icon="@mipmap/launcher_icon">
  ```

#### 设置网络权限

- android/app/src/main/AndroidManifest.xml

  ```xml
      </application>

      <uses-permission android:name="android.permission.INTERNET" />
  </manifest>
  ```

#### 编译打包

```shell
flutter build apk --split-per-abi
```

#### 输出目录

```sh
✓ Built build/app/outputs/apk/release/app-armeabi-v7a-release.apk (7.2MB).
✓ Built build/app/outputs/apk/release/app-arm64-v8a-release.apk (7.4MB).
✓ Built build/app/outputs/apk/release/app-x86_64-release.apk (7.6MB).
```

#### 混淆编译

https://github.com/flutter/flutter/wiki/Obfuscating-Dart-Code

- android/gradle.properties

  ```properties
  extra-gen-snapshot-options=--obfuscate
  ```

- android/proguard-rules.pro

  ```properties
  #Flutter Wrapper
  -dontwarn io.flutter.**
  -keep class io.flutter.app.** { *; }
  -keep class io.flutter.plugin.**  { *; }
  -keep class io.flutter.util.**  { *; }
  -keep class io.flutter.view.**  { *; }
  -keep class io.flutter.**  { *; }
  -keep class io.flutter.plugins.**  { *; }
  ```

- android/app/build.gradle

  ```gradle
      buildTypes {
          release {
              signingConfig signingConfigs.release

              minifyEnabled true  //资源压缩设置
              useProguard true    //代码压缩设置

              //读取代码压缩配置文件
              proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

          }
      }
  ```

- 编译

```sh
flutter build apk --split-per-abi
```

#### 启动页

- 图片
  ![](2020-05-07-21-56-57.png)

- android/app/src/main/res/values/colors.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <resources>
      <color name="cyan">#deecec</color>
  </resources>
  ```

- android/app/src/main/res/drawable/launch_background.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <!-- Modify this file to customize your launch splash screen -->
  <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item android:drawable="@color/cyan" />
      <item>
          <bitmap
              android:gravity="center"
              android:src="@mipmap/launch_image" />
      </item>
  </layer-list>
  ```

### 4. IOS 发布

#### 启动页

![](2020-05-07-21-52-25.png)

#### 修改程序名称

![](2020-05-07-21-51-46.png)

## 资源

### 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

### 蓝湖设计稿（加微信给授权 ducafecat）

https://lanhuapp.com/url/wbhGq

### YAPI 接口管理

http://yapi.demo.qunar.com/

### 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.10

### 参考

https://flutter.dev/docs/deployment/android

https://flutter.dev/docs/deployment/ios

https://flutter.dev/docs/deployment/obfuscate

https://github.com/flutter/flutter/wiki/Obfuscating-Dart-Code

https://pub.dev/packages/flutter_launcher_icons

https://developer.android.com/google-play/resources/icon-design-specifications

https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/app-icon/

https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/adaptivity-and-layout/#device-screen-sizes-and-orientations

https://developer.android.com/about/dashboards/index.html#Screens

https://uiiiuiii.com/screen/

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
