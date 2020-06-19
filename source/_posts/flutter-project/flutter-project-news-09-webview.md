---
title: Flutter 实战从零开始 新闻客户端 - 09 详情页展示、分享、远程真机调试
date: 2020-04-24 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

# 本节目标

- 详情页技术方案比较
- 载入 web 内容
- 自动计算高度
- 清除广告、推荐
- 拦截请求
- loading 状态显示
- 分享插件
- 远程 android 设备调试

## 详情展示

### 技术方案选择

#### 分析工具 UI automator view

- 文件位置

/Users/ducafecat/Library/Android/sdk/tools/bin/uiautomatorviewer

![](2020-04-24-10-32-51.png)

#### 淘宝方案

![](2020-04-24-10-30-45.png)

> 混合方式

#### 头条

![](2020-04-24-11-08-21.png)

![](2020-04-24-11-09-58.png)

> 混合方式

#### 什么值得买

![](2020-04-24-11-14-22.png)

![](2020-04-24-11-15-10.png)

![](2020-04-24-11-16-16.png)

> 单一 webView

### 技术点分析

- 1. webView 原生 混合方式
- 2. 计算 web 页面高度
- 3. 拦截请求，自定义指令
- 4. 内存占用（尽量少的 dom 元素）

### 安装插件

- webview_flutter

https://pub.flutter-io.cn/packages/webview_flutter

- pubspec.yaml

```yaml
dependencies:
  webview_flutter: ^0.3.20+2
```

- ios/Runner/Info.plist

```plist
	<key>io.flutter.embedded_views_preview</key>
	<true/>
```

### 构建界面代码

```dart
  // 顶部导航
  Widget _buildAppBar() {
    return Container();
  }

  // 页标题
  Widget _buildPageTitle() {
    return Container();
  }

  // 页头部
  Widget _buildPageHeader() {
    return Container();
  }

  // web内容
  Widget _buildWebView() {
    return Container();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: _buildAppBar(),
        body: SingleChildScrollView(
              child: Column(
                children: <Widget>[
                  _buildPageTitle(),
                  Divider(height: 1),
                  _buildPageHeader(),
                  _buildWebView(),
                ],
              ),
            ),
          );
  }

```

### url 载入

```dart
  Widget _buildWebView() {
    return Container(
      height: _webViewHeight,
      child: WebView(
        initialUrl:
            '$SERVER_API_URL/news/content/${widget.item.id}', //widget.url,
        javascriptMode: JavascriptMode.unrestricted,
        onWebViewCreated: (WebViewController webViewController) async {
          _controller.complete(webViewController);
        },
        gestureNavigationEnabled: true,
      ),
    );
  }
```

### 计算高度

- PX DP

  - https://blog.akanelee.me/2018/07/31/dpi-px-pt-dp-sp/

- 设备像素密度

  > 一个逻辑像素占用多少个实际像素

  - https://developer.mozilla.org/zh-CN/docs/Web/API/Window/devicePixelRatio
  - https://api.flutter.dev/flutter/dart-ui/Window/devicePixelRatio.html

- 注册 js

```dart
  double _webViewHeight = 200;

        javascriptChannels: <JavascriptChannel>[
          _invokeJavascriptChannel(context),
        ].toSet(),
```

```dart

  // 注册js回调
  JavascriptChannel _invokeJavascriptChannel(BuildContext context) {
    return JavascriptChannel(
        name: 'Invoke',
        onMessageReceived: (JavascriptMessage message) {
          print(message.message);
          var webHeight = double.parse(message.message);
          if (webHeight != null) {
            setState(() {
              _webViewHeight = webHeight;
            });
          }
        });
  }

```

- 回调

```dart
        onPageFinished: (String url) {
          _getWebViewHeight();
          setState(() {
            _isPageFinished = true;
          });
        },
```

```dart
  // 获取页面高度
  _getWebViewHeight() async {
    await (await _controller.future)?.evaluateJavascript('''
        try {
          // Invoke.postMessage([document.body.clientHeight,document.documentElement.clientHeight,document.documentElement.scrollHeight]);
          let scrollHeight = document.documentElement.scrollHeight;
          if (scrollHeight) {
            Invoke.postMessage(scrollHeight);
          }
        } catch {}
        ''');
  }
```

### 清除广告、推荐

- https://cn.engadget.com/cn-2020-01-21-google-pixelbook-go-not-pink-available.html

- 删除广告

```dart
        onPageStarted: (String url) {
          Timer(Duration(seconds: 1), () {
             setState(() {
               _isPageFinished = true;
             });
           _removeAd();
           _getViewHeight();
          });
        },
```

```dart
  _removeWebViewAd() async {
    await (await _controller.future)?.evaluateJavascript('''
        try {
          function removeElement(elementName){
            let _element = document.getElementById(elementName);
            if(!_element) {
              _element = document.querySelector(elementName);
            }
            if(!_element) {
              return;
            }
            let _parentElement = _element.parentNode;
            if(_parentElement){
                _parentElement.removeChild(_element);
            }
          }

          removeElement('module-engadget-deeplink-top-ad');
          removeElement('module-engadget-deeplink-streams');
          removeElement('footer');
        } catch{}
        ''');
  }
```

### 拦截请求

- 页面中 href

```html
<div class="tags">
  <a href="/tag/chrome-os" class="tag">chrome os</a>
  <a href="/tag/chromebook" class="tag">chromebook</a>
  <a href="/tag/computer" class="tag">computer</a>
  <a href="/tag/gear" class="tag">gear</a>
  <a href="/tag/google" class="tag">google</a>
  <a href="/tag/laptop" class="tag">laptop</a>
  <a href="/tag/personal computing" class="tag">personal computing</a>
  <a href="/tag/personalcomputing" class="tag">personalcomputing</a>
  <a href="/tag/pixelbook-go" class="tag">pixelbook go</a>
</div>
```

- navigation 拦截

```dart
        navigationDelegate: (NavigationRequest request) {
          if (request.url != '$SERVER_API_URL/news/content/${widget.item.id}') {
            toastInfo(msg: request.url);
            return NavigationDecision.prevent;
          }
          return NavigationDecision.navigate;
        },
```

### loading 状态显示

```dart

  bool _isPageFinished = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: _buildAppBar(),
        body: Stack(
          children: <Widget>[
            SingleChildScrollView(
              child: Column(
                children: <Widget>[
                  _buildPageTitle(),
                  Divider(height: 1),
                  _buildPageHeader(),
                  _buildWebView(),
                ],
              ),
            ),
            _isPageFinished == true
                ? Container()
                : Align(
                    alignment: Alignment.center,
                    child: LoadingBouncingGrid.square(),
                  ),
          ],
        ));
  }

```

## 分享

### 安装插件

```yaml
dependencies:
  share: ^0.6.4
```

### 代码

```dart
            onPressed: () {
              Share.share('${widget.item.title} ${widget.item.url}');
            },
```

### 真机调试

- scrcpy

https://github.com/Genymobile/scrcpy

## 资源

### 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

### 蓝湖设计稿（加微信给授权 ducafecat）

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

### YAPI 接口管理

http://yapi.demo.qunar.com/

### 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.9

### 参考

https://pub.flutter-io.cn/packages/webview_flutter
https://pub.flutter-io.cn/packages/loading_animations
https://pub.flutter-io.cn/packages/share
https://github.com/Genymobile/scrcpy

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
