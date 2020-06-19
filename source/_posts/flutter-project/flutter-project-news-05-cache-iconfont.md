---
title: Flutter 实战从零开始 新闻客户端 - 05 AppData、Cache、Fiddle、iconfont、主界面搭建
date: 2020-03-25 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

## 本节目标

- 全局数据、响应数据、持久化
- http get 缓存
- http proxy 代理
- fiddle 抓包工具
- iconfont 字体库
- 主界面搭建
- BottomNavigationBar 导航控件
- 编写 api 接口代码

## 客户端数据管理

### 数据类型

- 全局数据

存储在内存

用户数据、语言包

- 响应数据

存储在内存

用户登录状态、多语言、皮肤样式

Redux、Bloc、provider

- 持久化

APP 保持磁盘上

浏览器 cookie localStorage

### 编写全局管理

- lib/global.dart

```dart
/// 全局配置
class Global {
  /// 用户配置
  static UserLoginResponseEntity profile = UserLoginResponseEntity(
    accessToken: null,
  );

  /// 是否 release
  static bool get isRelease => bool.fromEnvironment("dart.vm.product");

  /// init
  static Future init() async {
    // 运行初始
    WidgetsFlutterBinding.ensureInitialized();

    // 工具初始
    await StorageUtil.init();
    HttpUtil();

    // 读取离线用户信息
    var _profileJSON = StorageUtil().getJSON(STORAGE_USER_PROFILE_KEY);
    if (_profileJSON != null) {
      profile = UserLoginResponseEntity.fromJson(_profileJSON);
    }

    // http 缓存

    // android 状态栏为透明的沉浸
    if (Platform.isAndroid) {
      SystemUiOverlayStyle systemUiOverlayStyle =
          SystemUiOverlayStyle(statusBarColor: Colors.transparent);
      SystemChrome.setSystemUIOverlayStyle(systemUiOverlayStyle);
    }
  }

  // 持久化 用户信息
  static Future<bool> saveProfile(UserLoginResponseEntity userResponse) {
    profile = userResponse;
    return StorageUtil()
        .setJSON(STORAGE_USER_PROFILE_KEY, userResponse.toJson());
  }
}

```

### 调用运行

- lib/main.dart

```dart
void main() => Global.init().then((e) => runApp(MyApp()));

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

## Http 内存缓存

### 缓存策略

![](2020-03-25-14-46-53.png)

### 代码

- 缓存工具类 lib/common/utils/net_cache.dart

```dart
import 'dart:collection';

import 'package:dio/dio.dart';
import 'package:flutter_ducafecat_news/common/values/values.dart';

class CacheObject {
  CacheObject(this.response)
      : timeStamp = DateTime.now().millisecondsSinceEpoch;
  Response response;
  int timeStamp;

  @override
  bool operator ==(other) {
    return response.hashCode == other.hashCode;
  }

  @override
  int get hashCode => response.realUri.hashCode;
}

class NetCache extends Interceptor {
  // 为确保迭代器顺序和对象插入时间一致顺序一致，我们使用LinkedHashMap
  var cache = LinkedHashMap<String, CacheObject>();

  @override
  onRequest(RequestOptions options) async {
    if (!CACHE_ENABLE) return options;

    // refresh标记是否是"下拉刷新"
    bool refresh = options.extra["refresh"] == true;

    // 如果是下拉刷新，先删除相关缓存
    if (refresh) {
      if (options.extra["list"] == true) {
        //若是列表，则只要url中包含当前path的缓存全部删除（简单实现，并不精准）
        cache.removeWhere((key, v) => key.contains(options.path));
      } else {
        // 如果不是列表，则只删除uri相同的缓存
        delete(options.uri.toString());
      }
      return options;
    }

    // get 请求，开启缓存
    if (options.extra["noCache"] != true &&
        options.method.toLowerCase() == 'get') {
      String key = options.extra["cacheKey"] ?? options.uri.toString();
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
    }
  }

  @override
  onError(DioError err) async {
    // 错误状态不缓存
  }

  @override
  onResponse(Response response) async {
    // 如果启用缓存，将返回结果保存到缓存
    if (CACHE_ENABLE) {
      _saveCache(response);
    }
  }

  _saveCache(Response object) {
    RequestOptions options = object.request;

    // 只缓存 get 的请求
    if (options.extra["noCache"] != true &&
        options.method.toLowerCase() == "get") {
      // 如果缓存数量超过最大数量限制，则先移除最早的一条记录
      if (cache.length == CACHE_MAXCOUNT) {
        cache.remove(cache[cache.keys.first]);
      }
      String key = options.extra["cacheKey"] ?? options.uri.toString();
      cache[key] = CacheObject(object);
    }
  }

  void delete(String key) {
    cache.remove(key);
  }
}

```

- dio 封装 lib/common/utils/http.dart

```dart
  // 加内存缓存
  HttpUtil._internal() {
    ...
    dio.interceptors.add(NetCache());
    ...
  }

  // 修改 get 请求
  /// restful get 操作
  /// refresh 是否下拉刷新 默认 false
  /// noCache 是否不缓存 默认 true
  /// list 是否列表 默认 false
  /// cacheKey 缓存key
  Future get(
    String path, {
    dynamic params,
    Options options,
    bool refresh = false,
    bool noCache = !CACHE_ENABLE,
    bool list = false,
    String cacheKey,
  }) async {
    try {
      Options requestOptions = options ?? Options();
      requestOptions = requestOptions.merge(extra: {
        "refresh": refresh,
        "noCache": noCache,
        "list": list,
        "cacheKey": cacheKey,
      });
      Map<String, dynamic> _authorization = getAuthorizationHeader();
      if (_authorization != null) {
        requestOptions = requestOptions.merge(headers: _authorization);
      }

      var response = await dio.get(path,
          queryParameters: params,
          options: requestOptions,
          cancelToken: cancelToken);
      return response.data;
    } on DioError catch (e) {
      throw createErrorEntity(e);
    }
  }
```

## Http Proxy 代理 + Fiddle 抓包

### 安装 Fiddle

https://www.telerik.com/download/fiddler-everywhere

![](2020-03-26-14-03-16.png)

### dio 加入 proxy

- lib/common/utils/http.dart

```dart
  if (!Global.isRelease && PROXY_ENABLE) {
    (dio.httpClientAdapter as DefaultHttpClientAdapter).onHttpClientCreate =
        (client) {
      client.findProxy = (uri) {
        return "PROXY $PROXY_IP:$PROXY_PORT";
      };
      //代理工具会提供一个抓包的自签名证书，会通不过证书校验，所以我们禁用证书校验
      client.badCertificateCallback =
          (X509Certificate cert, String host, int port) => true;
    };
  }
```

## Iconfont 字体库

### 引入流程

- 登录

https://www.iconfont.cn

- 创建字体项目

![](2020-03-25-14-56-48.png)

- 字体文件放入

assets/fonts/iconfont.ttf

- pubspec.yaml

```yaml
  fonts:
    ...
    - family: Iconfont
      fonts:
        - asset: assets/fonts/iconfont.ttf
```

- lib/common/utils/iconfont.dart

```dart
import 'package:flutter/material.dart';

class Iconfont {
    // iconName: share
  static const share = IconData(
    0xe60d,
    fontFamily: 'Iconfont',
    matchTextDirection: true,
  );

  ...
}
```

### 自动生成字体库代码

https://github.com/ymzuiku/iconfont_builder

- 拉取项目、编译

```sh
# 拉取项目
> git clone https://github.com/ymzuiku/iconfont_builder

# 更新包
> pub get

# 安装工具
> pub global activate iconfont_builder

# 检查环境配置
export PATH=${PATH}:~/.pub-cache/bin
```

- 参考我的配置

```sh
# flutter sdk
export PATH=${PATH}:~/Documents/sdk/flutter/bin

# dart sdk
export PATH=${PATH}:~/Documents/sdk/flutter/bin/cache/dart-sdk/bin
export PATH=${PATH}:~/.pub-cache/bin

# flutter-io 国内镜像
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

# android
export ANDROID_HOME=~/Library/Android/sdk
export PATH=${PATH}:${ANDROID_HOME}/platform-tools
export PATH=${PATH}:${ANDROID_HOME}/tools
```

- 生成字体类

```sh
cd 你的项目根目录
iconfont_builder --from ./assets/fonts --to ./lib/common/utils/iconfont.dart
```

## 编写 api 业务代码

- yapi 配置

导入 doc/api.json

![](2020-03-25-15-09-38.png)

- 代码

![](2020-03-25-15-06-42.png)

## 搭建主界面框架

![](2020-03-26-16-24-46.png)

- 框架页面 lib/pages/application/application.dart

```dart
...
class _ApplicationPageState extends State<ApplicationPage>
    with SingleTickerProviderStateMixin {
  // 当前 tab 页码
  int _page = 0;
  // tab 页标题
  final List<String> _tabTitles = [
    'Welcome',
    'Cagegory',
    'Bookmarks',
    'Account'
  ];
  // 页控制器
  PageController _pageController;

  // 底部导航项目
  final List<BottomNavigationBarItem> _bottomTabs = <BottomNavigationBarItem>[...];

  // tab栏动画
  void _handleNavBarTap(int index) {
    ...
  }

  // tab栏页码切换
  void _handlePageChanged(int page) {
    ...
  }

  // 顶部导航
  Widget _buildAppBar() {
    return Container();
  }

  // 内容页
  Widget _buildPageView() {
    return Container();
  }

  // 底部导航
  Widget _buildBottomNavigationBar() {
    return Container();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: _buildAppBar(),
      body: _buildPageView(),
      bottomNavigationBar: _buildBottomNavigationBar(),
    );
  }
}

```

## 编写首页代码

![](2020-03-26-16-25-42.png)

- 首页代码 lib/pages/main/main.dart

```dart
...

class _MainPageState extends State<MainPage> {
  NewsPageListResponseEntity _newsPageList; // 新闻翻页
  NewsRecommendResponseEntity _newsRecommend; // 新闻推荐
  List<CategoryResponseEntity> _categories; // 分类
  List<ChannelResponseEntity> _channels; // 频道

  String _selCategoryCode; // 选中的分类Code

  @override
  void initState() {
    super.initState();
    _loadAllData();
  }

  // 读取所有数据
  _loadAllData() async {
    ...
  }

  // 分类菜单
  Widget _buildCategories() {
    return Container();
  }
  // 抽取前先实现业务

  // 推荐阅读
  Widget _buildRecommend() {
    return Container();
  }

  // 频道
  Widget _buildChannels() {
    return Container();
  }

  // 新闻列表
  Widget _buildNewsList() {
    return Container();
  }

  // ad 广告条
  // 邮件订阅
  Widget _buildEmailSubscribe() {
    return Container();
  }

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
      child: Column(
        children: <Widget>[
          _buildCategories(),
          _buildRecommend(),
          _buildChannels(),
          _buildNewsList(),
          _buildEmailSubscribe(),
        ],
      ),
    );
  }
}

```

- 抽取新闻分类 lib/pages/main/categories_widget.dart

```dart
Widget newsCategoriesWidget(
    {List<CategoryResponseEntity> categories,
    String selCategoryCode,
    Function(CategoryResponseEntity) onTap}) {
  return categories == null
      ? Container()
      : SingleChildScrollView(
          scrollDirection: Axis.horizontal,
          child: Row(
            children: categories.map<Widget>((item) {
              return Container(
                alignment: Alignment.center,
                height: duSetHeight(52),
                padding: EdgeInsets.symmetric(horizontal: 8),
                child: GestureDetector(
                  child: Text(
                    item.title,
                    style: TextStyle(
                      color: selCategoryCode == item.code
                          ? AppColors.secondaryElementText
                          : AppColors.primaryText,
                      fontSize: duSetFontSize(18),
                      fontFamily: 'Montserrat',
                      fontWeight: FontWeight.w600,
                    ),
                  ),
                  onTap: () => onTap(item),
                ),
              );
            }).toList(),
          ),
        );
}
```

## 蓝湖设计稿

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

## YAPI 接口管理

http://yapi.demo.qunar.com/

## git 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.5

## 工具

- [json to object quicktype](https://app.quicktype.io)
- [Fiddler 抓包](https://www.telerik.com/download/fiddler-everywhere)
- [iconfont 阿里图标库](https://www.iconfont.cn)
- [Iconfont 生成工具](https://github.com/ymzuiku/iconfont_builder)

## VSCode 插件

- Flutter、Dart
- [Flutter Widget Snippets](https://marketplace.visualstudio.com/items?itemName=alexisvt.flutter-snippets)
- [Awesome Flutter Snippets](https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets)
- [Paste JSON as Code](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)
- [bloc](https://marketplace.visualstudio.com/items?itemName=FelixAngelov.bloc)

## 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
