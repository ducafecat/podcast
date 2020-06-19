---
title: Flutter 实战从零开始 新闻客户端 - 04 YAPI接口管理、RESTful、生成代码、dio封装
date: 2020-03-16 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpg)

## 本节目标

- 前后端分离、契约开发模式
- API 接口管理、工具
- RESTful 接口规范
- TOKEN 安全通讯
- 自动生成 entity 接口实体类
- dio 封装
- localstorage 本地存储
- 密码加密

## 1. 接口管理

### 1.1 前后端分离、契约模式

![](2020-03-17-14-01-41.png)

### 1.2 常见接口管理工具

- yapi
  https://github.com/YMFE/yapi

![](2020-03-17-14-55-25.png)

- easymock
  https://github.com/easy-mock/easy-mock

![](2020-03-17-14-56-43.png)

- RAP2
  https://github.com/thx/RAP

![](2020-03-17-14-54-52.png)

- swagger
  https://swagger.io/

![](2020-03-17-14-54-02.png)

### 1.3 yapi 接口管理工具（猫哥推荐）

http://yapi.demo.qunar.com/

- 输入

![](2020-03-17-14-58-00.png)

- 输出

![](2020-03-17-14-58-20.png)

### 1.4 mock 模拟数据

![](2020-03-17-14-59-40.png)

### 1.5 单元测试

![](2020-03-17-15-00-25.png)

### 1.6 swagger 导入

![](2020-03-17-15-00-49.png)

## 2. restful 接口风格

- [REST wiki](https://en.wikipedia.org/wiki/Representational_state_transfer)
- [理解 RESTful 架构 阮一峰](http://www.ruanyifeng.com/blog/2011/09/restful.html)
- [RESTful API 设计指南 阮一峰](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
- [RESTful API 最佳实践 阮一峰](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)
- [RESTful 架构详解](https://www.runoob.com/w3cnote/restful-architecture.html)

### 2.1 http 操作方式

- GET 取数据
- POST 新建数据
- PUT 更新全部数据
- PATCH 更新部分数据
- DELETE 删除数据

#### 例子:

```
GET /zoos：列出所有动物园
POST /zoos：新建一个动物园
GET /zoos/ID：获取某个指定动物园的信息
PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID：删除某个动物园
GET /zoos/ID/animals：列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物
```

### 2.2 state 状态控制

- 200 OK
- 400 错误的请求，比如数据结构不对
- 401 需要登录认证
- 403 已登录，但是当前资源没有授权
- 404 找不到，地址错误
- 500 服务程序错误
- 502 服务网关错误
- 503 服务挂了
- 504 服务网关超时

### 2.3 优秀实践

- [Github REST API v3](https://developer.github.com/v3/)

## 3. token 安全通讯

### 3.1 基于令牌的安全机制

- 流程

![](2020-03-17-15-58-06.png)

- 思路

![](2020-03-17-16-29-41.png)

### 3.2 Bearer Type Access Token

在通讯 HTTP HEADER 头中加入

```
GET /resource HTTP/1.1
Host: server.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### 3.3 JWT

- https://jwt.io/
- [JSON Web Token 入门教程](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)

![](2020-03-17-16-03-17.png)

## 4. 自动生成 entity

### 4.1 json_serializable （官方）

- https://pub.dev/packages/json_serializable

![](2020-03-17-16-37-25.png)

### 4.2 json to code （猫哥推荐）

- https://app.quicktype.io/

![](2020-03-17-16-35-57.png)

- vscode 插件

https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype

## 5. dio 封装

### 5.1 单例模式

- dio

https://pub.dev/packages/dio

- lib/common/utils/http.dart

单例常见封装方式

```dart
class HttpUtil {
  static HttpUtil _instance = HttpUtil._internal();
  factory HttpUtil() => _instance;

  Dio dio;
  CancelToken cancelToken = new CancelToken();

  HttpUtil._internal() {
    ...
```

### 5.2 维护 token

从本地 storage 中读取

- localstorage

https://pub.flutter-io.cn/packages/localstorage

- getLocalOptions()

```dart
  Options getLocalOptions() {
    Options options;
    String token = StorageUtil().getItem(STORAGE_USER_TOKEN_KEY);
    if (token != null) {
      options = Options(headers: {
        'Authorization': 'Bearer $token',
      });
    }
    return options;
  }
```

### 5.3 处理异常

格式化，错误信息，进行差别对待

- createErrorEntity()

```dart
  ErrorEntity createErrorEntity(DioError error) {
    switch (error.type) {
      case DioErrorType.CANCEL:
        {
          return ErrorEntity(code: -1, message: "请求取消");
        }
        break;
      case DioErrorType.CONNECT_TIMEOUT:
        {
          return ErrorEntity(code: -1, message: "连接超时");
        }
        break;
      case DioErrorType.SEND_TIMEOUT:
        {
          return ErrorEntity(code: -1, message: "请求超时");
        }
        break;
      case DioErrorType.RECEIVE_TIMEOUT:
        {
          return ErrorEntity(code: -1, message: "响应超时");
        }
        break;
      case DioErrorType.RESPONSE:
        {
          try {
            int errCode = error.response.statusCode;
            // String errMsg = error.response.statusMessage;
            // return ErrorEntity(code: errCode, message: errMsg);
            switch (errCode) {
              case 400:
                {
                  return ErrorEntity(code: errCode, message: "请求语法错误");
                }
                break;
              case 401:
                {
                  return ErrorEntity(code: errCode, message: "没有权限");
                }
                break;
              case 403:
                {
                  return ErrorEntity(code: errCode, message: "服务器拒绝执行");
                }
                break;
              case 404:
                {
                  return ErrorEntity(code: errCode, message: "无法连接服务器");
                }
                break;
              case 405:
                {
                  return ErrorEntity(code: errCode, message: "请求方法被禁止");
                }
                break;
              case 500:
                {
                  return ErrorEntity(code: errCode, message: "服务器内部错误");
                }
                break;
              case 502:
                {
                  return ErrorEntity(code: errCode, message: "无效的请求");
                }
                break;
              case 503:
                {
                  return ErrorEntity(code: errCode, message: "服务器挂了");
                }
                break;
              case 505:
                {
                  return ErrorEntity(code: errCode, message: "不支持HTTP协议请求");
                }
                break;
              default:
                {
                  // return ErrorEntity(code: errCode, message: "未知错误");
                  return ErrorEntity(
                      code: errCode, message: error.response.statusMessage);
                }
            }
          } on Exception catch (_) {
            return ErrorEntity(code: -1, message: "未知错误");
          }
        }
        break;
      default:
        {
          return ErrorEntity(code: -1, message: error.message);
        }
    }
  }
```

## 6. 登录调用

### 6.1 编写 api 接口

- lib/common/apis/user.dart

```dart
import 'package:flutter_ducafecat_news/common/entitys/entitys.dart';
import 'package:flutter_ducafecat_news/common/utils/utils.dart';

/// 用户
class UserAPI {
  /// 登录
  static Future<UserResponseEntity> login({UserRequestEntity params}) async {
    var response = await HttpUtil().post('/user/login', params: params);
    return UserResponseEntity.fromJson(response);
  }
}

```

### 6.2 密码加密

- crypto

https://pub.dev/packages/crypto

- lib/common/utils/security.dart

```dart
import 'dart:convert';
import 'package:crypto/crypto.dart';

/// SHA256
String duSHA256(String input) {
  String salt = 'EIpWsyfiy@R@X#qn17!StJNdZK1fFF8iV6ffN!goZkqt#JxO'; // 加盐
  var bytes = utf8.encode(input + salt);
  var digest = sha256.convert(bytes);

  return digest.toString();
}

```

### 6.3 调用接口

- lib/pages/sign_in/sign_in.dart

```dart
  // 执行登录操作
  _handleSignIn() async {
    if (!duIsEmail(_emailController.value.text)) {
      toastInfo(msg: '请正确输入邮件');
      return;
    }
    if (!duCheckStringLength(_passController.value.text, 6)) {
      toastInfo(msg: '密码不能小于6位');
      return;
    }

    UserRequestEntity params = UserRequestEntity(
      email: _emailController.value.text,
      password: duSHA256(_passController.value.text),
    );

    UserResponseEntity res = await UserAPI.login(params: params);

    // 写本地 access_token , 不写全局，业务：离线登录
    // 全局数据 gUser
  }
```

## YAPI 接口管理

http://yapi.demo.qunar.com/

## git 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.4

## 蓝湖设计稿

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

## 参考

- RESTful

  - [REST wiki](https://en.wikipedia.org/wiki/Representational_state_transfer)
  - [理解 RESTful 架构 阮一峰](http://www.ruanyifeng.com/blog/2011/09/restful.html)
  - [RESTful API 设计指南 阮一峰](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
  - [RESTful API 最佳实践 阮一峰](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)
  - [RESTful 架构详解](https://www.runoob.com/w3cnote/restful-architecture.html)

- Flutter packages

  - [localstorage](https://pub.flutter-io.cn/packages/localstorage)
  - [json_serializable](https://pub.dev/packages/json_serializable)
  - [dio](https://pub.dev/packages/dio)
  - [crypto](https://pub.dev/packages/crypto)

- VSCode 插件

  - [Awesome Flutter Snippets](https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets)
  - [Paste JSON as Code](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)

## 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
