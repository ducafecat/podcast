---
title: Flutter 2.2 升级了哪些东西？
tags: flutter
categories: 译文
date: 2021-05-20 00:00:00
---

![](2021-05-20-08-59-40.png)

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

> https://medium.com/flutter/whats-new-in-flutter-2-2-fd00c65e2039

## 正文

2.2 的发布侧重于改进和优化，包括 iOS 性能的提升，Android 延迟组件，为 Flutter web 更新服务工作者等等！

今天是我们让 Flutter 2.2 可用的日子。您可以通过切换到 stable 通道并升级当前的 Flutter 安装，或者转到 Flutter.dev/docs/get-started 来启动新的安装。

尽管距离 Flutter 2 发布只有几个月的时间，我们在 2.2 中还是有很多改进可以分享。这个版本合并了 2,456 个 PRs，关闭了框架、引擎和插件库中的 3,105 个问题。特别大声呼吁 Flutter 社区在广大提供了大量的公关和公关审查，包括 Abhishek01039 谁贡献了最多的 PRs (17)和 xu-baolin，谁审查了最多的 PRs (9) Flutter 2.2。感谢所有贡献者的帮助，使 Flutter 2.2 的稳定通道。没有你我们做不到。

每一个 Flutter 发布到 stable 的新版本都会带来一组新的更新，无论是性能增强、新特性还是 bug 修复。此外，一个版本还包括一些尚未准备好投入生产使用的特性，但我们希望您能够验证它们是否按照您希望的方式工作。最后，每个新版本都附带了一组来自更大的 Flutter 社区的相关工具更新和更新。老实说，最近 Flutter 的每一个新版本都发生了很多事情，我们无法在一篇博客文章中合理地捕捉到所有的内容，但是我们会尽量突出重点。

### 在 stable 下 Flutter 2.2 更新

这个版本涵盖了 Flutter 2 之上的一系列改进，包括跨 Android，iOS 和 web 的更新，新材质图标，文本处理更新，滚动条行为，以及对 TextSpan 小部件的鼠标光标支持，以及如何从单一源代码库中最好地支持多种平台的新指南。所有这些特性现在都可以稳定地使用，并且可以在生产应用程序中使用。它们都是建立在一个新的 Dart 释放上的。

#### Dart 2.13

Flutter 2.2 带有 Dart 2.13 版本。除此之外，这次 Dart 更新包含了一个新的类型别名功能，它可以让你为类型和函数创建别名:

```dart
// Type alias for functions (existing)
typedef ValueChanged<T> = void Function(T value);

// Type alias for classes (new!)
typedef StringList = List<String>;

// Rename classes in a non-breaking way (new!)
@Deprecated("Use NewClassName instead")
typedef OldClassName<T> = NewClassName<T>;
```

类型别名可以为长而复杂的类型提供很好的短名称，它还允许您以非破坏性的方式重命名类。在 Dart 2.13 中还有更多的新功能，请查看 Dart 2.13 发布公告中的详细信息。

https://medium.com/dartlang/announcing-dart-2-13-c6d547b57067

#### Flutter web 更新

Flutter 最新的稳定平台 web 已经在这个版本中得到了改进。

首先，我们使用新的服务工作者加载机制优化了缓存行为，并修复了 main.dart.js 的双重下载。在 Flutter web 的早期版本中，服务工作者在后台下载更新到您的应用程序，同时让您的用户访问您的应用程序的陈旧版本。一旦更新被下载，用户将不会看到这些更改，直到他们刷新浏览器页面几次。在 Flutter 2.2 中，当新的服务工作者检测到一个变化时，用户会等到更新被下载后才能使用这个应用程序，但是之后他们会看到更新，而不需要第二次手动刷新页面。

启用此更改需要您重新生成您的 Flutter 应用程序的 index.html。要做到这一点，保存修改，删除 index.html，然后运行 flutter create。在你的项目目录中重新创建它。

我们还对这两个网页渲染器进行了改进。对于 HTML，我们添加了对字体特性的支持，以支持设置 FontFeature，并使用 canvas api 呈现文本，以便文本在鼠标悬停时出现在正确的位置。对于 HTML 和 CanvasKit，我们增加了对着色遮罩和 computelinetrics 的支持，解决了 Flutter web 和移动应用之间的平价差距。例如，开发人员现在可以使用不透明遮罩来使用着色遮罩执行淡出过渡，并且可以像使用移动应用程序一样使用 computelinetrics。

对于 Flutter 网页，以及 Flutter general，可访问性是我们的最高优先事项之一。根据设计，Flutter 通过构建 SemanticsNode 树来实现可访问性。一旦 Flutter web 应用用户启用了可访问性，框架就会生成一个与 RenderObject DOM 树并行的 DOM 树，并将语义属性转换为 Aira。在这个版本中，我们改进了语义节点的位置，以便在使用转换时缩小移动和桌面 web 应用程序之间的差距，这意味着当小部件使用转换设计时，焦点框应该正确地出现在元素上。要看到这个效果，请看 Victor Tsaran 的视频，他领导了材料设计的可访问性项目，使用 VoiceOver with Flutter Gallery App:

我们还公开了语义节点调试树，在概要文件和释放模式中有一个命令行标志，通过可视化为 web 应用程序创建的语义节点来帮助开发人员调试可访问性。

为了使你自己的 Flutter web 应用程序能够使用这个功能，运行以下命令:

```sh
$ flutter run -d chrome --profile \
 --dart-define=FLUTTER_WEB_DEBUG_SHOW_SEMANTICS=true
```

激活该标志后，您将能够看到小部件顶部的语义节点，这样您就可以调试并查看语义元素是否放置在不应该放置的位置。如果你找到这样的例子，请不要犹豫，提交一份错误报告。

https://goo.gle/flutter_web_issue

虽然我们在支持一组核心可访问性特性方面取得了重大进展，但我们将继续改进可访问性支持。在 2.2 稳定版之外的主版和开发版渠道中，我们增加了一个 API，让开发者可以通过编程方式自动启用应用程序的可访问性，并通过使用带有屏幕阅读器的 Tab 解决问题。

最后，但肯定不是最不重要的，最新版本的 Flutter DevTools 现在支持您的 Flutter 网络应用程序的布局浏览器。

![](2021-05-20-06-08-40.png)

这个更新给你提供了和你的移动和桌面应用程序一样的网页布局调试工具。

#### iOS 页面转换和增量安装

对于 iOS，在这个版本中，我们通过减少 75% 渲染动画画面所需的时间，使 Cupertino 的页面转换更加流畅，而且可能在低端手机上更多。我们不只是寻找最终用户性能的改进，我们也一直在寻找提高开发性能的方法。

在这个版本中，我们在开发过程中实现了增量的 iOS 安装。在我们的基准测试中，我们发现安装升级版 iOS 应用程序的时间减少了 40% ，这样在测试应用程序更改时就会减少周转时间。

#### 使用 Flutter 构建平台自适应应用程序

随着 Flutter 扩展到支持更多稳定的平台，考虑不仅支持不同形式因素的应用程序变得很有用，比如移动设备、平板电脑和桌面，还有不同的输入类型(触摸和鼠标 + 键盘)和带有不同习惯用法的平台，比如导航抽屉和导航系统菜单。我们把能够根据不同目标平台的细节进行调整的应用称为“平台自适应”应用。

为了介绍在构建平台自适应应用程序时需要注意的事项，我们将向您介绍 Kevin Moore 的构建平台自适应应用程序会议。要了解更多详细信息，请查看 flutter.dev 上的平台自适应应用程序指南。

- 构建平台自适应应用
  https://events.google.com/io/session/868dfd56-7f8c-49ee-84ad-ac69a23ba19d?lng=en

- 指南
  https://flutter.dev/docs/development/ui/layout/building-adaptive-apps

最后，对于根据这些原则为多个平台编写的示例应用程序，我们推荐来自 gSkinner 的 Flokk 和 Flutter Folio 应用程序。你可以下载 Flokk 和 Folio 的代码，也可以从各种应用程序商店下载 Flokk 和 Folio，或者直接从浏览器上运行它们。另一个很好的例子是用于创建指南本身的应用程序:

https://flutter.gskinner.com/flokk

https://flutter.gskinner.com/folio

https://flutter.gskinner.com/flokk/#g-download

https://github.com/gskinnerTeam/flutter-folio

Flutter 平台自适应应用指南的 UX 部分是基于新的大屏幕材料指南。这个来自 Material 团队的新指导包括几个主要布局文章的返工，以及对几个组件的更新和更新的设计工具包，所有这些都考虑到了大屏幕。

https://material.io/blog/material-design-for-large-screens

![](2021-05-20-06-15-18.png)

Flutter 的目标一直是让应用程序不仅仅在多个平台上运行; 我们还要等到你的应用程序在所有你的目标平台上运行良好之后才能完成。的支持不仅可以针对多个平台的应用程序，还可以根据屏幕大小、输入模式和每个平台的习惯调整应用程序。

#### 更多 Material 图标

关于 Material guidance 的主题，在这个版本中，我们已经为 Flutter 添加了不止一个而是两个新的 Material 图标，包括一个 Dash 自己的图标！

https://github.com/flutter/flutter/pull/78311

![](2021-05-20-06-16-14.png)

![](2021-05-20-06-16-24.png)

这些更新使你的应用程序的材质图标总数超过 7000 个。如果你很难找到你想要的图标，那么在那些令人尴尬的财富中(谁不会呢?)你可在此 fonts.google.com/icons 按类别及姓名搜寻。

http://fonts.google.com/icons

![按名称搜索 Flutter 材质图标](2021-05-20-06-17-40.png)

一旦你找到了完美的图标，新的“ Flutter”标签会告诉你如何使用它，或者你可以下载这个图标作为应用程序中的独立资产。添加 Dash 到您的 Flutter 应用程序从来没有这么容易。

#### 改进的文本处理

随着我们继续改进 Flutter 以支持每个平台的细节，我们继续推进新的领域，这些领域在移动表单因素上不像在桌面表单因素上那么重要。其中一个领域是文本处理。在这个版本中，我们已经开始重构我们如何处理文本输入，以实现诸如取消在小部件层次结构中冒泡的击键等功能，并通过引入完全自定义与文本动作相关的击键功能。

能够取消击键使得 Flutter 能够在不触发滚动事件的情况下实现诸如使用空格键和箭头键之类的东西，给你的最终用户一个更直观的体验。您可以使用同样的功能来处理击键，然后再将其发送到您自己的应用程序中的父窗口部件。另一个例子是，在这个版本中，你可以在 TextField 和 Flutter 应用程序中的一个按钮之间选择 Tab，而且它正常工作:

```dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Flutter Text Editing Fun',
       home: HomePage(),
     );
}

class HomePage extends StatelessWidget {
 @override
 Widget build(BuildContext context) => Scaffold(
       body: Column(
         children: [
           TextField(),
           OutlinedButton(onPressed: () {}, child: const Text('Press Me')),
         ],
       ),
     );
}
```

![Flutter 2.2 可以取消冒出窗口部件层次结构的击键，例如允许 TAB 从 TextField 更改焦点](2021-05-20-06-19-31.png)

自定义文本操作允许你做一些事情，比如在 TextField 中特殊处理 Enter 键; 例如，你可以触发在聊天客户端发送消息，同时仍然允许通过 Ctrl + Enter 插入一个新行。这些相同的文本操作允许 Flutter 本身提供不同的按键来匹配主机 OS 本身的文本编辑行为，例如，在 Windows 和 Linux 上使用 Ctrl + c，而在 macOS 上使用 Cmd + c。

https://github.com/flutter/flutter/pull/75032

作为一个例子，下面的示例重写了默认的左箭头操作，并为退格键和删除键提供了一个新的操作:

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Flutter TextField Key Binding Demo',
       home: Scaffold(body: UnforgivingTextField()),
     );
}

/// A text field that clears itself if the user tries to back up or correct
/// something.
class UnforgivingTextField extends StatefulWidget {
 @override
 State<UnforgivingTextField> createState() => _UnforgivingTextFieldState();
}

class _UnforgivingTextFieldState extends State<UnforgivingTextField> {
 // The text editing controller used to clear the text field.
 late TextEditingController controller;

 @override
 void initState() {
   super.initState();
   controller = TextEditingController();
 }

 @override
 Widget build(BuildContext context) => Shortcuts(
       shortcuts: <LogicalKeySet, Intent>{
         // This overrides the left arrow key binding that the text field normally
         // has in order to move the cursor back by a character. The default is
         // created by the MaterialApp, which has a DefaultTextEditingShortcuts
         // widget in it.
         LogicalKeySet(LogicalKeyboardKey.arrowLeft): const ClearIntent(),

         // This binds the delete and backspace keys to also clear the text field.
         // You can bind any key, not just those already bound in
         // DefaultTextEditingShortcuts.
         LogicalKeySet(LogicalKeyboardKey.delete): const ClearIntent(),
         LogicalKeySet(LogicalKeyboardKey.backspace): const ClearIntent(),
       },
       child: Actions(
         actions: <Type, Action<Intent>>{
           // This binds the intent that indicates clearing a text field to the
           // action that does the clearing.
           ClearIntent: ClearAction(controller: controller),
         },
         child: Center(child: TextField(controller: controller)),
       ),
     );
}

/// An intent that is bound to ClearAction.
class ClearIntent extends Intent {
 const ClearIntent();
}

/// An action that is bound to ClearIntent that clears the TextEditingController
/// passed to it.
class ClearAction extends Action<ClearIntent> {
 ClearAction({required this.controller});

 final TextEditingController controller;

 @override
 Object? invoke(covariant ClearIntent intent) {
   controller.clear();
 }
}
```

![按左箭头或 ESC 可以清除文本](2021-05-20-06-20-56.png)

我们仍然有更多的工作要做，但我们正在努力给你提供完整的文本编辑操作。我们的目标是，当 Flutter 桌面变得稳定时，你的用户将无法分辨出 Flutter 应用程序中编辑文本与主机操作系统中其他应用程序之间的区别。

#### 自动滚动行为

作为我们持续追求的一部分，让 Flutter 应用在每个平台上都表现得像最好的应用一样，我们在这个版本中重新审视了滚动条。当实际显示滚动条时，安卓和 iOS 是相同的; 默认情况下它们不显示滚动条。另一方面，对于桌面应用程序，当内容大于容器时，通常会自动显示滚动条，这需要您添加一个滚动条父窗口小部件。为了在移动或桌面上获得正确的行为，此版本在必要时自动添加滚动条。

考虑下面的无滚动条代码:

```dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: 'Automatic Scrollbars',
       home: HomePage(),
     );
}

class HomePage extends StatelessWidget {
 @override
 Widget build(BuildContext context) => Scaffold(
       body: ListView.builder(
         itemCount: 100,
         itemBuilder: (context, index) => Text('Item $index'),
       ),
     );
}
```

在桌面上运行时，会出现一个滚动条:

![](2021-05-20-06-22-26.png)

如果您不喜欢滚动条的外观，或者总是显示滚动条，那么可以设置 ScrollBarTheme。如果您不喜欢这种默认行为，可以通过设置 ScrollBehavior 在应用程序范围内或在特定实例上更改它。有关新的默认滚动条行为以及如何将代码迁移到新的最佳实践集的更多细节，请查看 flutter.dev 上的文档。

https://flutter.dev/docs/release/breaking-changes/default-desktop-scrollbars

#### 鼠标 cursors over text spans

在以前的 Flutter 版本中，您可以在任何小部件上添加一个鼠标光标(就像一只手指示可点击的东西)。实际上，Flutter 本身在大多数情况下为您添加那些鼠标光标，比如在所有按钮上添加一个手动鼠标光标。然而，如果你想要一系列富文本，它们具有不同的文本跨度和各自的样式，并且可能有足够长的时间来包装，那么你就不太走运了ーー TextSpan 不是一个小部件，因此不能用作鼠标光标的视觉范围... ... 直到现在！在这个版本中，当你有一个带有手势识别器的 TextSpan 时，你将自动获得相应的鼠标光标:

```dart
import 'package:flutter/gestures.dart';
import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart' as urlLauncher;

void main() => runApp(App());

class App extends StatelessWidget {
 static const title = 'Flutter App';
 @override
 Widget build(BuildContext context) => MaterialApp(
       title: title,
       home: HomePage(),
     );
}

class HomePage extends StatelessWidget {
 @override
 Widget build(BuildContext context) => Scaffold(
       appBar: AppBar(title: Text(App.title)),
       body: Center(
         child: RichText(
           text: TextSpan(
             style: TextStyle(fontSize: 48),
             children: [
               TextSpan(
                 text: 'This is not a link, ',
                 style: TextStyle(color: Colors.black),
               ),
               TextSpan(
                 text: 'but this is',
                 style: TextStyle(color: Colors.blue),
                 recognizer: TapGestureRecognizer()
                   ..onTap = () {
                     urlLauncher.launch('https://flutter.dev');
                   },
               ),
             ],
           ),
         ),
       ),
     );
}
```

现在您可以拥有所需的所有包装文本跨度，并且其中任何带有识别器的文本都将获得适当的鼠标游标。

![](2021-05-20-06-24-38.png)

在这个版本中，TextSpan 还支持 onEnter 和 onExit 以及 mouseCursor。像这样的东西可能看起来很小，但他们有很长的路要走，使 Flutter 应用程序感觉就像用户期望它的感觉。

### Flutter 2.2 updates in preview

除了可用于产品的新功能外，Flutter 2.2 在预览版中提供了一些功能，包括 iOS 着色器编译器性能的改进，Android 延迟组件支持，Flutter 桌面更新，以及索尼的 ARM64 Linux 主机支持。请尝试一下，如果有任何问题请告诉我们。

http://github.com/flutter/flutter/issues

#### Preview: iOS 着色器编译改进

在图形渲染术语中，“着色器”是一个在终端用户设备上可用的 GPU 上编译和运行的程序。自 Skia 图形库诞生以来，Flutter 就一直使用着色器来提供高质量的图形效果，包括颜色、阴影、动画等等。由于 Flutter 的 api 的灵活性，着色器生成和编译实时，与帧工作负载需要他们同步。当编译着色器的时间超过帧预算时，结果对用户来说是显而易见的“ jank”

为了避免混乱，Flutter 提供了在训练过程中缓存着色器的能力，然后打包并捆绑一个应用程序，在第一帧之前编译，同时 Flutter 引擎启动。这意味着预编译的着色器不必在框架工作负载期间进行编译，也不会造成混乱。然而，Skia 最初只在 OpenGL 中实现了这个特性。

因此，当我们在 iOS 上默认启用 Metal 后端以回应苹果不推荐使用 OpenGL 时，根据我们的基准测量，最糟糕的帧时间有所增加，而且用户报告的 jank 也有所增加。我们自己的测量表明，这些报告通常是由于增加了着色器编译时间，增加了 Skia 为 Metal 后端生成的着色器数量，以及编译的着色器无法在运行过程中缓存，以至于 jank 在第一次运行应用程序之后仍然存在。

因此，到目前为止，避免 iOS 上这种混乱的唯一方法是简化场景和动画，这并不理想。

https://github.com/flutter/flutter/issues/79298

然而，现在在开发通道是一个预览的新支持在 Skia 的着色器热身的金属。通过 Skia，Flutter 现在编译绑定着色器之前的第一帧工作量开始。

![跟踪显示在应用程序启动期间发生的预编译](2021-05-20-06-27-32.png)

然而，这个解决方案也有一些警告:

- Skia 仍然为 Metal 后端生成比 OpenGL 后端更多的着色器
- 最终的着色器编译到机器代码仍然与框架工作负载同步进行，但这比作为框架渲染时间的一部分进行整个着色器生成和编译要快
- 生成的机器代码在应用程序第一次运行后被缓存，直到设备重新启动

如果您希望利用应用程序中的这种新支持，可以按照 flutter.dev 上的说明进行操作。

https://flutter.dev/docs/perf/rendering/shader#how-to-use-sksl-warmup

We’re not done with this work, however. On both Android and iOS, this implementation has a few drawbacks:

然而，我们还没有完成这项工作，无论是在安卓还是 iOS 上，这个实现都有一些缺点:

- 部署应用程序的大小更大，因为它包含捆绑着色器
- 应用程序启动延迟更长，因为捆绑着色器需要预编译
- 我们对这种实现所暗示的开发人员的体验并不满意

我们认为最后一个问题是需要解决的最重要问题。特别是，我们查看了执行训练运行的过程，并推理由应用程序大小和应用程序启动延迟造成的权衡过于繁重。因此，我们继续研究如何消除不依赖于此实现的着色器编译 jank 和一般的所有 jank。特别是，我们正在与 Skia 团队合作，以减少它响应 Flutter 的要求所产生的着色器的数量，同时也在调查使用与 Flutter 引擎捆绑在一起的一小套静态定义着色器可以实现多少 Flutter。

你可以跟随这个项目在 Flutter 回购看到我们的进展。

#### Android 延迟组件

对于 Android，这个版本使用了 Dart 的分离 AOT 编译特性，允许 Flutter 应用程序在运行时下载包含提前编译代码和资源的模块。我们将这些可安装的组件中的每一个称为延迟组件。通过将代码和资产的下载推迟到仅在需要时进行，初始安装大小可以显著减少。例如，我们实现了 Flutter Gallery 的一个版本，所有的研究和演示都被推迟了，发现初始安装规模减少了 46% 。

https://github.com/flutter/flutter/pull/76192

在启用延迟组件的情况下进行构建时，Dart 会将只使用 deferred 关键字导入的代码编译成单独的共享库，这些库与资产一起打包到延迟组件中。

延迟组件目前只能在 Android 上使用，这个特性是作为早期预览提供的。了解如何在 flutter.dev 上的新 Deferred 组件页面中实现 Deferred 组件。这个页面还链接到 Flutter 维基上的一个页面，该页面包含了关于这个功能如何工作的深入介绍。请在 Flutter 问题跟踪日志问题。

#### Flutter Windows UWP alpha

在这个版本中，Flutter 的另一个更新是针对桌面爱好者的; 对 Windows UWP 的支持在 dev 通道中已经转移到 alpha 版本(稳定的 2.2 版本之外)。允许你把 Flutter 应用程序带到标准 Windows 应用程序不能运行的设备上，包括 Xbox。要进行尝试，首先需要设置 UWP 先决条件。然后，切换到开发通道，启用 UWP 支持:

https://flutter.dev/desktop#windows-uwp

```
$ flutter channel dev
$ flutter upgrade
$ flutter config — enable-windows-uwp-desktop
```

一旦启用，创建一个 Flutter 应用程序包括一个新的 winuwp 文件夹，它允许你在 UWP 容器中构建和运行你的应用程序:

```
$ flutter create uwp_fun
$ cd uwp_fun
$ flutter pub get
$ flutter run -d winuwp
```

因为你正在构建一个 Windows UWP 应用程序，它在 Windows 的沙箱环境中运行，所以在开发过程中，你需要在本地主机上的应用程序防火墙上打一个洞，以启用热重载和调试器断点等功能。你可以通过 checkknesolation 命令按照扑桌面文档页面上的说明做到这一点。一旦你这样做了，你可以看到你最喜欢的 Flutter 应用程序运行作为一个 UWP 应用程序在 Windows 上。

![](2021-05-20-08-29-49.png)

你最喜欢的 Flutter 应用程序在 Windows UWP 容器中运行

当然，你可以运行更多有趣的 UWP 应用程序，比如这些 Flutter 应用程序运行在 Xbox 上。

我要特别感谢克拉克松，自从我加入 Flutter 队以来，他就一直致力于这项支持。有关 Windows UWP alpha 的更多细节，请查看 flutter.dev/desktop/# Windows-UWP。

http://flutter.dev/desktop/#windows-uwp

#### ARM64 Linux host support from Sony

另一个来自社区成员 HidenoriMatsubayashi 的杰出努力，他是索尼的软件工程师，为目标 ARM64 Linux 提供了支持。这个 PR 可以让你在 ARM64 Linux 机器上建立和运行 Flutter 应用程序。

您最喜欢的 Flutter 应用程序运行在 ARM64 Linux 机器上

https://github.com/HidenoriMatsubayashi

https://github.com/flutter/flutter/pull/61221

![](2021-05-20-08-35-23.png)

很高兴看到 Flutter 社区将 Flutter 带到了 Google 团队无法想象的地方。继续好好干吧！

### Flutter 生态系统和工具更新

Flutter 引擎和框架只是整体经验的一部分。对软件包生态系统和工具的更新对 Flutter 开发人员的体验同样重要。我们在这些领域有一些很棒的更新可以分享。

在生态系统方面，我们有一些新的 Flutter 最喜欢的软件包，以及 FlutterFire 的几个更新，Flutter 对 Firebase 的支持。更好的是，FlutterFire 支持新的 Firebase App Check 预览，所以 Flutter 开发者可以在第一天就利用它。

在工具方面，Flutter DevTools 提供了新的更新，用于优化应用程序的内存占用和提供程序包的新标签。对于 VS Code 和 Android Studio/IntelliJ 的 IDE 插件都有一些值得注意的更新，如果你是一个以 Flutter 为目标的内容作者，那么有一种全新的方式将 DartPad 整合到你的写作中。

最后但并非最不重要的是，有一个新的低代码的应用程序设计和建设工具称为 FlutterFlow，目标 Flutter 和运行在网络上，因为它本身是建立了 Flutter。

#### Flutter Favorite updates

作为这次发布的一部分，Flutter 生态系统委员会一直在努力认证 24 个新的 Flutter 最喜欢的软件包，我们最大的扩展。最新标记的 Flutter 收藏包括:

- 正在生产中:cloud_firestore, cloud_functions, firebase_auth, firebase_core, firebase_crashlytics, firebase_messaging, and 及 firebase_storage
- Flutter Community packages: android_alarm_manager_plus, android_intent_plus, battery_plus, connectivity_plus, device_info_plus, network_info_plus, package_info_plus, sensors_plus, and 及 share_plus
- googleapis package
- win32 package
- intl and 及 characters packages
- Sentry and sentry_flutter
- infinite_scroll_pagination and flutter_native_splash packages

所有这些软件包都已经迁移到 null 安全性，并且在适当的情况下支持 Android、 iOS 和 web。例如，firebase crashlytics 在 web 上没有底层的 SDK，Android alarm manager plus 是专门为 Android 设计的。

Flutter 社区“加上”包提供了一个超集相应的包从 Flutter 小组。例如，电池组在 Flutter 发布之前就已经由 Google 的 Flutter 团队提供，并且已经迁移到了 null 安全性，但是只支持安卓和 iOS。另一方面，Flutter 社区电池 + 软件包支持所有六个 Flutter 平台，包括 web、 Windows、 macOS 和 Linux。颁奖的 Flutter 最喜欢的奖项所有 9 个“加”包代表了一个成熟的 Flutter 社区作为一个整体向前迈出了一大步。比 Google 的工程师团队要大得多。您应该尽快将代码迁移到“附加”包，在未来几周内，Google 的相应包将进行更新，以推荐您这样做。

Googleapi 插件提供了大约 185 个自动生成的 Dart 包装器，用于客户端或服务器端的 Dart 应用程序(包括 Flutter 应用程序)。如果你想了解更多关于这个软件包，作者有一个关于使用 Google api 启动 Flutter 应用程序的 i/o 演讲。

Win32 包是一个工程奇迹，它使用 Dart FFI 包装了大多数常用的 Win32 API 调用，使它们可以被 Dart 代码访问，而不需要 c 编译器或 Windows SDK。随着 Flutter 在 Windows 平台上的流行，win32 包已经成为许多流行插件的关键依赖，包括最流行的路径提供者。作为一个完整性的测试，作者 timsneath 做了一些疯狂的事情，比如在原始的 Win32 中使用原始的 Dart 来实现记事本、 snake 和俄罗斯方块。

![](2021-05-20-08-41-41.png)

俄罗斯方块运行在 Windows 上，只使用 Dart FFI 和 Win32 调用

Win32 包是绝对值得检查，如果你做任何与 Dart 或扑在 Windows。

#### 更新和 Firebase 应用程序检查

FlutterFire，Flutter 对 Firebase 的支持，是 Flutter 使用的最流行的插件集合之一。转化酶已经做了一个巨大的工作，让它生产的 Flutter 2 释放，并继续改善它，从那时起。事实上，自从 FlutterFire 的初始生产版本发布以来，转化酶已经减少了 79% 的未解决问题，并且减少了 88% 的未解决问题。此外，他们不仅在产品质量插件方面做得非常出色，他们还将 beta 质量插件迁移到了空安全性，并且让它们在同一核心上构建和运行，这样你就可以混合和匹配了。

此外，Invertase 还继续为 FlutterFire 插件增加新的功能，包括一些 Flutter 与 Cloud Firebase 整合的更新:

- Typesafe API 空气污染指数 用于读写数据
- 支持 Firebase 本地模拟器套件
- 优化您的数据查询 data bundles 数据包

最后，但并非最不重要的是，FlutterFire 提供了对新 Firebase 产品 beta 版本的支持: Firebase App Check。防火基地应用程序检查保护你的后端资源，如云存储，免受滥用，如帐单欺诈或网络钓鱼。使用 App Check，运行你 Flutter 应用的设备使用一个应用程序身份认证提供商来确认它确实是你的应用程序，并且可能检查它是否运行在一个真实的、未被篡改的设备上。一旦你激活应用程序检查，这个认证就会附加到你的应用程序对 Firebase 后端资源提出的每个请求上。要了解更多信息，请参阅 FlutterFire App Check 文档。

#### Flutter DevTools updates

随着这个版本的发布，Flutter DevTools 有了一些值得注意的更新，包括两个内存跟踪改进和一个全新的选项卡专门为提供者插件。

DevTools 版本中的第一个内存跟踪改进提供了跟踪对象分配位置的能力。这对于在代码中找到内存泄漏的位置非常方便。
Flutter DevTools memory tab allocation stack trace Flutter DevTools 内存选项卡分配堆栈跟踪

![](2021-05-20-08-43-46.png)

第二个是将自定义消息注入内存时间轴的能力。这样你就可以为你的应用程序提供特定的标记，比如在你完成一些内存密集型的工作之前和之后，这样你就可以检查你是否正确地清理了东西。

![](2021-05-20-08-43-56.png)

自定义内存事件

随着 Flutter 应用程序越来越大，我们将继续确保 Flutter 开发者拥有他们需要的工具来跟踪和修复内存泄漏和各种运行时问题。

在使用 Flutter 框架时，你不仅要追踪运行时问题，有时你还要追踪与软件包相关的问题。在 pub.dev 上有超过 15,000 个 flutter 兼容的软件包和插件，随着应用程序使用更多的软件包，这种情况越来越有可能发生。因此，考虑到这一点，我们一直在尝试为 Flutter DevTools 添加一个新的 Provider 标签。实际上，这个选项卡是由提供程序包本身的作者 Remi Roussel 创建的(还有许多其他好东西)。如果你正在运行最新版本的 Flutter DevTools，并且正在调试一个使用提供者插件的 Flutter 应用程序，你将自动获得新的 Provider 标签。

![激活 DevTools Provider 选项卡](2021-05-20-08-44-26.png)

“提供者”选项卡显示与每个提供者关联的数据，包括运行应用程序时的实时更改。如果这还不够惊人的话，它可以让你直接改变数据，作为一种测试你的应用程序的角落情况的方法！

通过使用 Remi 的这个标签，我们学到了一些关于如何更好地支持其他想要做同样事情的软件包作者的知识; 你可以阅读 Remi 如何构建 Provider 标签，以及我们目前关于如何在 Flutter DevTools Plugins 提案中启用更多标签的想法。请给我们您的反馈，并随时联系告诉我们您的计划，一个新的标签在 Flutter 开发工具。

This is only a few of the cool new things in Flutter DevTools in this release. For the complete list, check out the individual announcements here:

这只是 Flutter DevTools 在这个版本中的一些很酷的新东西。完整的列表请点击这里查看个人声明:

- Flutter DevTools 2.1 Release Notes 2.1 发行说明
  https://groups.google.com/g/flutter-announce/c/tCreMfJaJFU/m/38p1BBeiCAAJ

- Flutter DevTools 2.2.1 Release Notes 2.2.1 发行说明
  https://groups.google.com/g/flutter-announce/c/t8opLnUyiFQ/m/dJth-jKxAAAJ

- Flutter DevTools 2.2.3 Release Notes 2.2.3 发行说明
  https://groups.google.com/g/flutter-announce/c/t8opLnUyiFQ/m/YX5Ds_q0AgAJ

#### IDE 插件更新

Visual Studio Code 和 IntelliJ/Android Studio IDE 扩展 for Flutter 在这个版本中也进行了更新。例如，visualstudio 代码扩展现在支持两个附加的 Dart 代码重构: 内联方法和内联局部变量。

![新的 Dart 重构行动内嵌方法](2021-05-20-08-46-00.png)

在 Android Studio/IntelliJ 扩展中，我们添加了将所有堆栈跟踪打印到控制台的功能。

![现在您可以获得所有的堆栈跟踪，而不仅仅是第一个](2021-05-20-08-47-02.png)

这对于那些根本原因可能在不同包中的项目很有帮助，因为以前没有打印这些包。我们已经有了一些想法，可以让这篇文章不那么冗长，所以在未来寻找更多的改变。

有关此版本的 IDE 扩展更改的完整列表，请查看以下公告:

- VS Code extension v3.21 VS Code 扩展 v3.21
  https://groups.google.com/g/flutter-announce/c/gNtKp9c1glU/m/SZYTuwcQBwAJ

- VS Code extension v3.22 VS Code 扩展 v3.22
  https://groups.google.com/g/flutter-announce/c/1XR7baYZOVI/m/y6MGYrGhAAAJ

- Flutter IntelliJ Plugin M55 Release 55 Release
  https://groups.google.com/g/flutter-announce/c/tYwFDPAtLu0/m/FrsntcNNBwAJ

- Flutter IntelliJ Plugin M56 Release 56 发布
  https://groups.google.com/g/flutter-announce/c/EkgiAO4p3UM/m/P32ZXXKfAAAJ

#### DartPad workshops

为了确保我们在快速发展的 Flutter 开发者社区中准备好文档，Dart 和 Flutter 团队总是在寻找改进和扩展创建教育内容的方法。随着这个版本的发布，我们为 DartPad 添加了一个新的、逐步的 UI，开发人员可以使用它来跟随讲师指导的工作坊。

![实践中的 DartPad 工作坊](2021-05-20-08-48-36.png)

通过直接在 DartPad 中添加说明，我们为 i/o 提供了一种有指导的工作坊体验。然而，我们不仅仅是为我们自己的工作坊创建它; 如果您想在您的 Dart 或 Flutter 工作坊中使用它，您可以通过遵循 DartPad 工作坊创作指南来实现。除此之外，你还可以在 Gist 中使用 DartPad 来共享代码，并在你自己的站点中嵌入 DartPad，这已经有一段时间了。

我们希望每个制作 Dart 和 Flutter 内容的人都能够为他们的用户提供丰富的交互式体验。请尝试一下这个新功能，让我们知道你的想法！

#### 社区聚光灯: FlutterFlow

FlutterFlow 是一个“低代码”应用程序设计和开发工具，用于在浏览器中构建应用程序。它提供了一个所见即所得的环境，可以使用 Firebase 的实际数据在多个页面上展示你的应用程序。低代码工具的目标是轻松地完成大多数常见事情，允许您编写尽可能少的行自定义代码。事实上，作为一个演示，他们在不到一个小时的时间里创建了一个整体的多页面移动应用程序，可以用零代码浏览大都会艺术博物馆。

FlutterFlow 输出 Flutter 代码，所以如果你需要添加代码来进一步定制你的应用程序，你可以。你可以在 FlutterFlow.io 上阅读 FlutterFlow 产品的发布。

https://flutterflow.io/blog/launch

### Breaking Changes

一如既往，我们努力减少破坏性更改的数量，在这个版本中，我们已经能够限制它去除这些反对意见:

- 73750 删除已弃用的 BinaryMessages
- 73751 删除已弃用的 TypeMatcher 类

您可以在 flutter.dev 上找到针对这些突发变化的缓解方法。

### 摘要

像往常一样，来自谷歌 Flutter 团队的所有人，我们想说ーー谢谢。感谢你成为社区的一部分，让这一切成为可能。随着超过八分之一的新应用程序在播放商店正在建立 Flutter 和超过 200,000 个 Flutter 应用程序在播放商店，我们的持续增长是令人兴奋的。世界各地的各种规模的应用程序都委托他们的用户界面，飞翼工艺美丽的多平台体验，以满足用户可能在任何地方。

![](2021-05-20-08-51-04.png)

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

![](/img/public-qrcode.png)

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
