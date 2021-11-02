---
title: Flutter 保护你的APP数据安全
tags: flutter
categories: 译文
date: 2021-11-04 00:00:00
---

![](2021-11-02-21-21-20.png)

## 原文

> https://medium.com/@mohammadEzzo/securing-flutter-apps-3cd1aedda088

## 参考

- https://www.guardsquare.com/en/blog/iOS-SSL-certificate-pinning-bypassing
- https://www.freecodecamp.org/news/openssl-command-cheatsheet-b441be1e8c4a/
- https://www.freecodecamp.org/news/what-is-tls-transport-layer-security-encryption-explained-in-plain-english/

## 正文

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/31c2f898600a9e90531c31bdf1df4d96c9326f1550634b381d917cf955969c25.jpeg)

目前，大多数应用程序都包含支付或存储一些重要的个人数据，这增加了数据被攻击者利用或暴露的风险。

在这篇文章中，我将谈论最有效的做法，以尽量减少 Flutter 应用程序中任何安全漏洞的风险，并设置尽可能多的路障，以任何攻击者的方式。当然，这并不能保证你的应用程序是 100% 安全的。

## 让我们开始

### 保护通信层

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/9eac2ec7d4330473eeb4e74b0ea31ce9ff0cdc4358d572e4b9f65c605b08fab2.png)

https://www.guardsquare.com/en/blog/ios-ssl-certificate-pinning-bypassing

当攻击者锁定一个应用程序时，首先要做的事情之一就是查看他们是否 **能拦截在应用程序和服务器后端之间传递的任何数据** 。

#### 1- 采用高度加密:

您可以通过使用 [SSL](https://www.freecodecamp.org/news/openssl-command-cheatsheet-b441be1e8c4a/) 和 [TLS](https://www.freecodecamp.org/news/what-is-tls-transport-layer-security-encryption-explained-in-plain-english/) 等协议来实现这一点，这些协议很容易添加到您的代码中，并且很难妥协。

如果您正在处理特别敏感的数据，您甚至可能需要更进一步，在应用程序中构建一个类似 vpn 的解决方案。

#### 2- 限制网络流量

将网络流量或连接限制到不安全端点的一种方法是显式地将域名列为白名单。

要做到这一点，在 flutter 应用程序，我们需要为每个平台做一些步骤:

**_android :_**

go to the android folder and create this file under

进入 android 文件夹，创建下面的文件

```dart
res/xml/network_security_config.xml
```

然后复制这个并将其添加到创建的 xml 文件中:

```dart
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config>
        <domain includeSubdomains="true">YOURDOMAIN.com</domain>
        <trust-anchors>
            <certificates src="@raw/YOURCERTIFICATE"/>
        </trust-anchors>
    </domain-config>
</network-security-config>
```

**_for ios:_**

add this to the info.plist file:

把这个添加到 info.plist 文件:

```dart
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <false/>
  <key>NSExceptionDomains</key>
  <dict>
    <key>YOURDOMAIN.com</key>
    <dict>
      <key>NSIncludesSubdomains</key>
      <true/>
      <key>NSExceptionAllowsInsecureHTTPLoads</key>
      <true/>
    </dict>
  </dict>
</dict>
```

然后用你的服务器域名替换 `YOURDOMAIN.com` 。

这样做将确保您的应用程序不被允许与任何其他域通信。

#### 3- 认可证书

SSL pinning 解决了 MITM (Man In The Middle)攻击。

怎么做到的？

在简单的语言中，您将从后端开发人员获得一个服务器证书文件，并将证书钉在每个 API 调用中。因此，HTTP 客户端将把这个证书作为一个可信赖的证书。现在，如果出现了 MITM，并且应用程序得到了一些错误的证书，那么由于握手错误，API 调用将被中断。

**所以让我们实现这个 Flutter :**

最有可能的证书延长将是。“.cef” 但是这个扩展在 flutter 中不可读，所以我们需要将其转换为 “.pem” 使用这个命令。

```dart
_openssl x509 -inform der -in_ Certificate_.cer -out_ Certificate_.pem_
```

证书是您可以自己使用的文件名。

然后将证书作为资产添加到 `pubspec.yaml`。

现在使用 **Dio** 包，我们可以管理应用程序中的所有请求:

```dart
final dio = Dio(); ByteData bytes = await rootBundle.load('assets/Certificate.pem');
(dio.httpClientAdapter as DefaultHttpClientAdapter).onHttpClientCreate  = (client) {
  SecurityContext sc = SecurityContext();
  sc.setTrustedCertificatesBytes(bytes.buffer.asUint8List());
  HttpClient httpClient = HttpClient(context: sc);
  return httpClient;
};
```

在这段代码中，我们从资产中读取证书，并将其作为可信证书添加到 dio 实例的 http 客户端。

现在，当使用这个 dio 实例向另一个服务器发出任何请求时，由于服务器的证书无效，我们将得到一个握手错误。

#### 4-使身份认证刀枪不入

除了你的应用程序的数据流，下一个最常见的攻击载体是它的认证方法的任何弱点。

因此，与服务器进行双因素身份验证是必要的，也是值得实现的。

除此之外，你还需要注意如何处理像钥匙交换这样的事情。至少，您应该使用加密来保证这些事务的安全。

- 到目前为止，我们已经尽力保护与服务器的传输层。

现在我们开始保护应用本身。

### 保护申请

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/05d23cfca0c5349f486b44fc2f22b8056895d166e3e624d78e2a31154dc24b32.jpeg)

基本了解 Android app. Source — [Pranay Airan](https://www.slideshare.net/PranayAiran1/reverse-engineering-android-apps).

##### 1- 模糊编码

编译后的二进制文件和应用程序的代码可以被逆向设计。可以公开的内容包括字符串、方法和类名以及 API 键。这些数据要么是原始形式，要么是纯文本形式。

- 你可以做的是使用 `--obfuscate` 参数时，建立您的 apk。

```dart
flutter build appbundle --obfuscate --split-debug-info=/<directory>
```

从本土的角度来说，你需要通过以下方式来处理这个问题:

**android**

在 `/android/app/build.gradle` 文件中，添加以下内容:

```dart
android {
	...
	buildTypes {
		release {
			signingConfig signingConfigs.release
			minifyEnabled true
			useProguard true
			proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
		}
	}
}
```

在 `/android/app/proguard-rules.pro` 中创建一个 ProGuard 配置文件:

```dart
# Flutter
-keep class io.flutter.app. { *; }
-keep class io.flutter.plugin.  { *; }
-keep class io.flutter.util.  { *; }
-keep class io.flutter.view.  { *; }
-keep class io.flutter.  { *; }
-keep class io.flutter.plugins.  { *; }
```

使用 ProGuard，它不仅可以模糊你的代码，还可以帮助你缩小 Android 应用程序的大小。

**iOS**

如果你使用 Objective-C 或 Swift 来编译 iOS，编译器会去掉这些符号并对你的代码进行优化，这就使得攻击者很难读取你的代码的编译输出。

还有一些付费工具可以帮助你模糊代码: [iXGuard](https://betterprogramming.pub/getting-started-with-ixguard-an-obfuscation-app-shrinking-tool-85e1342a5572) 和 [Vermatrix](https://www.verimatrix.com/solutions/application-shielding/code-obfuscation).

#### 2- 越狱和植根设备

越狱的 iOS 和安卓设备有更多的特权，可能会给用户的设备带来恶意软件，从而绕过设备的正常运行。

`flutter_jailbreak_detection` 是一个软件包，它可以帮助你检测你的应用是否正在一个越狱或根植的设备上运行,

它在 Android 上使用 [RootBeer](https://github.com/scottyab/rootbeer) on Android, and [DTTJailbreakDetection](https://github.com/thii/DTTJailbreakDetection) ，在 iOS 上使用 [DTTJailbreakDetection](https://github.com/thii/DTTJailbreakDetection) 。

而且很容易使用:

```dart
import 'package:flutter_jailbreak_detection/flutter_jailbreak_detection.dart';

bool jailbroken = await FlutterJailbreakDetection.jailbroken;
bool developerMode = await FlutterJailbreakDetection.developerMode; _// android only._
```

https://pub.dev/packages/flutter_jailbreak_detection

#### 3-保护用户资料

为了存储敏感的用户数据，你不应该使用共享首选项或 sqflite，因为它很容易在任何设备上打开，因为你需要对存储的数据进行加密，你可以使用 `flutter_secure_storage` 。

https://pub.dev/packages/flutter_secure_storage

这个软件包使用了 Android 的 Keystore 和 iOS 的 Keychains。

> 还值得设置一个周期性时间，以便自动清除过期的数据缓存。

#### 4. 使用本地身份验证

假设用户手机被盗，并且您的应用程序已经安装在手机上，并且它有一些支付信息:)

为了防止任何访问您的应用程序，您应该使用生物特征识别认证使用此软件包。

https://pub.dev/packages/local_auth

#### 5- 背景快拍预防

当一个应用程序被背景化时，操作系统会获取任务切换器中最后一个可见状态的快照。因此，防止后台快照捕捉到账户余额和付款细节是非常需要的。

用这个 `secure_application` 包就能解决这个问题

https://pub.dev/packages/secure_application

他的插件允许你保护你的应用程序内容不被点击查看。

### 总结

最终，作为一个开发人员，这就是所有人对你的要求。

我还想提到“如何保护您的 Flutter 应用程序?” 是移动应用程序开发者在求职面试中最常见的问题，所以我希望这将是有用的。

---

© 猫哥

- https://ducafecat.tech/

- https://github.com/ducafecat

- 微信群 ducafecat

- b 站 https://space.bilibili.com/404904528

![](https://ducafecat.tech/img/public-qrcode.png)

## 往期

### 开源

#### GetX Quick Start

https://github.com/ducafecat/getx_quick_start

#### 新闻客户端

https://github.com/ducafecat/flutter_learn_news

### strapi 手册译文

https://getstrapi.cn

### 微信讨论群 ducafecat

### 系列集合

#### 译文

https://ducafecat.tech/categories/%E8%AF%91%E6%96%87/

#### 开源项目

https://ducafecat.tech/categories/%E5%BC%80%E6%BA%90/

#### Dart 编程语言基础

https://space.bilibili.com/404904528/channel/detail?cid=111585

#### Flutter 零基础入门

https://space.bilibili.com/404904528/channel/detail?cid=123470

#### Flutter 实战从零开始 新闻客户端

https://space.bilibili.com/404904528/channel/detail?cid=106755

#### Flutter 组件开发

https://space.bilibili.com/404904528/channel/detail?cid=144262

#### Flutter Bloc

https://space.bilibili.com/404904528/channel/detail?cid=177519

#### Flutter Getx4

https://space.bilibili.com/404904528/channel/detail?cid=177514

#### Docker Yapi

https://space.bilibili.com/404904528/channel/detail?cid=130578
