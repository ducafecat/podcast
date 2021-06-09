---
title: 介绍 Flutter 桌面应用 NativeShell
tags: flutter
categories: 译文
date: 2021-06-09 00:00:00
---

![](2021-06-09-08-59-44.png)

## 猫哥说

看到这张图片，我就感觉脖子酸。。。我这样摆过，虽然看起来很 cool，然后你的脖子要上下调整，这比左右调整费事。

今天推荐阅读的是关于 Flutter 桌面开发，下面有原文、代码 Git 链接。

开始前我想起 React Native 说过的一句话 《用同样的方法在多端开发》，这句话很玄妙又很正确，就是说不是让你同一套代码同时能编译成 ios android web windows linux ...，只是代码的高度复用，所以有的同学在做多端架构时就要注意拆包了。

比如 你的业务代码、API、Entity、功能函数 很多情况下是复用的。但是各个平台的底层、交互体验是个性化。

不要被在 ios android 两端平滑兼容所欺骗。

今天介绍 NativeShell 这个桌面程序，这个程序演示了 多窗口、模式对话框、拖拽、菜单、工具栏、文件操作、系统 API 调用的例子，如果你正在做研究，这是一个很好参考。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## 原文

> https://matejknopp.com/post/introducing-nativeshell/

## 视频

## 代码

https://github.com/nativeshell/app_template

## 参考

- http://airflow.app/
- https://airflow.app/remote-app/
- https://github.com/nativeshell/examples/blob/main/src/file_open_dialog.rs#L18
- https://github.com/nativeshell/examples/blob/main/lib/pages/drag_drop.dart
- https://github.com/nativeshell/nativeshell/blob/main/nativeshell/src/shell/platform/win32/drag_com.rs

## 正文

自从我第一次看到 Turbo Vision，我就对桌面应用程序感兴趣。DOS 中那些文本模式可调整大小的窗口对我来说就像魔术一样。它激发了人们对用户界面框架的兴趣，这种框架在 20 多年后依然很强大。

在过去十年左右的时间里，人们的注意力主要转移到了网络和移动设备上，这并没有让我感到特别高兴。所以我觉得是时候从阴影中爬出来，离开我的舒适区，试着把一些聚光灯带回它应该在的地方。到桌面上去！:)

### Flutter 之路

我最后一个处理的桌面应用程序是(现在仍然是) Airflow。它是 Qt 和大量平台特定代码的混合体。如果我自己这么说的话，我对最终的结果非常满意，但是开发人员的经验和整体生产力还有很多需要改进的地方。

http://airflow.app/

大约两年前，我需要一个适用于 iOS 和安卓系统的气流应用程序。经过几个原型后，决定做出，我去 Flutter。我确实喜欢认为我有自己的 UI 开发经验，毕竟，我在不同的平台上使用过十几个 GUI 框架，所以现在没有什么能让我感到惊讶的了，对吧？错了。他们所有人中最大的惊喜就是和 Flutter 一起工作的感觉是多么的好。在我的一生中，从来没有，一次也没有，建立用户界面这么有意义。

https://airflow.app/remote-app/

如果我能用这种方式构建桌面应用程序，岂不是很神奇？当然，这是可能的，但现实是一个严酷的情妇，在那个时候桌面嵌入仍然处于婴儿期。那就回到 Qt 吧。但这个念头一直在我脑海中挥之不去。

一两年过去了，很多事情发生了变化。还有很多工作要做，但是桌面嵌入已经成熟了很多，而且 Flutter on desktop 已经开始成为一个可行的选择。

### Flutter desktop 嵌入

现在你可能会问: Matt，Flutter 不是已经嵌入了桌面吗? 那么这一切都是为了什么呢？

是的，的确如此。NativeShell 就建立在他们之上。您可以将 Flutter 桌面嵌入程序想象为一个平台视图组件(想想 GtkWidget、 NSView 或者，恕我直言，HWND)。它处理鼠标和键盘输入，绘画，但它不尝试管理窗口，或 Flutter 引擎/隔离。或者做一些像平台菜单和拖放的事情。为了使事情更加复杂，Flutter 嵌入在每个平台上都有完全不同的 API。因此，如果您希望为一些低级代码创建引擎或注册平台通道处理程序，则需要为每个平台分别执行此操作。

https://nativeshell.dev/

### 这就是 NativeShell 介入的地方

从 Flutter 桌面嵌入结束的地方开始。它为现有的 Flutter 嵌入提供了一个一致的、与平台无关的 API。它管理引擎和窗户。它提供了拖放支持，对平台菜单的访问，以及其他超出 Flutter 嵌入范围的功能。并且它通过简单易用的 Dart API 公开了所有这些。

NativeShell 是用铁锈写的。锈是伟大的，因为它可以让你编写高效的低级平台特定的代码，如果你需要，但它也让你使用 NativeShell，而不必知道任何锈。简单地执行货物运输是所有需要让事情进行。Cargo 是 Rust 软件包管理器(就像 pub 是用于 Dart 的) ，它负责下载和构建所有依赖项。

### 开始

- Install Rust
  https://www.rust-lang.org/tools/install

- Install Flutter
  https://flutter.dev/docs/get-started/install

- 在 Flutter 中启用桌面支持(为您的平台选择一个)

```sh
$ flutter config --enable-windows-desktop
$ flutter config --enable-macos-desktop
$ flutter config --enable-linux-desktop
```

- Switch to Flutter Master

```sh
$ flutter channel master
$ flutter upgrade
```

在这之后，你应该可以开始了:

```sh
$ git clone https://github.com/nativeshell/examples.git
$ cd examples
$ cargo run
```

NativeShell 透明地集成了 Flutter 建立过程和货物。如果铁锈和飞镖之神在对你微笑，这就是你现在应该看到的:

![](2021-06-09-08-38-25.png)

### Platform Channels

如果您需要从 Flutter 应用程序调用本机代码，这两个选项是平台通道或 FFI。对于一般使用的平台通道是预先设计好的，因为它们更容易使用，并且能够在平台和 UI 线程之间适当地传递消息。

这就是使用 NativeShell 注册平台通道处理程序的效果(我保证，这里也是惟一的 Rust 代码)

```rust
fn register_example_channel(context: Rc<Context>) {
    context
        .message_manager
        .borrow_mut()
        .register_method_handler("example_channel", |call, reply, engine| {
            match call.method.as_str() {
                "echo" => {
                    reply.send_ok(call.args);
                }
                _ => {}
            }
        });
}
```

为了直接使用现有的平台嵌入 API 来完成这项工作，您需要使用平台特定的 API 为每个平台分别编写这些代码。然后确保每次创建新引擎时都注册处理程序(关闭引擎时可能注销)。

使用 NativeShell，您只需注册处理程序一次，它可以从任何引擎调用。消息可以通过 Serde 被透明地序列化和反序列化为 Rust 结构(使用 Flutter 的标准方法编解码格式)。

https://github.com/nativeshell/examples/blob/main/src/file_open_dialog.rs#L18

### Window Management

想必你希望你的桌面应用程序有多个窗口？NativeShell 已经掩护你了。调整窗口大小到内容或设置最小的窗口大小，使 Flutter 布局不底流？它也能做到这一点。它还确保只在内容准备好后才显示窗口，从而消除了丑陋的闪烁。

目前，每个窗口作为单独的独立窗体运行。NativeShell 为创建窗口、设置和调整几何形状、更新样式和窗口标题提供了 API。它还提供了便于在窗口之间进行通信的 API。

视频可以根据内容调整大小，也可以根据内容大小调整大小。

- 多窗口

![](2021-06-09-08-40-27.png)

- 模式对话框

![](2021-06-09-08-40-59.png)

这将是 Dart 中如何创建和管理多个窗口的最小演示:

```dart
void main() async {
  runApp(MinimalApp());
}

class MinimalApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // Widgets above WindowWidget will be same in all windows. The actual
    // window content will be determined by WindowState
    return MaterialApp(
      home: WindowWidget(
        onCreateState: (initData) {
          WindowState? context;
          context ??= OtherWindowState.fromInitData(initData);
          // possibly no init data, this is main window
          context ??= MainWindowState();
          return context;
        },
      ),
    );
  }
}

class MainWindowState extends WindowState {
  @override
  Widget build(BuildContext context) {
    return TextButton(
      onPressed: () async {
        // This will create new isolate for a window. Whatever is given to
        // Window.create will be provided by WindowWidget in new isolate
        final window = await Window.create(OtherWindowState.toInitData());
        // you can use the window object to communicate with newly created
        // window or register handlers for window events
        window.closeEvent.addListener(() {
          print('Window closed');
        });
      },
      child: Text('Open Another Window'),
    );
  }
}

class OtherWindowState extends WindowState {
  @override
  Widget build(BuildContext context) {
    return Text('This is Another Window!');
  }

  // This can be anything that fromInitData recognizes
  static dynamic toInitData() => {
        'class': 'OtherWindow',
      };

  static OtherWindowState? fromInitData(dynamic initData) {
    if (initData is Map && initData['class'] == 'OtherWindow') {
      return OtherWindowState();
    }
    return null;
  }
}
```

### Drag & Drop

很难想象还有哪个桌面用户界面框架不支持拖放操作。NativeShell 支持拖放文件路径、 url、自定义 Dart 数据(由 StandardMethodCodec 实现序列化) ，甚至可以扩展以处理自定义平台特定格式。

它应该很容易使用，而且我对它的结果很满意，尽管它确实涉及到编写一些看起来非常吓人的代码。

![](2021-06-09-08-43-26.png)

https://github.com/nativeshell/examples/blob/main/lib/pages/drag_drop.dart

https://github.com/nativeshell/nativeshell/blob/main/nativeshell/src/shell/platform/win32/drag_com.rs

### Popup Menu

很多框架和应用程序都犯了这样的错误，这常常让我感到惊讶。直到最近 Firefox 才开始在 macOS 上使用本地弹出菜单。无论你的应用程序多么精致，如果你的菜单出错了，你就会感觉不对劲。

允许你轻松地创建和显示上下文菜单。考虑到菜单系统的强大功能，这个菜单 API 看似简单。菜单是反应性的。你可以要求重建菜单，而可见和 NativeShell 将计算三角洲和只更新菜单项实际上已经改变。

![](2021-06-09-08-44-37.png)

```dart
  int _counter = 0;

  void _showContextMenu(TapDownDetails e) async {
    final menu = Menu(_buildContextMenu);

    // Menu can be updated while visible
    final timer = Timer.periodic(Duration(milliseconds: 500), (timer) {
      ++_counter;
      // This will call the _buildContextMenu() function, diff the old
      // and new menu items and only update those platform menu items that
      // actually changed
      menu.update();
    });

    await Window.of(context).showPopupMenu(menu, e.globalPosition);

    timer.cancel();
  }

  List<MenuItem> _buildContextMenu() => [
        MenuItem(title: 'Context menu Item', action: () {}),
        MenuItem(title: 'Menu Update Counter $_counter', action: null),
        MenuItem.separator(),
        MenuItem.children(title: 'Submenu', children: [
          MenuItem(title: 'Submenu Item 1', action: () {}),
          MenuItem(title: 'Submenu Item 2', action: () {}),
        ]),
      ];
```

### MenuBar

可能是 NativeShell 中我最喜欢的功能。在 macOS 上，它呈现为空窗口小部件，而是将菜单放在系统菜单栏(在屏幕顶部)。在 Windows 和 Linux 上，它使用 Flutter 小部件呈现顶级菜单项，然后使用本机菜单处理其余部分。这意味着菜单栏可以位于小部件层次结构中的任何位置，它不局限于窗口的顶部，也不依赖于 GDI 或 Gtk 来绘制 iself。

它支持鼠标跟踪和键盘导航，就像普通系统菜单栏一样，但没有任何限制。

![](2021-06-09-08-45-43.png)

### 现在情况如何

NativeShell 正在被沉重的开发。事情很可能会破裂。迫切需要更多的文档和示例。但我认为它的形状可能对某些人有用。

所有三个支持的平台(macOS，Windows，Linux)都具有完全的同等功能。

https://github.com/nativeshell/nativeshell/tree/main/nativeshell/src/shell/platform/macos

https://github.com/nativeshell/nativeshell/tree/main/nativeshell/src/shell/platform/win32

https://github.com/nativeshell/nativeshell/tree/main/nativeshell/src/shell/platform/linux

如果你一路走到了这里，你可以继续 nativeshell.dev。

https://nativeshell.dev/

感谢您的反馈！

https://github.com/nativeshell/nativeshell/issues

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
