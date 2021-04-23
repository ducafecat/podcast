---
title: Flutter Getx 04 - GetConnect、StateMixin、SuperController、GetController + Dio
date: 2021-4-23 00:00:00
tags: flutter getx
categories: Flutter Getx
---

![](2021-04-23-18-24-26.png)

## 本节目标

- GetConnect
- StateMixin
- GetController + Dio
- SuperController

## 视频

## 代码

https://github.com/ducafecat/getx_quick_start

## 参考

- https://pub.flutter-io.cn/packages/get#getconnect

## 正文

### GetConnect

![](GetContent.png)

- 瞎聊设计模式

`Provider` 提供者模式 位于高层 由他来决定从哪里、提供什么

相对应的有 `Consumer` 消费者模式

`Repository` 模式，这层有 `OO` 面向对象的意思，用来处理拉取数据细节，这样到 `Controller` 控制器 这一层只要处理业务就行，可方便测试

`DAO` 就是纯粹的数据访问层，没有 `00` 的概念

`Service` `Model` `Entity` ...

前端其实对数据加工、面向服务、领域模型偏弱，更多的是组件拆分、样式、布局，这才是要关系的，就算是测试也是 `E2E` 侧重不同。

`E2E`（End To End）即端对端测试，属于黑盒测试，通过编写测试用例，自动化模拟用户操作，确保组件间通信正常，程序流数据传递如预期。

- 封装 GetConnect

lib/common/utils/base_provider.dart

```dart
class BaseProvider extends GetConnect {
  @override
  void onInit() {
    httpClient.baseUrl = SERVER_API_URL;

    // 请求拦截
    httpClient.addRequestModifier<void>((request) {
      request.headers['Authorization'] = '12345678';
      return request;
    });

    // 响应拦截
    httpClient.addResponseModifier((request, response) {
      return response;
    });
  }
}
```

- Provider

lib/pages/getConnect/provider.dart

```dart
abstract class INewsProvider {
  Future<Response<NewsPageListResponseEntity>> getNews();
}

class NewsProvider extends BaseProvider implements INewsProvider {
  // 新闻分页
  // @override
  // Future<Response<NewsPageListResponseEntity>> getNews() => get("/news");
  @override
  Future<Response<NewsPageListResponseEntity>> getNews() async {
    var response = await get("/news");
    var data = NewsPageListResponseEntity.fromJson(response.body);
    return Response(
      statusCode: response.statusCode,
      statusText: response.statusText,
      body: data,
    );
  }
}
```

- Repository

lib/pages/getConnect/repository.dart

```dart
abstract class INewsRepository {
  Future<NewsPageListResponseEntity> getNews();
}

class NewsRepository implements INewsRepository {
  NewsRepository({required this.provider});
  final INewsProvider provider;

  @override
  Future<NewsPageListResponseEntity> getNews() async {
    final response = await provider.getNews();
    if (response.status.hasError) {
      return Future.error(response.statusText!);
    } else {
      return response.body!;
    }
  }
}
```

- Controller

lib/pages/getConnect/controller.dart

```dart
class NewsController extends SuperController<NewsPageListResponseEntity> {
  NewsController({required this.repository});

  final INewsRepository repository;

  @override
  void onInit() {
    super.onInit();

    //Loading, Success, Error handle with 1 line of code
    // append(() => repository.getNews);
  }

  // 拉取新闻列表
  Future<void> getNewsPageList() async {
    append(() => repository.getNews);
  }

  @override
  void onReady() {
    print('The build method is done. '
        'Your controller is ready to call dialogs and snackbars');
    super.onReady();
  }

  @override
  void onClose() {
    print('onClose called');
    super.onClose();
  }

  @override
  void didChangeMetrics() {
    print('the window size did change');
    super.didChangeMetrics();
  }

  @override
  void didChangePlatformBrightness() {
    print('platform change ThemeMode');
    super.didChangePlatformBrightness();
  }

  @override
  Future<bool> didPushRoute(String route) {
    print('the route $route will be open');
    return super.didPushRoute(route);
  }

  @override
  Future<bool> didPopRoute() {
    print('the current route will be closed');
    return super.didPopRoute();
  }

  @override
  void onDetached() {
    print('onDetached called');
  }

  @override
  void onInactive() {
    print('onInative called');
  }

  @override
  void onPaused() {
    print('onPaused called');
  }

  @override
  void onResumed() {
    print('onResumed called');
  }
}
```

- GetView

lib/pages/getConnect/view.dart

```dart
class NewsView extends GetView<NewsController> {
  NewsView({Key? key}) : super(key: key);

  _buildListView(NewsPageListResponseEntity? state) {
    return ListView.separated(
      itemCount: state != null ? state.items!.length : 0,
      itemBuilder: (context, index) {
        final NewsItem item = state!.items![index];
        return ListTile(
          onTap: () => null,
          title: Text(item.title),
          trailing: Text("分类 ${item.category}"),
        );
      },
      separatorBuilder: (BuildContext context, int index) {
        return Divider();
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("GetConnect Page"),
      ),
      body: controller.obx(
        (state) => _buildListView(state),
        onEmpty: Text("onEmpty"),
        onLoading: Center(
          child: Column(
            children: [
              Text("没有数据"),
              ElevatedButton(
                onPressed: () {
                  controller.getNewsPageList();
                },
                child: Text('拉取数据'),
              ),
            ],
          ),
        ),
        onError: (err) => Text("onEmpty" + err.toString()),
      ),
    );
  }
}
```

- Bindings

lib/pages/getConnect/bindings.dart

```dart
class NewsBinding implements Bindings {
  @override
  void dependencies() {
    Get.lazyPut<INewsProvider>(() => NewsProvider());
    Get.lazyPut<INewsRepository>(() => NewsRepository(provider: Get.find()));
    Get.lazyPut(() => NewsController(repository: Get.find()));
  }
}
```

- 路由

lib/common/routes/app_pages.dart

```dart
    GetPage(
      name: AppRoutes.GetConnect,
      binding: NewsBinding(),
      page: () => NewsView(),
    ),
```

### StateMixin

![](GetContentStateMixin.png)

雷同代码不再重复

- 控制器 Mixin 如下

lib/pages/getConnect_stateMixin/controller.dart

```dart
class NewsStateMixinController extends GetxController
    with StateMixin<NewsPageListResponseEntity> {
  final NewsStateMixinProvider provider;
  NewsStateMixinController({required this.provider});

  // 拉取新闻列表
  Future<void> getNewsPageList() async {
    // 获取数据
    final Response response = await provider.getNews();

    // 判断，如果有错误
    if (response.hasError) {
      // 改变数据，传入错误状态，在ui中会处理这些错误
      change(null, status: RxStatus.error(response.statusText));
    } else {
      // 否则，存储数据，改变状态为成功
      var data = NewsPageListResponseEntity.fromJson(response.body);
      change(data, status: RxStatus.success());
    }
  }
}
```

> 这种方式确实简化了很多代码

### GetController + Dio

这种方式就是之前 [Flutter 新闻客户端](https://github.com/ducafecat/flutter_learn_news) 的写法，能复用原来的 dio 代码。

- dio 基础类

lib/common/utils/http.dart

```dart

/*
  * http 操作类
  *
  * 手册
  * https://github.com/flutterchina/dio/blob/master/README-ZH.md
  *
  * 从 3 升级到 4
  * https://github.com/flutterchina/dio/blob/master/migration_to_4.x.md
*/
class HttpUtil {
  static HttpUtil _instance = HttpUtil._internal();
  factory HttpUtil() => _instance;

  late Dio dio;

  HttpUtil._internal() {
    // BaseOptions、Options、RequestOptions 都可以配置参数，优先级别依次递增，且可以根据优先级别覆盖参数
    BaseOptions options = new BaseOptions(
      // 请求基地址,可以包含子路径
      baseUrl: SERVER_API_URL,

      // baseUrl: storage.read(key: STORAGE_KEY_APIURL) ?? SERVICE_API_BASEURL,
      //连接服务器超时时间，单位是毫秒.
      connectTimeout: 10000,

      // 响应流上前后两次接受到数据的间隔，单位为毫秒。
      receiveTimeout: 5000,

      // Http请求头.
      headers: {},

      /// 请求的Content-Type，默认值是"application/json; charset=utf-8".
      /// 如果您想以"application/x-www-form-urlencoded"格式编码请求数据,
      /// 可以设置此选项为 `Headers.formUrlEncodedContentType`,  这样[Dio]
      /// 就会自动编码请求体.
      contentType: 'application/json; charset=utf-8',

      /// [responseType] 表示期望以那种格式(方式)接受响应数据。
      /// 目前 [ResponseType] 接受三种类型 `JSON`, `STREAM`, `PLAIN`.
      ///
      /// 默认值是 `JSON`, 当响应头中content-type为"application/json"时，dio 会自动将响应内容转化为json对象。
      /// 如果想以二进制方式接受响应数据，如下载一个二进制文件，那么可以使用 `STREAM`.
      ///
      /// 如果想以文本(字符串)格式接收响应数据，请使用 `PLAIN`.
      responseType: ResponseType.json,
    );

    dio = new Dio(options);

    // Cookie管理
    CookieJar cookieJar = CookieJar();
    dio.interceptors.add(CookieManager(cookieJar));
  }

  /// restful get 操作
  Future get(
    String path, {
    dynamic? queryParameters,
    Options? options,
  }) async {
    var response = await dio.get(
      path,
      queryParameters: queryParameters,
      options: options,
    );
    return response.data;
  }
}

```

- api 定义

lib/common/apis/news.dart

```dart
/// 新闻
class NewsAPI {
  /// 翻页
  static Future<NewsPageListResponseEntity> newsPageList(
      {NewsRecommendRequestEntity? param}) async {
    var response = await HttpUtil().get(
      '/news',
      queryParameters: param?.toJson(),
    );
    return NewsPageListResponseEntity.fromJson(response);
  }
}
```

- 控制器

lib/pages/getController_dio/controller.dart

```dart
class NewsDioController extends GetxController {
  var newsPageList =
      Rx<NewsPageListResponseEntity>(NewsPageListResponseEntity());

  @override
  void onInit() {
    super.onInit();
    print("onInit");
  }

  @override
  void onClose() {
    super.onClose();
    print("onClose");
  }

  getPageList() async {
    newsPageList.value = await NewsAPI.newsPageList();
  }
}
```

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
