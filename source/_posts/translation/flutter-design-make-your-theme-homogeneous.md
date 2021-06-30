---
title: Flutter 设计 使您的主题同质化
tags: flutter
categories: 译文
date: 2021-07-01 00:00:00
---

![](2021-07-01-05-45-34.png)

## 猫哥说

如果你和我一样长期面对电脑，分享几个对眼睛有好处的经验

- 竟可能的室内用自然光，关掉多余的光源
- 显示器分辨率比例调小，字体大些
- 如果你需要更多屏幕空间，可以加一个辅助屏幕
- 可能的话去慢慢适应暗色主题
- 颜色调的对比度低些
- 连续工作 2 小时，起来走走 让眼睛眺望下远方

> 当然并不是所有人都适合，让自己的眼睛舒服就行

这篇文章是告诉你如何通过 ThemeData 来全局管理 Flutter 的界面样式。

有一次我项目做完，已经通过了评审，然后产品和我说要调下样式，刷的一下，给我了一个新的 sketch 设计稿，我的内心是抗拒的，但是只能耐心的去分析这个新版的样式标准，幸好设计师是一个设计学科的硕士做事还算规范。

然后我通过 ThemeData 解决了 90% 的问题，因为我在代码中尽可能的用官方组件，这样在 ThemeData 中还能找到这个对象。

剩下的自定义组件，应为我有抽取公共组件，所以改改就完成了。

阅读建议，你可以通过我的译文大致的了解内容，如果感兴趣可以通过原文细细品味，下方有原文链接。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信群 ducafecat

## b 站 https://space.bilibili.com/404904528

## 原文

> https://medium.com/flutter-community/flutter-design-make-your-theme-homogeneous-13ddeffb186f

## 代码

https://github.com/GONZALEZD/flutter_demos/tree/main/app_theming

## 参考

## 正文

![](2021-07-01-05-33-41.png)

在很多 Flutter 源代码和应用程序中，我观察到一个反复出现的实践，即通过小部件参数直接添加自定义样式，导致不一致的设计和额外的维护。作为一个个人的例子，我必须维护一个 Flutter 应用程序，其中所有标题有不同的字体大小(有时字体重量)。

在这篇文章中，我将解释你的重要性的方式，你应该设计您的 Flutter 应用程序，尤其是关注的主题。

在您看来，这些“设置”页面在代码方面的区别是什么？

![](2021-07-01-05-34-14.png)

Settings page 设置页面

### 应用程序主题的正确方法是什么？

> 在上图中，“设置”页面共享完全相同的代码。在这个层次上，这四种设计之间没有严格的区别。

这里面没有什么神奇的东西: 所有的主题相关的东西都集中在更高的层次，在 MaterialApp widget 中。这个小工具允许你定义两个主题，一个用于 light brightness，另一个用于 dark theme 模式。

此外，如果没有给出任何价值，大多数小部件都会从中检索它们的设计。

小部件正在从 ThemeData (大多数情况下)设置默认值

让我们来看一个如何正确做到这一点的例子: Card widget。你可以观察到在设置页面的第三个例子中，形状是直线而不是角。

而只有‘ child’属性在代码中使用: Card (child: ...)

当你深入研究 Card 小部件是如何设计的时候，你会看到它的形状是如何定义的。下面是有关 Card.shape 属性的代码文档:

```dart
/// The shape of the card's [Material].
///
/// Defines the card's [Material.shape].
///
/// If this property is null then [CardTheme.shape] of [ThemeData.cardTheme]
/// is used. If that's null then the shape will be a [RoundedRectangleBorder]
/// with a circular corner radius of 4.0.
final ShapeBorder? shape;
```

因此，为了确保“普通”卡片共享相同的设计，您必须定义自己的 ThemeData，并在 MaterialApp 小部件中使用它作为主题(或 darkTheme)。

```dart
ThemeData example() {
  final base  = ThemeData.dark();
  final mainColor = Colors.lightBlue;
  return base.copyWith(
    cardColor: Color.lerp(mainColor, Colors.white, 0.2),
    cardTheme: base.cardTheme?.copyWith(
      color: Color.lerp(mainColor, Colors.black, 0.1),
      margin: EdgeInsets.all(20.0),
      elevation: 0.0,
      shape: BeveledRectangleBorder(
          borderRadius: BorderRadius.circular(14.0),
          side: BorderSide(color: Colors.white24, width: 1)),
    ),
  );
}
```

上面的代码演示了如何更改卡片设计，但是您可以为来自 Flutter SDK 的几乎所有小部件进行更改。此外，您应该将主题相关的内容集中到单个文件中，因为 ThemeData 是一个巨大的数据结构。

### 主题数据涵盖了所有的窗口小部件，对吧？

不幸的是，ThemeData 并不能覆盖所有窗口小部件。例如，您不能在其中定义列表贴片设计。幸运的是，您可以通过 ListTileTheme 小部件实现这一点。

说明如何更改列表平铺选定的颜色和填充，而无需在源代码中显式设置页面。

![](2021-07-01-05-36-52.png)

通过 ListTileTheme，我们可以在不更改页面代码一行的情况下重新定义选择的标题/背景和前景色

```dart
ListView.builder(
  itemBuilder: (context, index) {
    final value = elements[index];
    return ListTile(
      selected: value == selected,
      title: Text(value.title),
      subtitle: Text(value.message),
      leading: Icon(value.icon),
      onTap: () => setState(() => selected = value),
    );
  },
  itemCount: elements.length,
);
```

正如您在代码中看到的，与设计没有任何关系。它的优点是避免代码噪声，使其简洁易懂。另外，我喜欢的一点是我所有的方法都很小(少于 30 行)。

### 总结

在本文中，我们了解了如何将应用程序设计集中到 ThemeData 对象中。正如你所理解的，你可能需要阅读很多 Flutter SDK 代码文档，但是当你或者其他同事需要维护它的时候，好处就来了:

- 避免代码重复
- 减少页面中的代码，使代码更易于阅读和理解
- 确保设计的一致性

但是正如你可能已经看到的，ThemeData 对象是一个非常大的结构，在不断的演变中。所以，请密切关注它！

我在开始一个新项目时查看 ThemeData

### 为了更进一步..

再给你两个小建议！首先，如果您希望为您的组件之一进行特定的设计，而不是在构建方法中定制它，您可以将小部件包装到 Theme 小部件中。

当您设计一个新的小部件时，您可能希望通过从 ThemeData 检索主题信息来模仿 SDK 小部件。您可能已经看到，从 ThemeData 继承是不正确的方式。相反，您可以通过 InheritedTheme 小部件“扩展”它，并模仿它在 ListTileTheme 小部件中的实现方式。

像往常一样，你可以在这里找到本文使用的源代码和 adobe 设计:

https://github.com/GONZALEZD/flutter_demos/tree/main/app_theming

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
