---
title: Flutter Convex Bottom 底部导航
tags: flutter
categories: 译文
date: 2022-01-13 21:00:00
---

![](2022-01-13-21-34-45.png)

## 原文

> https://medium.flutterdevs.com/convex-bottom-bar-in-flutter-7158fc642111

## 代码

https://github.com/flutter-devs/convex_bottom_bar_demo

## 参考

- https://pub.dev/packages/convex_bottom_bar

## 正文

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/9f3fbf1c9a26d4c2249bbd15d35a7f5f04e453bc04f0d295d3d40a8f8f199452.png)

**convex Bottom** 底部条是一个应用程序 sketch 图，它的形状是 convex Bottom 的。它可以使用户界面看起来很棒，也可以改进用户与界面的交互方式。在本文中，我们将建立一个简单的应用程序与最简单的形式之一，在 Flutter convex Bottom bar

# 目录:

> 引言
>
> 添加依赖性
>
> 如何使用
>
> 功能
>
> 属性
>
> 实施
>
> 总结
>
> GitHub 链接

# 简介:

大家好，今天我们要讨论的是一个非常重要的关于 Flutter UI 的话题，这个话题就是底层导航栏。在本文中，我们了解了**Convex Bottom Bar**。**Convex Bottom Bar**是一种 Flutter 包装。Convex Bottom 的底部条是一个应用程序栏草图这样的方式，有一个 convex Bottom 状它。它可以使用户界面看起来很棒，也可以改进用户与界面的交互方式。在本文中，我们将构建一个简单的应用程序与最简单的形式之一的**Convex Bottom Bar**。

查看列表 TabItems 小部件，您可以解释在 appbar 中显示的图标和它们的标题。列表应该只包含奇数的 TabItems 来运行小部件，而不会引起谬误。如果要显示图像或图标，可以为每个 TabItem 小部件中的图标变量提供图像。如果你想生成一个图标底部 appbar，你可以使用 ConvexButton.fab 小部件。它生成的参数更少，并且有一个快速漂亮的单图标 appbar。

# 添加依赖项:

在你的项目中去 pubspec。添加依赖项: 添加 https://pub.dev/packages/convex_bottom_bar 的最新版本。

```dart
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  convex_bottom_bar: ^3.0.0
```

我们使用 `convax_bottom_bar` 来创建一个更好的 bootobar UI。

# 如何使用:

通常， **ConvexAppBar** 可以通过设置它的 bottomNavigationBar 来与脚手架一起工作。convexAppBar 有两个构造函数，convexAppBar ()将使用 levant 样式简化制表符的创建。将其添加到包的 pubspec.yaml 文件中，使用最新版本。

```dart
Scaffold(
  body: bottomNavigationBar: ConvexAppBar();
);
```

# 特点:

- 提供各种内部款式
- 可以更改 AppBar 的主题。
- 提供用于修改新样式的构建器 API。
- 在选项卡菜单中加入徽章。
- 优美的过渡动画
- 提供钩子 API 来覆盖一些内部样式。
- RTL 补强

# 属性:

> 下面是 **_Convex_Bottom_Bar_** 的一些性质:

- **fixed** (副标题图标停留在中心)
- **fixedCircle** (相同，但在固定图标的所有边上都有一个白色的圆圈)
- **react** (上标图标取代点击另一个图标)
- **reactCircle** (与上标图标中的白色圆圈相同)
- **textIn** (选定的离子出现相应的标题)
- **titled** (未选择的图标是显示其标题的单个图标)
- **flip** (点击图标显示一个 flip 动画)
- **custom** (使用 ConvexBottomAppBar 构建器自定义预定义的参数)
- **height** (grabbing the appbar)
- **top** (grabbing the superscripted icon)
- **curveSize** (拉伸上标图标的曲线)
- **color** (设置图标的颜色)
- **backgroundColor** (设置 appbar 背景颜色)
- **gradient** (使用渐变小部件设置 appbar 背景颜色)
- **activeColor** (设置圆形颜色)

# 实施方案:

在 `Convex_Bottom_Bar` 演示中，首先，我们在这个类中创建一个名为 MyHomePage ()的有状态类，我们创建一个值为 0 的变量 selectedpage 类型的 integer pass。定义一个名为 pageNo 的列表，在这个列表中我们传递要添加到 bootom 导航栏中的所有页面。

```dart
int selectedpage =0;
final _pageNo = [Home() , Favorite() , CartPage() , ProfilePage()];
```

在 BuildContext ()中，我们定义 Scaffold。

```dart
appBar: AppBar(
  centerTitle: true,
  title: Text('Convex Bottom Bar'),
),
```

首先在正文中传递 \_pageno，其值为 selectedPage。使用 scaffold 属性，我们使用 bottomNavigationBar。在这里，我们创建 ConvexAppBar ()并传递 Items、 initialActiveIndex 和 onTap。在条目中，我们通过所有的屏幕，我们希望在我们的应用程序中显示。在 initialActiveIndexwe 中，我们传递已经定义的变量 selectedpage，在 onTap 中，我们传递 index 并在 setState 中定义 setState () ，我们传递 selectedpage 相当于 index。

```dart
bottomNavigationBar: ConvexAppBar(
  items: [
    TabItem(icon: Icons._home_, title: 'Home'),
    TabItem(icon: Icons._favorite_, title: 'Favorite'),
    TabItem(icon: Icons._shopping_cart_, title: 'Cart'),
    TabItem(icon: Icons._person_, title: 'Profile'),
  ],
  initialActiveIndex: selectedpage,
  onTap: (int index){
      setState(() {
        selectedpage = index;
      });
  },
),
```

如果我们创建不同的页面, Home(), Favorite(), CartPage(), ProfilePage(). 在 Home 类中，我们定义一个带有背景颜色的文本。

> Home 主屏幕:

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class Home extends StatefulWidget {
  const Home({Key? key}) : super(key: key);

  @override
  _HomeState createState() => _HomeState();
}

class _HomeState extends State<Home> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('Home Page',
          style: TextStyle(
              fontSize: 20,
              fontWeight: FontWeight.bold
          ),),
      ),
    );
  }
}
```

> Favorite 最喜欢的屏幕:

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class Favorite extends StatefulWidget {
  const Favorite({Key? key}) : super(key: key);

  @override
  _FavoriteState createState() => _FavoriteState();
}

class _FavoriteState extends State<Favorite> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors._greenAccent_,
     body: Center(
          child: Text('Favorite Page',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight._bold_ ),
          )),
    );
  }
}
```

> CartPage 屏幕:

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class CartPage extends StatefulWidget {
  const CartPage({Key? key}) : super(key: key);

  @override
  _CartPageState createState() => _CartPageState();
}

class _CartPageState extends State<CartPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors._pink_.shade100,
      body: Center(
          child: Text('Cart Page',
            style: TextStyle(
                fontSize: 20,
                fontWeight: FontWeight._bold_ ),)),
    );
  }
}
```

> \_ProfilePage 个人资料页面:

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class ProfilePage extends StatefulWidget {
  const ProfilePage({Key? key}) : super(key: key);

  @override
  _ProfilePageState createState() => _ProfilePageState();
}

class _ProfilePageState extends State<ProfilePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors._yellow_,
      body: Center(
          child: Text('Profile Page',
            style: TextStyle(
                fontSize: 20,
                fontWeight: FontWeight._bold_ ),)),
    );  }
}
```

当我们运行应用程序，我们应该得到屏幕的输出像下面的屏幕视频。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/88693dbd56d2a370fbe1835330dd7d6233025431ddbaff7b17aca3470813c456.gif)

# 结语:

本文介绍了什么是**Convex Bottom Bar**以及如何在 Flutter 法中实现**Convex Bottom Bar**。你可以根据自己的需要使用这个包。

如果我做错了什么，请在评论中告诉我，我很乐意改进。

鼓掌如果这篇文章对你有帮助的话。

# GitHub Link:

https://github.com/flutter-devs/convex_bottom_bar_demo

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
