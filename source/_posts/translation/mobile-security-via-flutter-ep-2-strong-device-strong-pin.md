---
title: Flutter 移动安全 ー Ep.2 Strong Device/ Strong Pin
tags: flutter
categories: 译文
date: 2021-04-30 00:00:00
---

![](2021-04-30-09-24-45.png)

## 原文

> https://medium.com/kbtg-life/mobile-security-via-flutter-ep-2-strong-device-strong-pin-70b4322bffc2

## 前言

![](2021-04-30-08-29-29.png)

在本期节目中，我们将进一步强化您的移动应用程序。“但是怎么做呢?”你可能会好奇。让我们把它比作建造一座房子。为了确保它配备了最高级别的安保系统，你可能需要在房子周围安装所有的警报器和摄像头。但是如果你碰巧把钥匙留在门口，这些都不重要了！对于安全性，我们需要考虑一个坏黑客可能选择攻击的每一种可能性，所以这不仅仅是一个解决方案，然后，一切都完成了。我们必须确保外部和内部的应用程序都是安全的。

上一集，我们讨论了 SSL。你可以把它想象成房子周围高高的篱笆。虽然爬过去比较困难，但还是可以管理的。因此，在本期节目中，我们将确保没有人意外地把钥匙落在门口。

https://medium.com/kbtg-life/mobile-security-via-flutter-ep-1-ssl-pinning-c57f18b711f6

在 iOS/Android 中，已经有一个本地安全 API 来保护他们的应用程序不受外界的攻击，所以我们将使用这个 API 来实现 Flutter。让我们来看看什么是必要的，什么是美好的。

## 参考

- https://pub.dev/packages/flutter_secure_storage
- https://www.macrumors.com/2020/12/16/ios-14-installed-81-percent-iphones/
- https://pub.dev/packages/flutter_jailbreak_detection
- https://developer.apple.com/documentation/localauthentication/logging_a_user_into_your_app_with_face_id_or_touch_id
- https://frida.re/
- https://developer.android.com/training/sign-in/biometric-auth#crypto
- https://medium.com/androiddevelopers/using-biometricprompt-with-cryptoobject-how-and-why-aace500ccdb7
- https://github.com/zionspike/android-FingerAuthenSample-Asym
- https://www.researchgate.net/publication/313823128_Understanding_Human-Chosen_PINs_Characteristics_Distribution_and_Security

## 正文

### 强制性

#### 安全的数据存储

基本上有两件事你需要记住:

- 不要在应用程序中保存任何安全信息，比如名字、姓氏、电子邮件、用户名、密码、公民身份，或任何能让黑客追踪并查出用户身份的信息。如果你真的需要保存它，比如一个令牌或者任何你想用来改善用户体验的东西，确保你把它保存在两个操作系统平台都提供的安全的数据存储中。为此，我使用了这个库。

https://pub.dev/packages/flutter_secure_storage

对于 iOS，他们使用 Keychain，即使你删除了应用程序也不会被删除，而对于安卓，他们使用 KeyStore 来存储解密保存的数据的密钥。

- 额外的安全层总是加密的安全信息，即使 iOS/Android 已经有了安全存储。这是因为在未来，可能会有一种工具，可以让黑客破解设备上的加密。既然我们在讨论加密的话题，我们也需要讨论一下密钥。它应该是动态的和唯一的每个用户，所以我们决定使用用户自己的密码，因为我们可以肯定，只有应用程序所有者知道如何解密它与正确的密码和访问所有保存的数据。

然而，这意味着密码被秘密保存在设备上。如果一个黑客暴力破解了一个密码，他们就会知道这是正确的密码，这可能会更加危险。他们将有足够的时间解密，因为它是在设备上，所以我们的解决方案是使用另一层保护。我们允许黑客使用任何密钥解密，这样他们就不知道哪个是正确的密钥来调用我们的服务器。如果他们输入错误的密码 3 次，后端会自动锁定他们。这将解决这个问题。这里不提供使用任何密钥进行解密的解决方案。

#### 关闭生产中的日志

开发人员需要一个日志来查看他们的代码是否正常工作。这对于非生产环境来说没有问题，但是对于生产环境来说，您需要关闭它以防止任何人看到它。为此，我通过在 main.dart 中调用以下代码来覆盖 debugPrint 函数

```
debugPrint = (String message, {int wrapWidth}) {};
```

这意味着如果我们使用 debugPrint，它将不会打印任何东西。我把它划分为 main_dev。飞镖和主电极。然后把这个函数放在 main_prod 中。这样我们就看不到任何生产日志了。至于非刺激性构建，您可以就这样保留它。没有必要添加任何东西。我们为什么一定要关掉这根木头？这是因为我们不想让任何人看到幕后的应用程序。不要给黑客任何他们下一步行动应该是什么的线索。

从现在开始只使用 debugPrint 而不是 print

#### 不支持旧版本操作系统

我们必须把这个设置成本地，而不是 Flutter。我并不是真的担心 iOS，因为 iOS 用户倾向于根据本文的采用率频繁更新操作系统。

https://www.macrumors.com/2020/12/16/ios-14-installed-81-percent-iphones/

在 iOS 14 发布后仅仅 3 个月，81% 的设备都更新了他们的操作系统。请记住，安全就像猫和老鼠，您需要一直远离潜在的威胁。我们不应该因为操作系统的安全漏洞而尝试支持过时的操作系统版本。在我看来，只支持最新版本或者更早的版本是可以的。例如，现在我们有 iOS 14，所以我们应该只支持 iOS 12,13 和 14。这将允许 98% 的用户使用你的应用程序。我们可以在 Xcode 设定一个最低目标来控制它。

与此同时，Android 是开源的，这意味着谷歌无法控制它。因此，收养率非常低。看看 Android 的公告，看看哪些 Android 操作系统正在变得过时，没有更多的安全补丁。现在他们仍然支持 Android 8.0，所以我们可能不得不将 minSdkVersion 的目标定为 build.gradle 为 26。

简而言之，iOS = 12 及以上/Android = 26 及以上

#### 只在你需要的时候请求许可

所有开发人员都应该知道的一件基本事情是，总是在需要时请求权限。不要从一开始就要求许可，只要求你需要的东西。例如，一些应用程序可能会要求访问您的 GPS 位置，即使没有任何功能需要它。你永远不应该这样做，因为两件事。首先，你在收集不必要的数据，这些数据是用户的私人数据。其次，如果这些数据没有被正确实现，黑客也可以访问这些数据。因此，为了安全起见，只问你需要什么和什么时候需要它。当用户第一次打开应用程序时，你不希望用户体验是 10 个弹出窗口。用户将离开，再也不会回来。

#### 越狱检测

对于 Flutter，我们使用了原生的越狱检测填充，比如用于安卓系统的 Rootbeer 和用于 iOS 系统的 DTTJailbreakDetection。这两个都是著名的。对于 Flutter，我使用这个库。

https://pub.dev/packages/flutter_jailbreak_detection

尽管对于有经验的黑客来说，它可能还不够强大，但至少我们有东西可以保护我们的应用免受没有经验的黑客的攻击。他们在商业 SDK 中有几个解决方案来保护这一个，但是他们不是强制性的。除非你的应用需要高安全级别，否则 Flutter 越狱检测应该足够了。

#### 利用密码技术进行生物测定

目前，许多应用程序为了更好的用户体验而实现了生物特征识别认证。然而，他们中的大多数人只是相信 iOS/Android，我并不真的推荐这样做。首先，我们可能会在 Swift 中看到这样的代码，甚至苹果也推荐这样的代码。

https://developer.apple.com/documentation/localauthentication/logging_a_user_into_your_app_with_face_id_or_touch_id

```
let reason = “Log in to your account”
context.evaluatePolicy(.deviceOwnerAuthentication, localizedReason: reason ) { success, error in
   if success {
       // Login Succeed, do something next
   } else {
       // Failed, somebody else!!
   }
}
```

每个人都会相信他们的设备。为什么不呢? 嗯，实际上有一个工具叫做 Frida 脚本。

https://frida.re/

浏览之后，你会发现使用这个工具绕过生物计量学工具是多么容易。

这里的教训是 DO n’t just trust boolean from devices。这里有一个来自谷歌的教程，其他人已经在 Android 版本中实现了它。

https://developer.android.com/training/sign-in/biometric-auth#crypto

https://medium.com/androiddevelopers/using-biometricprompt-with-cryptoobject-how-and-why-aace500ccdb7

https://github.com/zionspike/android-FingerAuthenSample-Asym

这是生物识别和密码复查的结合。您可以使用同样的概念扑动。也就是说，如果你的应用程序没有任何金融或在线支付功能，你可以跳过这一部分。

#### 不要用纯文本发送密码

有些人可能会说 HTTPS 是足够安全的，为什么我们必须关心以纯文本发送密码？事实上，黑客可以使用 MITM (中间人攻击文档)以纯文本形式轻松检索你的信息。这就是为什么我们应该在离开移动应用程序之前对密码进行修改，以防黑客窃听，最好的方法就是用盐来修改密码。

- hash(password + dynamic salt)

盐应该在短时间内频繁更换，我们可以做一些基本的事情。

- hash(password + userID)

尽管用户 ID 是唯一的和动态的，但它并不总是变化的。尽管如此，对于基本安全来说，这是可以接受的。然而，如果你想要实现完全的安全性，你必须找到其他一直在改变的东西，并且使用缓慢的散列算法，比如 Bcrypt 而不是 SHA256，因为它们需要特殊的硬件来破解。

### 可选，最好去做

这部分是针对需要额外安全性的金融或银行应用程序。

#### 反调试

对于安卓系统，我们将其分为多个功能

- 关闭 Debuggable 在 buildTypes 部分中添加此标志以 build.gradle。将 release 设置为 false，debug 设置为 true，或者如果需要，可以将两者都设置为 false。

```
buildTypes {
    release {
        debuggable false
        signingConfig signingConfigs.release
        minifyEnabled true
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }

    debug {
        debuggable true
        signingConfig signingConfigs.release
    }

}
```

- 阻止调试器

```
// Open ADB Debugging
if (Settings.Secure.getInt(this.applicationContext.contentResolver, Settings.Global.ADB_ENABLED, 0) == 1) {

}

// Check by using `adb shell getprop ro.crypto.type`
if ((applicationContext.getSystemService(Context.DEVICE_POLICY_SERVICE) as DevicePolicyManager).storageEncryptionStatus == DevicePolicyManager.ENCRYPTION_STATUS_UNSUPPORTED) {

}

// flag debuggable in gradle is true
if ((0 != applicationInfo.flags and ApplicationInfo.FLAG_DEBUGGABLE || BuildConfig.DEBUG))  {

}

// Use Debugger in Android Studio to connect for getting log
if (Debug.isDebuggerConnected() || Debug.waitingForDebugger()) {

}
```

您可以复制上面的代码并将其粘贴到 MainActivity.kt。如果是真的，你可以把用户踢出去或者做任何你想做的事情。请允许我解释一下。

- 打开 ADB 调试防止用户打开 ADB 模式。虽然这没有必要，因为大多数 Android 开发人员总是在需要测试应用程序的时候打开它。这取决于你是否祝酒消息警告用户或评论它出来。

- 不支持 ENCRYPTION _ status _ unsupported 以检查设备是否支持存储加密。在安卓系统中，默认情况下它应该是开启的。如果它被关闭了，那么他们的设备就出了问题，因为用户通常不能自己做到这一点。为了防止上述风险，我们只是不允许任何人使用它。

- ApplicationInfo.FLAG_DEBUGGABLE 检查我们在 Gradle 添加的标志调试是否为 true。如果没有，不要让用户使用它。和上一个一样，这是用户无法改变的东西，除非有人反向工程你的应用程序，并打包为 APK 再次。

- 来检查你的应用程序是否连接到了 Android Studio 调试器。如果是，不要让用户使用它。

- 至于 iOS，现在还没有简单的方法来实现它，但是你可以找到一些商业 SDK 来帮助你实现它。

#### 检查设备是否有安全访问

有些用户可能决定不在他们的设备上安装针、手指扫描或面部扫描。如果有人偷了他们的设备，他们只需要解锁就可以了。拥有这种类型的安全措施就像是安全的第一道门。

在 Android 中，我使用 Flutter 通道调用 Flutter 来配置 FlutterEngine 功能，让 Flutter 来决定如何处理这个用户。下面是 Android 的代码，用来检查设备是否有密码。

```
private val CHANNEL = "com.kbtg.flutter"
MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler { call, result ->
  if (call.method == "getDeviceHasPasscode") {
    result.success((getSystemService(Context.KEYGUARD_SERVICE) as KeyguardManager).isDeviceSecure)
  } else {
    result.notImplemented()
  }
}
```

请拨打以下电话到 Flutter。

```
try {
    final hasPasscode =
await Storage.platform.invokeMethod('getDeviceHasPasscode');
    if (!hasPasscode) {
        Toast.show(
        "No pin, DANGER DANGER",
        context,
        duration: 5,
        gravity: Toast.BOTTOM,
        );
    }
} on PlatformException catch (e) {
    debugPrint("==== Failed to scan security '${e.message}' ====");
}
```

如果 hasPasscode 是错误的，我们只是烤的消息，以警告用户，你没有一个针激活在您的设备。

#### 如果需要，关闭第三方键盘

有些第三方键盘可能是恶意的。你永远不会知道他们是否已经实现了秘密地将你输入的密码发送到他们的服务器上的功能。对于 Android 来说，没有简单的方法来防止这种情况，因为所有的键盘都算作第三方键盘，甚至 Android 自己的键盘。解决这个问题的办法是自己实现一个安全的键盘，方法是使用带有字母和数字的布局从头开始构建一个键盘。

但是，对于 iOS 系统，我们可以使用下面的函数来禁用它，只限制使用本地键盘

```
override func application(_ application: UIApplication, shouldAllowExtensionPointIdentifier extensionPointIdentifier: UIApplicationExtensionPointIdentifier) -> Bool {
    return extensionPointIdentifier != .keyboard
}
```

#### 检查代码完整性

可以对 IPA 和 APK 进行反编译，以更改内部的一些代码并重新构建以便再次发布。黑客修改的代码可能是你连接到同一个服务器，但是每个信息也会推送到黑客的服务器。对于 IPA 来说，我并不担心，因为从 App Store 以外安装应用程序是相当复杂的。你必须先安装证书并接受某些条件或破解你的设备才能安装它。至于安卓系统，它很容易伪造一个应用程序，因为它更开放，允许任何用户安装外部的 APK 只需要 1-2 次点击。

这就是代码完整性的用武之地。你可以计算你的应用程序的校验和，并在每次打开应用程序之前检查它是否仍然和你部署到应用程序商店的应用程序一样。这个概念听起来很简单，但是很难实现。幸运的是，有一个商业 SDK 可以解决这个问题，所以我们不需要自己动手。

#### 代码混淆

对于我们实现或保存到应用程序中的所有业务逻辑和安全逻辑，我们需要确保没有人能够反编译并查看源代码。如果黑客能看到它，他们就会知道我们用哪种逻辑来加密数据，或者我们在哪里存储安全信息。他们可以模仿这种逻辑，发送到他们的服务器，而不是使用我们的应用程序。对于代码混淆，可以使用商业 SDK 来加强应用程序。

你可能已经注意到我经常提到商业 SDK。有些人可能会想，“如果是这样的话，这篇文章的意义何在？我写这篇文章是为了寻找一种在应用程序中实现它的方法，而不是仅仅转到另一个链接。”

没有人什么都擅长。使用商业 SDK 就像拥有一个专注于安全工作的专业团队。你自己不能实现和关闭所有的安全漏洞，所以最好把它留给专家，他们知道他们在做什么，你做你最擅长的是开发应用程序。

现在你的应用外壳更加坚固，你的用户不再把钥匙留在门上，让我们确保房子的钥匙不容易被复制。我说的是密码和密码

### Stronger Pin

Pin 或 password 是一种确认你是账户真正所有者的方法。我们选择为应用程序设置一个密码，有两个原因。

#### 更好的用户体验

尽管密码更安全，但考虑到 A-Z、0-9 和特殊字符可以组成十亿种可能的组合，在设备上使用小键盘输入太难了。在进入你的应用程序之前，你可能会花费大量的时间。这就是为什么我们采用一个 6 位数的引脚来代替。虽然你只能用 4 个数字创建 10,000 个可能的引脚，但 6 个数字给你 1,000,000 个可能性，这是 100 倍的难度。

人们可以在 2-3 秒内输入一个密码，但是一个密码可能会花费他们 10-15 秒，这取决于它有多难。

#### 易于记忆

是的，我们希望让黑客难以破解密码，但我们也希望我们的用户能够不费吹灰之力地记住它。由于 pin 是 iOS/Android 的基本安全访问设备，它不应该发布任何挑战，因为用户已经习惯了

对于销子，我们不想让它变得太容易。“‘太容易’到底是什么意思?”你问。好吧，让我们说得更具体一点。

针没有标准，所以我从网上的研究中得出了一个想法。请看下面的链接。

https://www.researchgate.net/publication/313823128_Understanding_Human-Chosen_PINs_Characteristics_Distribution_and_Security

![](2021-04-30-09-15-15.png)

根据上述模式，我们得出以下规则

- 不允许有序列号，例如 123456、234567、345678 ー包括反序列号，例如 654321、543210
- 不要使用「同一行别针」 ，例如 123123、456456、789789
- 只允许 3 个或 3 个以上的唯一数字，例如 122112 个是不允许的，但 123321 个是可以的(即使这可能与上面的统计数据相矛盾)155115,133133,166661 是不允许的，因为只有两个不同的数字在密码中使用
- 有些人甚至建议禁用生日别针，比如如果你的生日是 1986 年 6 月 8 日，你就不能用 080686 作为别针来防止黑客利用这些信息入侵。但是，我不这样做只是因为我不在系统中保存用户的生日

我们不想制定太多的规则。否则，你会删除所有的密码组合，更糟糕的是，黑客更容易暴力破解它。我们可以制定更多的规则，但是如果 100 万种可能性变成 100-200 个引脚，那又有什么意义呢？

使用上面的方法，我们仍然有大约 60,000 种可能性供用户使用。下面是实现它的示例代码。

```
static bool isPinComplexity(String pin) {
   const notAllowListPin = [
   "123123",
   "456456",
   "789789",
   "012345",
   "123456",
   "234567",
   "345678",
   "456789",
   "567890",
   "098765",
   "987654",
   "876543",
   "765432",
   "654321",
   "543210"
   ];
   final pinSet = new Set.from(pin.split(""));
   final uniqueCharacter = pinSet.length > 2;
   return uniqueCharacter && !notAllowListPin.contains(pin);
}
```

您可以使用 isPinComplexity 来检查引脚。如果返回 true，我们允许用户添加它。不要忘记将所有可能的散列也添加到后端。我们在前端实现只是为了更好的用户体验，这样用户就不必打电话给网络，从服务器和服务器上被拒绝，以确保如果黑客试图绕过别针，服务器将不允许它。

你可以根据你的需要在 notAllowListPin 中添加更多的条件，但是我现在很好。

因此，对于我们实施的所有这些解决方案，我们要确保我们的应用程序配备了另一层保护。

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
