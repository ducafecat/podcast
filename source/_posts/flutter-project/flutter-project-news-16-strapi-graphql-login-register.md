---
title: Flutter 实战从零开始 新闻客户端 - 16 strapi + graphql 用户注册、登录、异常处理
date: 2020-07-14 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](2020-07-14-15-06-20.png)

# 本节目标

- 编写 mutation 操作，登录、注册
- graphql 操作类加入异常处理

## 视频

https://www.bilibili.com/video/BV1vt4y1Q7i3/

![](2020-07-20-09-12-19.png)

## 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.16

## strapi 运行环境网盘下载

- 网盘

链接:https://pan.baidu.com/s/13Ujy2hzXp8tSqxCx_4IhVQ
密码:yu82

- 运行

需要用 docker-compose 启动
账号 admin
密码 123456

```sh
# 启动
docker-compose up -d --remove-orphans

# 关闭
docker-compose down
```

## 正文

- 调试地址

http://localhost:1337/graphql

### 注册 graphql

- mutation

```graphql
mutation UserRegister($username: String!, $email: String!, $password: String!) {
  register(input: { username: $username, email: $email, password: $password }) {
    jwt
    user {
      id
      username
      email
      role {
        id
        name
        description
        type
      }
      blocked
      confirmed
    }
  }
}
```

- variables

```json
{
  "username": "dbuser",
  "email": "dbuser@ducafecat.tech",
  "password": "12345678"
}
```

### 登录 graphql

```graphql
mutation UserLogin($identifier: String!, $password: String!) {
  login(input: { identifier: $identifier, password: $password }) {
    jwt
    user {
      id
      username
      email
      role {
        id
        name
        description
        type
      }
      blocked
      confirmed
    }
  }
}
```

- variables

```json
{
  "identifier": "dbuser",
  "password": "12345678"
}
```

> identifier 可以是 username、email ，都是唯一的

### Graphql 请求类加入异常处理

- lib/common/utils/graphql_client.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_ducafecat_news/common/utils/utils.dart';
import 'package:flutter_ducafecat_news/common/values/values.dart';
import 'package:flutter_ducafecat_news/common/widgets/widgets.dart';
import 'package:flutter_ducafecat_news/global.dart';
import 'package:graphql/client.dart';

class GraphqlClientUtil {
  static OptimisticCache cache = OptimisticCache(
    dataIdFromObject: typenameDataIdFromObject,
  );

  static client() {
    HttpLink _httpLink = HttpLink(
      uri: '$SERVER_STRAPI_GRAPHQL_URL/graphql',
    );

    if (Global.profile?.jwt != null) {
      final AuthLink _authLink = AuthLink(
        getToken: () => 'Bearer ${Global.profile.jwt}',
      );
      final Link _link = _authLink.concat(_httpLink);
      return GraphQLClient(
        cache: cache,
        link: _link,
      );
    } else {
      return GraphQLClient(
        cache: cache,
        link: _httpLink,
      );
    }
  }

  /// 错误处理
  static _formatException(BuildContext context, OperationException exception) {
    var statusCode = '';
    try {
      statusCode = exception
          .graphqlErrors[0]?.extensions["exception"]["output"]["statusCode"]
          .toString();
      if (statusCode == '') {
        statusCode = exception.graphqlErrors[0]?.extensions["exception"]["code"]
            .toString();
      }
    } catch (e) {}

    switch (statusCode) {
      case '400': // 重新登录
        toastInfo(msg: "错误请求，提交数据错误！");
        break;
      case '401': // 没有认证
      case '403': // 没有授权
        toastInfo(msg: "账号无效、服务没有授权，请重新登录！");
        return goLoginPage(context);
      // break;
      default:
        toastInfo(msg: exception.toString());
    }
    throw exception;
  }

  // 查询
  static Future query({
    @required BuildContext context,
    @required String schema,
    Map<String, dynamic> variables,
  }) async {
    QueryOptions options = QueryOptions(
      documentNode: gql(schema),
      variables: variables,
    );

    QueryResult result = await client().query(options);

    if (result.hasException) {
      _formatException(context, result.exception);
    }

    return result;
  }

  // 操作
  static Future mutate({
    @required BuildContext context,
    @required String schema,
    Map<String, dynamic> variables,
  }) async {
    MutationOptions options = MutationOptions(
      documentNode: gql(schema),
      variables: variables,
    );

    QueryResult result = await client().mutate(options);

    if (result.hasException) {
      _formatException(context, result.exception);
    }

    return result;
  }
}

```

- 常见错误码:

400 数据提交时间
401 需要登录认证
403 功能需要授权

### Entity 用户

- lib/common/entitys/gql_user.dart

```dart
// 用户登录 - request
class GqlUserLoginRequestEntity {
  GqlUserLoginRequestEntity({
    this.identifier,
    this.password,
  });

  String identifier;
  String password;

  factory GqlUserLoginRequestEntity.fromJson(Map<String, dynamic> json) =>
      GqlUserLoginRequestEntity(
        identifier: json["identifier"],
        password: json["password"],
      );

  Map<String, dynamic> toJson() => {
        "identifier": identifier,
        "password": password,
      };
}

// 用户登录 - request
class GqlUserRegisterRequestEntity {
  GqlUserRegisterRequestEntity({
    this.username,
    this.email,
    this.password,
  });

  String username;
  String email;
  String password;

  factory GqlUserRegisterRequestEntity.fromJson(Map<String, dynamic> json) =>
      GqlUserRegisterRequestEntity(
        username: json["username"],
        email: json["email"],
        password: json["password"],
      );

  Map<String, dynamic> toJson() => {
        "username": username,
        "email": email,
        "password": password,
      };
}

//////////////////////////////////////////////////////////////////

// 用户登录 - response
class GqlUserLoginResponseEntity {
  GqlUserLoginResponseEntity({
    this.jwt,
    this.user,
  });

  String jwt;
  UserEntity user;

  factory GqlUserLoginResponseEntity.fromJson(Map<String, dynamic> json) =>
      GqlUserLoginResponseEntity(
        jwt: json["jwt"],
        user: UserEntity.fromJson(json["user"]),
      );

  Map<String, dynamic> toJson() => {
        "jwt": jwt,
        "user": user.toJson(),
      };
}

// 注册新用户 - response
class GqlUserRegisterResponseEntity {
  GqlUserRegisterResponseEntity({
    this.jwt,
    this.user,
  });

  String jwt;
  UserEntity user;

  factory GqlUserRegisterResponseEntity.fromJson(Map<String, dynamic> json) =>
      GqlUserRegisterResponseEntity(
        jwt: json["jwt"],
        user: UserEntity.fromJson(json["user"]),
      );

  Map<String, dynamic> toJson() => {
        "jwt": jwt,
        "user": user.toJson(),
      };
}

// 用户
class UserEntity {
  UserEntity({
    this.id,
    this.username,
    this.email,
    this.role,
    this.blocked,
    this.confirmed,
  });

  String id;
  String username;
  String email;
  RoleEntity role;
  bool blocked;
  bool confirmed;

  factory UserEntity.fromJson(Map<String, dynamic> json) => UserEntity(
        id: json["id"],
        username: json["username"],
        email: json["email"],
        role: RoleEntity.fromJson(json["role"]),
        blocked: json["blocked"],
        confirmed: json["confirmed"],
      );

  Map<String, dynamic> toJson() => {
        "id": id,
        "username": username,
        "email": email,
        "role": role.toJson(),
        "blocked": blocked,
        "confirmed": confirmed,
      };
}

// 角色
class RoleEntity {
  RoleEntity({
    this.id,
    this.name,
    this.description,
    this.type,
  });

  String id;
  String name;
  String description;
  String type;

  factory RoleEntity.fromJson(Map<String, dynamic> json) => RoleEntity(
        id: json["id"],
        name: json["name"],
        description: json["description"],
        type: json["type"],
      );

  Map<String, dynamic> toJson() => {
        "id": id,
        "name": name,
        "description": description,
        "type": type,
      };
}

```

### API 用户注册、登录

- lib/common/apis/gql_user.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_ducafecat_news/common/entitys/entitys.dart';
import 'package:flutter_ducafecat_news/common/graphql/graphql.dart';
import 'package:flutter_ducafecat_news/common/utils/utils.dart';
import 'package:graphql/client.dart';

/// 新闻
class GqlUserAPI {
  /// 登录
  static Future<GqlUserLoginResponseEntity> login({
    @required BuildContext context,
    @required GqlUserLoginRequestEntity variables,
  }) async {
    QueryResult response = await GraphqlClientUtil.mutate(
        context: context,
        schema: GQL_USER_LOGIN,
        variables: variables.toJson());

    return GqlUserLoginResponseEntity.fromJson(response.data["login"]);
  }

  /// 注册
  static Future<GqlUserRegisterResponseEntity> register({
    @required BuildContext context,
    @required GqlUserRegisterRequestEntity variables,
  }) async {
    QueryResult response = await GraphqlClientUtil.mutate(
        context: context,
        schema: GQL_USER_REGISTER,
        variables: variables.toJson());

    return GqlUserRegisterResponseEntity.fromJson(response.data["register"]);
  }
}

```

## 资源

### 设计稿蓝湖预览

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
