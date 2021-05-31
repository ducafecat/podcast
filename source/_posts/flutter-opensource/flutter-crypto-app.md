---
title: Flutter开源项目 - 加密币客户端 flutter-crypto-app
tags: flutter
categories: 开源
date: 2021-06-01 00:00:00
---

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 猫哥说

节日好伙伴们，今天这个项目推荐给大家，主要是用了 Riverpod 状态管理，Freezed 代码生成器, Flutter 2.2 空安全

还有就是 写了 单元测试 集成测试 Github Action，大家可以学习下。

![](2021-06-01-05-50-29.png)

## 代码

https://github.com/salvadordeveloper/flutter-crypto-app

## 参考

- https://flutter.dev/docs
- https://riverpod.dev/docs/getting_started/
- https://docs.cryptowat.ch/rest-api/

## 正文

### 特性功能

- API REST (CryptoWatch) restful 拉取数据
- Linear Graph View (Hour, Day, Week, etc) 图
- OHLC Graph 图
- Search 搜索
- Light / Dark Theme 样式主题
- Multi Lenguage 多语言
- Exchange Selection 交易
- Favorite Pair 收藏

### 技术栈

- Flutter 2.2.0
- Riverpod + Hooks 状态管理
- Freezed 代码生成器
- Dio http 通讯

### 测试

- Unit Testing (flutter_test)
- Integration Testing (integration_test)
- Mock Data (http_mock_adapter)
- Github Actions (iOS & Android Integration Test)

### 屏幕截图

![](2021-06-01-05-54-39.png)

![](2021-06-01-05-54-48.png)

![](2021-06-01-05-55-05.png)

![](2021-06-01-05-55-14.png)

![](2021-06-01-05-55-22.png)

![](2021-06-01-05-55-29.png)

### 项目安装

下载代码

```
git clone https://github.com/salvadordeveloper/flutter-crypto-app
```

安装包

```
flutter pub get
```

去申请 https://cryptowat.ch/zh-cn/ 账号 api

替换 API_KEY

```
API_KEY={CryptoWatch_KEY}
```

生成代码

```
flutter pub run build_runner build --delete-conflicting-outputs
```

运行 app

```
flutter run
```

单元测试

```
flutter test
```

集成测试

```
flutter drive --driver=test_driver/integration_test.dart --target=integration_test/main_test.dart
```

## 参考

- https://flutter.dev/docs
- https://riverpod.dev/docs/getting_started/
- https://docs.cryptowat.ch/rest-api/

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
