---
title: Flutter 实战从零开始 新闻客户端 - 07 Provider、认证授权、骨架屏、磁盘缓存
date: 2020-04-08 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

## 本节目标

- 第一次登录显示欢迎界面
- 离线登录
- Provider 响应数据管理
- 实现 APP 色彩灰度处理
- 注销登录
- Http Status 401 认证授权
- 首页磁盘缓存
- 首页缓存策略，延迟 1~3 秒
- 首页骨架屏

## 视频

<iframe src="//player.bilibili.com/player.html?bvid=BV1vV411o7bn&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

<iframe src="//player.bilibili.com/player.html?bvid=BV1SA411t7ov&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

<iframe src="//player.bilibili.com/player.html?bvid=BV1jt4y1U7Nn&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

<iframe src="//player.bilibili.com/player.html?bvid=BV1wt4y127L5&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

<iframe src="//player.bilibili.com/player.html?bvid=BV1b54y1d7DB&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

<iframe src="//player.bilibili.com/player.html?bvid=BV11z411b7FJ&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="400px"> </iframe>

## 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.7

## 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

## 资源

- 蓝湖设计稿（加微信给授权 ducafecat）
  https://lanhuapp.com/url/wbhGq

- YAPI 接口管理
  http://yapi.demo.qunar.com/

- 代码
  https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.7

- 参考

  - [provider](https://pub.flutter-io.cn/packages/provider)
  - [pk_skeleton](https://pub.flutter-io.cn/packages/pk_skeleton)

## 第一次显示欢迎界面、离线登录

![](2020-04-08-22-08-21.png)

- lib/global.dart

```dart
  /// 是否第一次打开
  static bool isFirstOpen = false;

  /// 是否离线登录
  static bool isOfflineLogin = false;

  /// init
  static Future init() async {
    ...

    // 读取设备第一次打开
    isFirstOpen = !StorageUtil().getBool(STORAGE_DEVICE_ALREADY_OPEN_KEY);
    if (isFirstOpen) {
      StorageUtil().setBool(STORAGE_DEVICE_ALREADY_OPEN_KEY, true);
    }

    // 读取离线用户信息
    var _profileJSON = StorageUtil().getJSON(STORAGE_USER_PROFILE_KEY);
    if (_profileJSON != null) {
      profile = UserLoginResponseEntity.fromJson(_profileJSON);
      isOfflineLogin = true;
    }
```

- lib/pages/index/index.dart

```dart
class IndexPage extends StatefulWidget {
  IndexPage({Key key}) : super(key: key);

  @override
  _IndexPageState createState() => _IndexPageState();
}

class _IndexPageState extends State<IndexPage> {
  @override
  Widget build(BuildContext context) {
    ScreenUtil.init(
      context,
      width: 375,
      height: 812 - 44 - 34,
      allowFontScaling: true,
    );

    return Scaffold(
      body: Global.isFirstOpen == true
          ? WelcomePage()
          : Global.isOfflineLogin == true ? ApplicationPage() : SignInPage(),
    );
  }
}
```

## Provider 实现动态灰度处理

https://pub.flutter-io.cn/packages/provider

### 步骤 1：安装依赖

```yaml
dependencies:
  provider: ^4.0.4
```

### 步骤 2：创建响应数据类

- lib/common/provider/app.dart

```dart
import 'package:flutter/material.dart';

/// 系统相应状态
class AppState with ChangeNotifier {
  bool _isGrayFilter;

  get isGrayFilter => _isGrayFilter;

  AppState({bool isGrayFilter = false}) {
    this._isGrayFilter = isGrayFilter;
  }
}
```

### 步骤 3：初始响应数据

#### 方式一：先创建数据对象，再挂载

- lib/global.dart

```dart
  /// 应用状态
  static AppState appState = AppState();
```

- lib/main.dart

```dart
void main() => Global.init().then((e) => runApp(
      MultiProvider(
        providers: [
          ChangeNotifierProvider<AppState>.value(
            value: Global.appState,
          ),
        ],
        child: MyApp(),
      ),
    ));
```

#### 方式二：挂载时，创建对象

- lib/main.dart

```dart
void main() => Global.init().then((e) => runApp(
      MultiProvider(
        providers: [
          ChangeNotifierProvider<AppState>(
            Create: (_) => new AppState(),
          ),
        ],
        child: MyApp(),
      ),
    ));
```

### 步骤 4：通知数据发声变化

- lib/common/provider/app.dart

```dart
class AppState with ChangeNotifier {
  ...

  // 切换灰色滤镜
  switchGrayFilter() {
    _isGrayFilter = !_isGrayFilter;
    notifyListeners();
  }
}
```

### 步骤 5：收到数据发声变化

#### 方式一：Consumer

- lib/main.dart

```dart
void main() => Global.init().then((e) => runApp(
      MultiProvider(
        providers: [
          ChangeNotifierProvider<AppState>.value(
            value: Global.appState,
          ),
        ],
        child: Consumer<AppState>(builder: (context, appState, _) {
          if (appState.isGrayFilter) {
            return ColorFiltered(
              colorFilter: ColorFilter.mode(Colors.white, BlendMode.color),
              child: MyApp(),
            );
          } else {
            return MyApp();
          }
        }),
      ),
    ));
```

#### 方式二：Provider.of

- lib/pages/account/account.dart

```dart
    final appState = Provider.of<AppState>(context);

    return Column(
      children: <Widget>[
        MaterialButton(
          onPressed: () {
            appState.switchGrayFilter();
          },
          child: Text('灰色切换 ${appState.isGrayFilter}'),
        ),
      ],
    );
```

### 多个响应数据处理

- 挂载用 MultiProvider

- 接收用 Consumer2 ~ Consumer6

## 注销登录

- lib/common/utils/authentication.dart

```dart
/// 检查是否有 token
Future<bool> isAuthenticated() async {
  var profileJSON = StorageUtil().getJSON(STORAGE_USER_PROFILE_KEY);
  return profileJSON != null ? true : false;
}

/// 删除缓存 token
Future deleteAuthentication() async {
  await StorageUtil().remove(STORAGE_USER_PROFILE_KEY);
  Global.profile = null;
}

/// 重新登录
Future goLoginPage(BuildContext context) async {
  await deleteAuthentication();
  Navigator.pushNamedAndRemoveUntil(
      context, "/sign-in", (Route<dynamic> route) => false);
}
```

- lib/pages/account/account.dart

```dart
class _AccountPageState extends State<AccountPage> {
  @override
  Widget build(BuildContext context) {
    final appState = Provider.of<AppState>(context);

    return Column(
      children: <Widget>[
        Text('用户: ${Global.profile.displayName}'),
        Divider(),
        MaterialButton(
          onPressed: () {
            goLoginPage(context);
          },
          child: Text('退出'),
        ),
      ],
    );
  }
}
```

## Http Status 401 认证授权

### dio 封装界面的上下文对象 BuildContext context

- lib/common/utils/http.dart

```dart
  Future post(
    String path, {
    @required BuildContext context,
    dynamic params,
    Options options,
  }) async {
    Options requestOptions = options ?? Options();
    requestOptions = requestOptions.merge(extra: {
      "context": context,
    });
    ...
  }
```

### 错误处理 401 去登录界面

- lib/common/utils/http.dart

```dart
    // 添加拦截器
    dio.interceptors
        .add(InterceptorsWrapper(onRequest: (RequestOptions options) {
      return options; //continue
    }, onResponse: (Response response) {
      return response; // continue
    }, onError: (DioError e) {
      ErrorEntity eInfo = createErrorEntity(e);
      // 错误提示
      toastInfo(msg: eInfo.message);
      // 错误交互处理
      var context = e.request.extra["context"];
      if (context != null) {
        switch (eInfo.code) {
          case 401: // 没有权限 重新登录
            goLoginPage(context);
            break;
          default:
        }
      }
      return eInfo;
    }));
```

## 首页磁盘缓存

- lib/common/utils/net_cache.dart

```dart
      // 策略 1 内存缓存优先，2 然后才是磁盘缓存

      // 1 内存缓存
      var ob = cache[key];
      if (ob != null) {
        //若缓存未过期，则返回缓存内容
        if ((DateTime.now().millisecondsSinceEpoch - ob.timeStamp) / 1000 <
            CACHE_MAXAGE) {
          return cache[key].response;
        } else {
          //若已过期则删除缓存，继续向服务器请求
          cache.remove(key);
        }
      }

      // 2 磁盘缓存
      if (cacheDisk) {
        var cacheData = StorageUtil().getJSON(key);
        if (cacheData != null) {
          return Response(
            statusCode: 200,
            data: cacheData,
          );
        }
      }
```

## 首页缓存策略，延迟 1~3 秒

- lib/pages/main/channels_widget.dart

```dart
  // 如果有磁盘缓存，延迟3秒拉取更新档案
  _loadLatestWithDiskCache() {
    if (CACHE_ENABLE == true) {
      var cacheData = StorageUtil().getJSON(STORAGE_INDEX_NEWS_CACHE_KEY);
      if (cacheData != null) {
        Timer(Duration(seconds: 3), () {
          _controller.callRefresh();
        });
      }
    }
  }
```

## 首页骨架屏

https://pub.flutter-io.cn/packages/pk_skeleton

- lib/pages/main/main.dart

```dart
  @override
  Widget build(BuildContext context) {
    return _newsPageList == null
        ? cardListSkeleton()
        : EasyRefresh(
            enableControlFinishRefresh: true,
            controller: _controller,
            ...
```

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
