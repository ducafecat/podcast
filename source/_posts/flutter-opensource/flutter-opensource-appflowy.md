---
title: Flutter开源项目 - appFlowy 真的是 Notion 的替代品?
tags: flutter
categories: 开源
date: 2021-12-01 22:20:14
---

![](2021-12-01-22-22-25.png)

## git

https://www.appflowy.io/

https://github.com/AppFlowy-IO/appflowy

![](2021-12-01-22-23-16.png)

![](2021-12-01-22-23-59.png)

> 也就 1 周 star 9.8k，我以为他是刷的。

## 目标&特色

- 目标替代 Notion

- 数据 100% 自己管理

- 开源方式提供，你可以自己改

- 多平台支持

- 原生体验，估计是用了 flutter 关系

## 编译运行

- 1. git clone

```
git clone https://github.com/AppFlowy-IO/appflowy.git
```

- 2. 安装 rust

```

cd appflowy/frontend

make install_rust

source $HOME/.cargo/env

make install_cargo_make

cargo make install_targets

```

- 3. 切换 flutter dev

```
flutter channel dev

or

fvm use dev
fvm global dev

```

- 4. flutter 启用 desktop

```
# for windows
flutter config --enable-windows-desktop

# for macos
flutter config --enable-macos-desktop

# for linux
flutter config --enable-linux-desktop
```

- 5. 用 vscode 或者其它 idea 打开

```
open appflowy/frontend
```

![](2021-12-01-23-31-18.png)

![](2021-12-01-22-30-35.png)

## 代码架构

- 技术选型

  - flutter: 多端适配

  - rust: ffi 平台接口、服务端

- flutter 端 `frontend/app_flowy`

![](2021-12-01-22-41-01.png)

- 前端业务 rust 层 `frontend/rust-lib`

![](2021-12-01-22-43-09.png)

- 共享库 shared-lib `shared-lib`

![](2021-12-01-22-44-58.png)

- 后端 rust api `backend`

![](2021-12-01-22-45-48.png)

## 参阅技术说明 `doc`

- 系统设计说明 `doc/APPFLOWY_SYSTEM_DESIGN.md`

```
                        Frontend                                                     FLowySDK
                                                             │                                              ┌─────────┐
                                                             │                                          ┌7─▶│Handler A│
                                                             │                                          │   └─────────┘
                                                             │                             ┌─────────┐  │   ┌─────────┐
┌──────┐    ┌────┐    ┌──────────────┐                       │                        ┌───▶│Module A │──┼──▶│Handler B│
│Widget│─1─▶│Bloc│─2─▶│ Repository A │─3─┐                   │                        │    └─────────┘  │   └─────────┘
└──────┘    └────┘    └──────────────┘   │                   │                        │                 │   ┌─────────┐
                      ┌──────────────┐   │    ┌───────┐    ┌─┴──┐     ┌───────────┐   │    ┌─────────┐  └──▶│Handler C│
                      │ Repository B │───┼───▶│ Event │─4─▶│FFI │─5──▶│Dispatcher │─6─┼───▶│Module B │      └─────────┘
                      └──────────────┘   │    └───────┘    └─┬──┘     └───────────┘   │    └─────────┘
                      ┌──────────────┐   │                   │                        │
                      │ Repository C │───┘                   │                        │    ┌─────────┐
                      └──────────────┘                       │                        └───▶│Module C │
                                                             │                             └─────────┘
                                                             │
                                                             │
```

Here are the event flow:

1. User click on the `Widget`(The user interface) that invokes the `Bloc` actions
2. `Bloc` calls the repositories to perform additional operations to handle the actions.
3. `Repository` offers the functionalities by combining the event, defined in the `FlowySDK`.
4. `Events` will be passed in the `FlowySDK` through the [FFI](https://en.wikipedia.org/wiki/Foreign_function_interface) interface.
5. `Dispatcher` parses the event and generates the specific action scheduled in the `FlowySDK` runtime.
6. `Dispatcher` find the event handler declared by the modules.
7. `Handler` consumes the event and generates the response. The response will be returned to the widget through the `FFI`.

The event flow will be discussed in two parts: the frontend implemented in flutter and the FlowySDK implemented in Rust.

- linux 编译说明 `doc/BUILD_ON_LINUX.md`

- windows 编译说明 `doc/BUILD_ON_WINDOWS.md`

- 贡献参与说明 `doc/CONTRIBUTING.md`

- DDD 设计说明 `doc/DOMAIN_DRIVEN_DESIGN.md`

```
    ┌──────────────────────────────────────────────────┐         ─────────▶
    │                Presentation Layer                │──┐      Dependency
    └──────────────────────────────────────────────────┘  │
                              │                           │
                              ▼                           │
    ┌──────────────────────────────────────────────────┐  │
    │                Application Layer                 │  │
    └──────────────────────────────────────────────────┘  │
                              │                           │
                              ▼                           │
    ┌──────────────────────────────────────────────────┐  │
    │                   Domain Layer                   │◀─┘
    └──────────────────────────────────────────────────┘
                              ▲
                              │
    ┌──────────────────────────────────────────────────┐
    │               Infrastructure Layer               │
    └──────────────────────────────────────────────────┘
```

**Presentation Layer**:

- Responsible for presenting information to the user and interpreting user commands.
- Consists of Widgets and also the state of the Widgets.

**Application Layer**:

- Defines the jobs the software is supposed to do. (Shouldn't find any UI code or network code)
- Coordinates the application activity and delegates work to the next layer down.
- It doesn't contain any complex business logic but the basic validation on the user input before
  passing to the other layer.

**Domain Layer**:

- Responsible for representing concepts of the business.
- Manages the business state or delegated to the infrastructure layer.
- Self contained and it doesn't depend on any other layers. Domain should be well isolated from the
  other layers.

**Infrastructure Layer**:

- Provides generic technical capabilities that support the higher layers. It deals with APIs, persistence and network, etc.
- Implements the repository interface and hiding the complexity of the Domain layer.

* 编辑器 `doc/EDITOR.md`

* roadmap

https://trello.com/b/NCyXCXXh/appflowy-roadmap

## 看看本地文件在哪里

- 找到 sqlite

![](2021-12-01-22-58-59.png)

- 搜索本地

```
find ~ -iname 'flowy-database.db'

~/Library/Containers/com.appflowy.macos/Data/Documents/flowy/fd3ada7d-7653-4196-90e1-7de0019627bc/flowy-database.db
```

## flutter 插件

- flutter-quill

https://github.com/singerdmx/flutter-quill

富文本编辑器

![](2021-12-01-23-27-48.png)

- freezed

https://pub.flutter-io.cn/packages/freezed

数据 model 生成器，支持注解方式

- flutter_colorpicker

https://pub.flutter-io.cn/packages/flutter_colorpicker

颜色选取工具

![](2021-12-01-23-11-05.png)

- styled_widget

https://pub.flutter-io.cn/packages/styled_widget

简化小组件定义

![](2021-12-01-23-14-32.png)

```dart
Icon(OMIcons.home, color: Colors.white)
  .padding(all: 10)
  .decorated(color: Color(0xff7AC1E7), shape: BoxShape.circle)
  .padding(all: 15)
  .decorated(color: Color(0xffE8F2F7), shape: BoxShape.circle)
  .padding(all: 20)
  .card(
    elevation: 10,
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(20),
    ),
  )
  .alignment(Alignment.center)
  .backgroundColor(Color(0xffEBECF1));
```

- get_it

https://pub.flutter-io.cn/packages/get_it

全局访问你的业务对象，你可以拆分业务和 UI

```dart
// 定义

final getIt = GetIt.instance;

void setup() {
  getIt.registerSingleton<AppModel>(AppModel());

// Alternatively you could write it if you don't like global variables
  GetIt.I.registerSingleton<AppModel>(AppModel());
}

// 使用

MaterialButton(
  child: Text("Update"),
  onPressed: getIt<AppModel>().update   // given that your AppModel has a method update
),

```

## 总结

- 看到人家的架构，感觉自己弱爆了，我还是先领域分层设计做做干净
- flutter bloc , rust ffi web protobuf 感觉还是成熟的选择
- 如果持续更新的话，我也很想看看架构的演变
- 现阶段估计还是在测试架构设计，应该不会上很多功能

© 猫哥

- https://ducafecat.tech/

- https://github.com/ducafecat

- 微信群 ducafecat

- b 站 https://space.bilibili.com/404904528

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
