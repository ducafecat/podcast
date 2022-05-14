---
title: 译 10 个 Flutter 组件推荐 - 2
tags: flutter
categories: 译文
date: 2022-05-14 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220514091335.png)

今天，我们再次推荐 package。我们主要处理用户界面和图标包，所以... 让我们去和愉快的阅读！

## 原文

> https://tomicriedel.medium.com/10-flutter-tips-part4-10-season-2-260e23214e3f

## 正文

### swipe_to

https://pub.dev/packages/swipe_to

如果你想给任何小工具添加简单的滑动手势，滑动到是一个很好的软件包。这个软件包的优点是用户不必点击动作，但是当滑动时动作会立即执行。

这里有一个例子可以更好地理解它:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/967711800dc70be72fc717e94af2162454a347141168fd7dc10287437b5b257c.gif)

但是... ... 我怎样才能将这个令人难以置信的包集成到我的代码中呢？

好吧，这很简单:

```dart
SwipeTo(
    child: Container(
        padding: const EdgeInsets.symmetric(vertical: 10.0, horizontal: 8.0),
        child: text('👈🏿 Swipe me Left | YOU |Swipe me right 👉🏿'),
    ),
    onLeftSwipe: () {.
        print('Callback from Swipe To Left');
    },
    onRightSwipe: () {
        print('Callback from Swipe To Right');
    },
),
```

看，一点都不难，对吧？

### rename

https://pub.dev/packages/rename

当然，你希望你的应用程序在发布之前有一个很酷的名字。但是，让我们面对现实吧，如果您为每个平台手动执行这个过程，那么这个过程可能会非常痛苦。

这就是为什么存在着重命名。这个包允许你在几秒钟内在任何平台上重命名你的应用程序。以下是它的工作原理:

1.  在命令行中执行这个命令:

```sh
pub global activate rename
```

2. 现在执行这两个命令:

```dart
pub global run rename --bundleId com.myDomain.myApp
pub global run rename --appname "MyApp"
```

这两个命令的第一个设置你的应用程序的 BundleId。这是你的领域，只不过是倒过来的。如果你没有域名，把它设置成你的名字或者你公司的名字。但无论如何，如果这个域名没有被占用，那将是有利的。

第二个命令正确地重命名应用程序。你会在你的主屏幕上看到这个名字，在 iOS 和 Android 或者其他平台上。

你可以随心所欲地更改名字。

还有更多的可能性，比如只为一个特定的平台重命名一个应用程序。

### Hidable

https://pub.dev/packages/hidable

Hidable 可以让你在用户滚动时隐藏任何窗口小部件。这对于很多事情都是非常有用的，所以现在我们来看看如何实现这个包:

1.  首先，创建一个 ScrollController。

```dart
final ScrollController scrollController = ScrollController();
```

2。现在将 scrollController 作为控制器添加到可滚动的小部件中。下面是一个 ListView 的例子:

```dart
ListView.separated(
// General scroll controller which makes bridge between// This ListView and Hidable widget.
  controller: scrollController,
  itemCount: colors.length,
  itemBuilder: (_, i) => container(
     height: 50,
     color: colors[i].withOpacity(.6),
  ),
  separatorBuilder: (_, __) =>const SizedBox(height: 10),
),
```

3。第三步，也是最后一步，用 Hidable widget 包装任何组件。在小部件中，指定 scrollController 作为控制器。可租用小部件还有一些其他属性。我们现在使用 AppBar 来完成这个过程。

```dart
Hidable(
  controller: scrollController,
  wOpacity:true,// As default it's true.
  size: 56,// As default it's 56.
  child: BottomNavigationBar(...),
),
```

就是这样。现在你已经成功地实现了可租用小工具！

### Flutter Sticky Header

有时候你在应用程序中滚动，然后突然间类别卡在屏幕顶部，这意味着你总是知道你在哪里。你有没有想过在 Flutter 是怎么工作的？不是吗？好吧，那么无论如何还是阅读这一部分，因为它非常有趣

如果你还在读，我会给你详细展示我的意思:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a66ec0c60d8a1fa3ffb74809134a4d0974b354d1298414a0a561d48a3f430489.gif)

我想我们现在都知道我的意思了。

但是我如何在 Flutter 中实现这个呢？嗯，非常简单: 带有 flutter/sticky/header 包。这个软件包可以很容易地为你的应用程序添加这样一个伟大的功能。让我来告诉你如何在代码中实现它:

```dart
SliverStickyHeader(
  header: Container(
    height: 60.0,
    color: Colors.lightBlue,
    padding: EdgeInsets.symmetric(horizontal: 16.0),
    alignment: Alignment.centerLeft,
    child: Text(
      'Header #0',
      style:const TextStyle(color: Colors.white),
    ),
  ),
  sliver: SliverList(
    delegate: SliverChildBuilderDelegate(
      (context, i) => ListTile(
            leading: CircleAvatar(
              child: Text('0'),
            ),
            title: Text('List tile #$i'),
          ),
      childCount: 4,
    ),
  ),
);
```

但是还有其他方法可以实现类似的东西，比如使用 sliversticheader.builder。

### Preload Page View

每个人都知道 PageViews。它们非常方便，甚至可以在 TikTok 上找到(如果它是用 Flutter 建造的)。但是如果你在 PageView 上有来自服务器的内容呢。当用户进入一个页面后，加载的内容却要花费很长的时间，这是不好的。这个问题通过预加载 \_ 页面视图来解决。正如它的名字所说，你可以使用这个包来预加载任意数量的页面，这样你就不会有永恒的加载屏幕。这个小部件的实现非常简单，工作原理几乎和一般的 PageView 小部件一样:

```dart
@override
  Widget build(BuildContext context) {
    return new PreloadPageView.builder(
      itemCount: ..,
      itemBuilder: ..,
      onPageChanged: { (int position) { ...},
      .....
      preloadPagesCount: 3,
      controller: PreloadPageController(),
    );
  }
```

其实也没那么难。

### Phosphor Icons

http://phosphoricons.com/

正如开头提到的，我们当然想谈谈图标库。相信我，这个图标图书馆真是太棒了。在我看来，这些是迄今为止我见过的最好的图标。最棒的是， phosphoricons.com 上有 5364 个图标，有各种各样的变化。

当然，这只是网站的一个截图。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/746da2eaa13a85ea921db94a84d8f3fb20290858e88c5a2a4e04c9459bf97708.png)

https://pub.dev/packages/phosphor_flutter

但是... 这整件事和 Flutter 现在有什么关系？有一个官方支持的 Flutter 软件包，它包含了所有磷图标。

在 Flutter 中实现图标非常简单:

```dart
Icon(
  PhosphorIcons.pencil,// Pencil Icon
),
```

就是这样，现在你的应用程序中有了一个漂亮的铅笔图标。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/b5ae58cff570d7383dd40cc037df502642151a1a8c310bdea2f34bdaaf2642ff.png)

### Sliding Clipped Nav Bar

https://pub.dev/packages/sliding_clipped_nav_bar

到目前为止，它几乎是一个仪式，我提出了我的 10 个 Flutter 技巧文章导航 Pub 包。今天这篇文章提到了滑动滑块导航栏。这个软件包为你提供了一个让你的应用程序对用户来说更有趣的好方法。由于其令人难以置信的简单的执行和美丽的设计，它是最好的导航 Pub 包之一。

但是... ... 说够了，导航栏和这个包装看起来怎么样？

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/17ebe40afb9ca623734af582cec50ae8b3499ae89ee573e6440a56baaa1414d3.gif)

我想你很清楚我为什么这么喜欢这个包裹。有这么多的定制可能性和实现非常简单:

```dart
return Scaffold(
      body: PageView(
      physics: NeverScrollableScrollPhysics(),
      controller: controller,
...
      ),
      bottomNavigationBar: SlidingClippedNavBar(
        backgroundColor: Colors.white,
        onButtonPressed: (index) {
          setState(() {
            selectedIndex = index;
          });
          controller.animateToPage(selectedIndex,
              duration:const Duration(milliseconds: 400),
              curve: Curves.easeOutQuad);
        },
        iconSize: 30,
        activeColor: Color(0xFF01579B),
        selectedIndex: selectedIndex,
        barItems: [
          BarItem(
            icon: Icons.event,
            title: 'Events',
          ),
          BarItem(
            icon: Icons.search_rounded,
            title: 'Search',
          ),
/// Add more BarItem if you want
        ],
      ),
    );
```

是的，就是这样。你不需要做其他的事情。这个软件包的 README 文件非常详细，使您可以非常容易地开始进入软件包。

### Battery plus

https://pub.dev/packages/battery_plus

Battery_plus 是一个由 Flutter 社区支持的软件包，它可以提供关于电池状态的各种信息。它非常容易使用，并且可以用于许多应用程序:

```dart
// Import package
import 'package:battery_plus/battery_plus.dart';
// Instantiate it
var battery = Battery();
// Access current battery level
print(await battery.batteryLevel);
// Be informed when the state (full, charging, discharging) changes
battery.onBatteryStateChanged.listen((BatteryState state) {
// Do something with new state
});
```

### Freezed

http://pub.dev/packages/freezed

也许你已经听说过这个包被冻结了，但是如果没有，我现在就向你解释(因为它非常有用)。

现在我们假设您已经在 Flutter 中创建了一个数据类。正如您可能已经注意到的，这里有大量的锅炉代码，查看这些代码令人不快。这就是为什么冰冻被创造出来。使用这个包，您可以创建具有多种可能性的数据类，只需要很少的样板代码。

我不会在这里向您展示如何实现 Freezed，但是 README 文件非常棒。

end.

祝你今天愉快！

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
