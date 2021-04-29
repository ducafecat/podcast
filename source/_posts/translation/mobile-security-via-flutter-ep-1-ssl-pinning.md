---
title: Flutter 移动安全 ー Ep.1 SSL Pinning
tags: flutter
categories: 译文
date: 2021-04-29 00:00:00
---

![](2021-04-29-06-24-31.png)

## 原文

> https://medium.com/kbtg-life/mobile-security-via-flutter-ep-1-ssl-pinning-c57f18b711f6

## 前言

![](2021-04-29-06-21-31.png)

这篇文章还是很好的，作者是一个银行从业者，简单说就是在你的程序里指定 ssl 通讯证书，提供安全性。

为啥这样做呢，比如你用公共 `wifi` ，这时候打开 `www.taobaomy.com` 竟然证书显示机构 “淘宝官方”，这显然是伪造的。

还有你确实访问了 `www.taobao.com` ，但是被公共 `wifi` 中间劫持替换证书，重定向去了他的网站，你输入的 `用户名、密码` 都被窃取了。

所以说呢，验证通讯证书很有必要。

如果觉得好，请分享到朋友圈。

## 参考

- https://owasp.org/
- https://api.flutter.dev/flutter/dart-io/HttpClient/badCertificateCallback.html
- https://github.com/dart-lang/sdk/issues/39425#issuecomment-680312787
- https://pub.dev/packages/ssl_pinning_plugin

## 正文

由于 2019 冠状病毒疾病的流行，我们看到移动应用程序的使用量有了很大的增长。开发人员必须不断跟上新特性的发展，或者改善更好的用户体验。随着一切都在网上进行，越来越多的钱涌入这个行业，这些自然而然的坏家伙(也就是坏黑客)想要利用它。我敢肯定我们都知道设计思维和如何与用户产生共鸣，因为如果我们从未使用过我们的产品，或者更糟糕的是，喜欢它，你还能指望用户如何喜欢我们的产品吗？是时候设身处地为别人着想了。在 KBTG，我们有一个叫做“狗食”的程序。你们中的一些人可能听说过这个成语“吃自己的狗粮”。这意味着你必须在别人喜欢你的产品之前使用你的产品，坚持使用你的产品，并且热爱你的产品。有些人可能会认为用户喜欢的是前沿的特性或者很酷的设计，但是在 KBTG，我们不仅仅局限于其中的两个。

安全是我们同情用户的另一个重要组成部分。他们信任我们，向我们提供他们有价值的私人信息，所以我们的荣誉和责任就是为我们的系统提供完全的内部和外部保护。所谓内部人员，我们指的是像我们这样的开发人员。出于数据隐私的原因，我们根本没有能力查看我们的数据库或查看用户的信息。所有的东西都是加密的，我们的团队中没有人可以访问这个密钥。这个过程表明我们是多么关心我们的用户。对我们来说困难的部分是当我们遇到一个问题，它是很难调查，因为我们没有访问原始数据来解决它，但我们接受这个挑战，因为我们自豪地保护我们的用户隐私。

在本系列文章中，我将分享我从银行业工作中获得的知识，我坚信这些知识体现了最高安全性的领域。我的向导没那么难。我只是遵循 OWASP 的标准。

https://owasp.org/

我会告诉你如何在 Flutter 中实现这个。我们还没有尝试开发一个具有超级安全性的应用程序，但这将是指导所有移动应用程序的基本的，必须具备的安全性。实现过程并不复杂。这可能需要你 10 天左右的时间来开发。是的，只有 10 天！与通常需要 80-90 天的功能相比，这听起来几乎没什么。让我们从 SSL 钉住开始本系列的第一集。

### SSL Pinning

SSL Pinning 可以防止 MITM (Man in the Middle Attack) ，但那到底是什么呢？

简而言之，当你连接到一个公共 WIFI 或热点时，负责网络的 IT 人员，无论好坏，都可以把流量从你的移动设备传送到你连接的服务器。想了解更多细节，你可以去专业网站上查找，比如下面的一个。底线是不要使用公共 WIFI 或任何其他人的热点！

![](2021-04-29-06-12-51.png)

Credit 信用 https://www.guardsquare.com/en/blog/iOS-SSL-certificate-pinning-bypassing

### 开始实现

如果您正在调查 Stackoverflow 关于 Flutter 或 Dart 中 SSL pinting 的内容，那么您可能会找到一个关于 `badCertificateCallback` 的解决方案。

https://api.flutter.dev/flutter/dart-io/HttpClient/badCertificateCallback.html

基本上，您可以通过告诉 Flutter 不要信任任何证书(除了您在移动应用程序中提供的证书)来覆盖 Flutter。下面是如何实现的方法。

```dart
HttpClient _client = new HttpClient(context: await globalContext);
_client.badCertificateCallback =
(X509Certificate cert, String host, int port) => false;
var _ioClient = new IOClient(_client);
_ioClient.get(url)
```

创建 HttpClient，将 `globalContext` 传递给它，并将 badcertificateecallback 分配为 false。

获得 ioClient 之后，可以使用它调用 GET、 POST、 PUT、 DELETE。上面的代码表明我只会信任其中的两个证书，因此如果其他证书在移动应用程序有请求时被发送，它将在获得 badCertificateCallback 后停止工作。

以下是获取 GlobalContext 的方法。

```dart
Future<SecurityContext> get globalContext async {
   // Note: Not allowed to load the same certificate
   final sslCert1 = await
   rootBundle.load('assets/cert/certificate.pem');
   final sslCert2 = await
   rootBundle.load('assets/cert/certificate2.pem');
   SecurityContext sc = new SecurityContext(withTrustedRoots: false);
   sc.setTrustedCertificatesBytes(sslCert1.buffer.asInt8List());
   sc.setTrustedCertificatesBytes(sslCert2.buffer.asInt8List());
return sc;
}
```

为了获得 certificate.pem，我使用这个脚本从服务器获得公钥，并在 Terminal 中运行这个命令以获得 certificate.pem 文件。不要忘记在没有 HTTP 或 HTTPs 的情况下将“ your-url. com”更改为您的网站。

```shell
openssl s_client -showcerts -connect your-url.com:443 -servername your-url.com </dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > certificate.pem
```

一旦你获得了 certificate.pem，把你的证书放到你的目标资产中，并将你的资产添加到 pubspec.yaml 中。我将它添加到 cert 文件夹中。

```yaml
assets:
  - assets/cert/
```

您可以设置任意多的信任证书，但我通常只设置四个证书。原因是你拥有的越多，你就越容易被黑客攻击。但是为什么是四个呢？对于防火墙，我使用 Akamai 在到达服务器之前阻止和过滤请求，消除系统中的坏请求、匿名请求和 DDoS 攻击。因此，两个证书是为 Akamai，而其他两个是为了更新证书在未来。我曾经试图固定相同的证书或无效的证书，结果 Dart 像预期的那样抛给我一个错误。

一旦我们完成了以上所有的工作，一切似乎都在工作。如果您更改了服务器上的证书，应用程序将停止工作。呜呼！任务完成了，对吧？一开始是我..。

直到我发现了 badCertificateCallback 的一个大问题。

https://github.com/dart-lang/sdk/issues/39425#issuecomment-680312787

原来 badCertificateCallback 在没有检查通用名称的情况下就固定了中间证书，这造成了一个严重的安全问题，因为坏的黑客也可以创建这些证书。例如，在我的例子中，我将 Let’s Encrypt 作为一个中间证书，因此如果一个黑客创建了假证书并将其发送到我们的应用程序，它将接受这个请求！因为我们不检查通用名称，并假设证书来自同一证书提供程序。

为了解决这个问题，我还必须使用另一种方法检查该证书的 SHA256。

```dart
Future<bool> get _isAllowList async {
    const myAllowList = "xxxxxxx";
    final x509Cert1 = await _readPemCert('assets/cert/certificate.pem');
    X509CertificateData data = X509Utils.x509CertificateFromPem(x509Cert1);
    return data.sha256Thumbprint == myAllowList
}
```

来自下面的函数。这可能不是一个好的解决方案，因为我不知道证书是如何工作的，所以我只使用字符串操作: p

基本上，我只是削减了证书的其他部分，只得到最后一部分。

```dart
Future<String> _readPemCert(String path) async {
   final sslCert = await rootBundle.load(path);
   final data = sslCert.buffer.asUint8List();
   final pemString = utf8.decode(data);
   final pemArray = pemString.split("-----END CERTIFICATE-----");
   final cert = [pemArray[0], "-----END CERTIFICATE-----"].join("");
   return cert;
}
```

然后，我使用 libs basic_utils 来解析证书。把这个加到你的 pubspec.yaml 里。我不能用 basic 语言解析整个证书，所以我必须这样做。

```yaml
basic_utils: ^2.7.0-rc.4
```

使用 get X509Utils 获得 sha256 来与允许列表进行比较，这个允许列表在省道中保存为常量。现在，我们可以使用两个方法(badCertCallback 和 X509Utils)再次检查安全性，以查看添加到允许列表中的证书和来自服务器的 Sha256 是否相同。

最近，在我实现了我的方法之后，我发现了一个关于 SSL 固定的新的库。

https://pub.dev/packages/ssl_pinning_plugin

由于我的解决方案已经被移动安全渗透测试接受，我还没有测试过这个解决方案，所以我决定坚持使用旧的解决方案。它可能看起来不那么干净，但是它正如预期的那样工作，所以我称之为成功。

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
