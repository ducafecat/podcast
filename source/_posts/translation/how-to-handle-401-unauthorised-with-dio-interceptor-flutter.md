---
title: Flutter 如何处理401 未授权的 Dio 拦截器
tags: flutter
categories: 译文
date: 2021-07-09 00:00:00
---

![](2021-07-08-22-00-27.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/@wmnkrishanmadushanka/how-to-handle-401-unauthorised-with-dio-interceptor-flutter-60398a914406

## 代码

## 参考

- https://pub.dev/packages/dio/versions/4.0.0
- https://pub.dev/packages/flutter_secure_storage
- https://pub.dev/packages/shared_preferences

## 正文

在本文中，我将解释如何使用 flutter dio (4.0.0)进行网络调用，以及如何在您的 flutter 应用程序中使用刷新令牌和访问令牌来处理授权时处理 401。

在阅读这篇文章之前，我希望你们对颤抖移动应用程序开发有一个基本的了解。

### Basic Authentication flow with refresh and access tokens

![](2021-07-08-21-53-21.png)

正如您在上面的图中所看到的，很明显，在身份验证流中使用刷新和访问令牌时的流程是什么。登录后，您将获得两个称为刷新和访问的标记。此访问令牌快速过期(刷新令牌也过期，但是它将比访问令牌花费更多的时间)。当您使用过期的访问令牌发出请求时，响应中会出现状态码 401(未经授权)。在这种情况下，您必须从服务器请求一个新的令牌，并使用有效的访问令牌再次发出上一个请求。如果刷新令牌也已过期，您必须指示用户登录页面并强制再次登录。

### Dio class

```dart
class DioUtil {
  static Dio _instance;//method for getting dio instance  Dio getInstance() {
    if (_instance == null) {
      _instance = createDioInstance();
    }
    return _instance;
  }

   Dio createDioInstance() {
    var dio = Dio();// adding interceptor    dio.interceptors.clear();
    dio.interceptors.add(InterceptorsWrapper(onRequest: (options, handler) {
      return handler.next(options);
    }, onResponse: (response, handler) {
      if (response != null) {
        return handler.next(response);
      } else {
        return null;
      }
    }, onError: (DioError e, handler) async {

        if (e.response != null) {
          if (e.response.statusCode == 401) {//catch the 401 here
            dio.interceptors.requestLock.lock();
            dio.interceptors.responseLock.lock();
            RequestOptions requestOptions = e.requestOptions;

            await refreshToken();
            Repository repository = Repository();
            var accessToken = await repository.readData("accessToken");
            final opts = new Options(method: requestOptions.method);
            dio.options.headers["Authorization"] = "Bearer " + accessToken;
            dio.options.headers["Accept"] = "*/*";
            dio.interceptors.requestLock.unlock();
            dio.interceptors.responseLock.unlock();
            final response = await dio.request(requestOptions.path,
                options: opts,
                cancelToken: requestOptions.cancelToken,
                onReceiveProgress: requestOptions.onReceiveProgress,
                data: requestOptions.data,
                queryParameters: requestOptions.queryParameters);
            if (response != null) {
              handler.resolve(response);
            } else {
              return null;
            }
          } else {
            handler.next(e);
          }
        }

    }));
    return dio;
  }

  static refreshToken() async {
    Response response;
    Repository repository = Repository();
    var dio = Dio();
    final Uri apiUrl = Uri.parse(BASE_PATH + "auth/reIssueAccessToken");
    var refreshToken = await repository.readData("refreshToken");
    dio.options.headers["Authorization"] = "Bearer " + refreshToken;
    try {
      response = await dio.postUri(apiUrl);
      if (response.statusCode == 200) {
        LoginResponse loginResponse =
            LoginResponse.fromJson(jsonDecode(response.toString()));
        repository.addValue('accessToken', loginResponse.data.accessToken);
        repository.addValue('refreshToken', loginResponse.data.refreshToken);
      } else {
        print(response.toString()); //TODO: logout
      }
    } catch (e) {
      print(e.toString()); //TODO: logout
    }
  }
}
```

以上是完整的课程，我将解释其中最重要的部分。

主要是，正如您在 createDioInstance 方法中看到的，您必须添加一个拦截器来捕获 401。当 401 发生在 error: (DioError e，handler) async {}被调用时。所以你的内心

1. Check the error code(401), 检查错误代码(401) ,
2. Get new access token 获取新的访问令牌

```dart
await refreshToken();
```

上面的代码将调用 refreshToken 方法并在存储库中存储新的刷新和访问令牌。(对于存储库，您可以使用 secure storage or shared preferences)

3. 复制前一个请求并设置新的访问令牌

```dart
RequestOptions requestOptions = e.requestOptions;
Repository repository = Repository();
var accessToken = await repository.readData("accessToken");
final opts = new Options(method: requestOptions.method);
dio.options.headers["Authorization"] = "Bearer " + accessToken;
dio.options.headers["Accept"] = "*/*";
```

4. Make the previous call again

```dart
final response = await dio.request(requestOptions.path,
    options: opts,
    cancelToken: requestOptions.cancelToken,
    onReceiveProgress: requestOptions.onReceiveProgress,
    data: requestOptions.data,
    queryParameters: requestOptions.queryParameters);
```

一旦收到回复，就 call

```dart
handler.resolve(response);
```

然后响应将被发送到您调用 api 的位置，如下所示。

```dart
var dio = DioUtil().getInstance();
final String apiUrl = (BASE_PATH + "payments/addNewPayment/");
var accessToken = await repository.readData("accessToken");
dio.options.headers["Authorization"] = "Bearer " + accessToken;
dio.options.headers["Accept"] = "*/*";//response will be assigned to response variable
response = await dio.post(apiUrl, data: event.paymentRequest.toJson());
```

Thats all. happy coding :)

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
