---
title: 17 个提高性能的 Flutter 最佳实践
tags: flutter
categories: 译文
date: 2022-04-27 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220427064913.png)

与其他混合平台相比， Flutter 性能够快吗？答案是肯定的，但是出于这种考虑，让我们来看看一些令人惊叹的性能和优化实践。

## 原文

> https://inficial.medium.com/flutter-best-practices-for-improve-performance-7e21e14efebb

## 参考

- https://api.flutter.dev/flutter/widgets/ListView/itemExtent.html
- https://blog.codemagic.io/how-to-improve-the-performance-of-your-flutter-app./#dont-split-your-widgets-into-methods
- https://pub.dev/packages/nil
- https://medium.com/flutter-community/flutter-memory-optimization-series-8c4a73f3ea81
- https://dart.dev/guides/language/effective-dart/style
- https://dart-lang.github.io/linter/lints/index.html
- https://itnext.io/comparing-darts-loops-which-is-the-fastest-731a03ad42a2
- https://iisprey.medium.com/get-rid-of-all-kind-of-boilerplate-code-with-flutter-hooks-2e17eea06ca0

## 正文

### 1. 使用 Widgets 代替函数

不要这样用它

```dart
Widget _buildFooterWidget() {
  return Padding(
            padding: const EdgeInsets.all(8.0),
            child: Text('This is the footer '),
         );
}.
```

像这样使用它

```dart
class FooterWidget extends StatelessWidget {
  @override

   Widget build(BuildContext context) {
  return Padding(
            padding: const EdgeInsets.all(8.0),
            child: Text('This is the footer '),
         );
      }
}
```

如果一个函数可以做同样的事情，Flutter 就不会有 StatelessWidget。

类似地，它主要是针对公共 widgets 的，这些 widgets 可以重复使用。私人函数只能使用一次并不重要ーー尽管意识到这种行为仍然是好的。

使用函数而不是类之间有一个重要的区别，那就是: 框架不知道函数，但是可以看到类。

考虑下面的“ widget”函数:

```dart
Widget functionWidget({ Widget child}) {
    return Container(child: child);
}
```

这样使用:

```dart
functionWidget(
    child : functionWidget()
)
```

而且它是等价的:

```dart
class ClassWidget extends StatelessWidget {
final Widget child;
const ClassWidget({Key key, this.child}) : super(key: key); @override
 Widget build(BuildContext context) {
      return Container(
               child: child,
              );
  }
}
```

这样使用:

```dart
new ClassWidget(
  child : new ClassWidget(),
)
```

在纸面上，两者似乎做了同样的事情: 创建 2 个容器，一个嵌套在另一个中。但现实情况略有不同。

对于函数，生成的 widgets 树如下所示:

```dart
Container
  Container
```

对于类，widgets 树是:

```dart
ClassWidget
  Container
    ClassWidget
      Container
```

这一点很重要，因为它改变了框架在更新 widgets 时的行为。

**为什么这很重要**

通过使用函数将 widgets 树拆分为多个 widgets，您将自己暴露在 bug 之中，并错过了一些性能优化。

不能保证使用函数会出现 bug，但是使用类可以保证不会遇到这些问题。
the issues:

下面是一些在 Dartpad 上的交互式例子，你可以自己跑步来更好地理解这些问题:

- <https://dartpad.dev/1870e726d7e04699bc8f9d78ba71da35> 这个例子展示了如何通过分割你的应用程序的功能，你可能会不小心破坏诸如 AnimatedSwitcher 之类的东西
- <https://dartpad.dev/a869b21a2ebd2466b876a5997c9cf3f1>
  这个例子展示了类如何允许更细粒度地重新构建 widgets 树，从而提高性能
- <https://dartpad.dev/06842ae9e4b82fad917acb88da108eee>
  这个例子展示了在使用 InheritedWidgets (比如主题或提供程序)时，如何通过使用函数使自己暴露在错误使用 BuildContext 和错误面前

总的来说，由于这些原因，在类上使用函数来重用 widgets 被认为是一种不好的做法。你可以，但它可能会在未来咬你。

- 避免重复重建所有 widgets
- 而且，添加 const 是个好主意。
- ListView 长列表的用户项目范围。

指定 [itemExtent](https://api.flutter.dev/flutter/widgets/ListView/itemExtent.html) 比让子滚动机制确定其范围更有效，因为滚动机制可以利用子滚动机制的预先知识来节省工作，例如当滚动位置发生剧烈变化时。

- 避免在 animatedBuilder 中重建不必要的 widgets

不要这样用它

```dart
body: AnimatedBuilder(
    animation: _controller,
    builder: (_, child) => Transform(
        alignment: Alignment.center,
        transform: Matrix4.identity()
          ..setEntry(3, 2, 0.001)
          ..rotateY(360 * _controller.value * (pi / 180.0)),
        child: CounterWidget(
                 counter: counter,
               ),),
),
```

这将在出现动画时重新构建 CounterWidget widgets。如果您将 log 放入 counterWidget 的 build 方法，那么您可以看到每次出现动画时它都会打印一个日志。

像这样使用它。

```dart
body: AnimatedBuilder(
   animation: _controller,
   child : CounterWidget(
       counter: counter,
    ),
    builder: (_, child) => Transform(
        alignment: Alignment.center,
        transform: Matrix4.identity()
          ..setEntry(3, 2, 0.001)
          ..rotateY(360 * _controller.value * (pi / 180.0)),
        child: child),
),
```

我们使用 AnimatedBuilder 提供的子属性，它允许我们缓存 widgets 以在动画中重用它们。我们这样做是因为这个 widgets 不会改变，它唯一要做的就是旋转，但是为此，我们有了 Transform widgets。

**查看更多信息:**

https://blog.codemagic.io/how-to-improve-the-performance-of-your-flutter-app./#dont-split-your-widgets-into-methods

### 2. 尽可能使用 const

```dart
class CustomWidget extends StatelessWidget {
   const CustomWidget(); @override
  Widget build(BuildContext context) {
    ...
  }
}
```

- 在构建 widgets 时，或者使用 flutter widgets 时。这有助于 flutter 只重建应该更新的 widgets。当 setState 调用时，widgets 不会更改。它将阻止 widgets 重新构建。
- 您可以节省 CPU 周期，并将它们与 const 构造函数一起使用。

### 3. 用 nil 代替 const Container ()

```dart
// good
text != null ? Text(text) : const Container()
// Better
text != null ? Text(text) : const SizedBox()
// BEST
text != null ? Text(text) : nil
or
if (text != null) Text(text)
```

当您不想显示任何内容时，在 widgets 树中添加一个简单的 widgets，对性能的影响最小。

https://pub.dev/packages/nil

### 4. 用 keys 提高 Flutter 性能

```dart
// FROM
return value
   ? const SizedBox()
   : const Placeholder(),
// TO
return value
   ? const SizedBox(key: ValueKey('SizedBox'))
   : const Placeholder(key: ValueKey('Placeholder')),

----------------------------------------------

// FROM
final inner = SizedBox();
return value ? SizedBox(child: inner) : inner,
// TO
final global = GlobalKey();
final inner = SizedBox(key: global);
return value ? SizedBox(child: inner) : inner,
```

通过使用键的颤动更好地识别 widgets，这提供了更好的性能。

### 5. 使用图像 ListView 时优化内存

```dart
ListView.builder(
  ...
   addAutomaticKeepAlives: false (true by default)
   addRepaintBoundaries: false (true by default)
);
```

ListView 无法杀死它的子节点，因为它们在屏幕上不可见。如果儿童有高分辨率的图像，会消耗大量的内存。

做这些选项错误，可能导致使用更多的 GPU 和 CPU 工作，但它可以解决我们的内存问题，您将得到一个非常高性能的看法没有明显的问题。

Flutter 记忆优化系列

几乎在 Flutter 的任何东西都是默认优化和增强的，正如你可能已经知道的那样，感谢 Flutter 团队在..

https://medium.com/flutter-community/flutter-memory-optimization-series-8c4a73f3ea81

### 6. 遵循 Dart 风格

**Identifiers:**: 标识符有三种风格

- UpperCamelCase 名称将每个单词的首字母大写，包括第一个单词。
- lowerCamelCase 每个单词的第一个字母都大写，除了第一个字母总是小写，即使它是首字母缩略词。
- lowercase\*with_underscores 名称只使用小写字母，即使对于首字母缩写也是如此，而使用 \_ 的单词也是如此。

**使用 UpperCamelCase 命名类型**

类、枚举类型、 typedef 和类型参数应该大写每个单词(包括第一个单词)的第一个字母，并且不使用分隔符。

```dart
good
class SliderMenu { ... }
class HttpRequest { ... }
typedef Predicate<T> = bool Function(T value);
const foo = Foo();
@foo
class C { ... }
```

**使用以下命名库、包、目录和源文件**

```dart
Good
library peg_parser.source_scanner;
import 'file_system.dart';
import 'slider_menu.dart';

Bad
library pegparser.SourceScanner;
import 'file-system.dart';
import 'SliderMenu.dart';
```

要了解更多样式，请查看 Dart 样式:

好的代码的一个惊人的重要部分是好的风格。一致的命名、排序和格式有助于代码..

https://dart.dev/guides/language/effective-dart/style

要了解更多规则，请查看 Dart 连接器:

https://dart-lang.github.io/linter/lints/index.html

### 7. 只在需要的时候使用 MediaQuery/LayoutBuilder

### 8. 使用 async/await 代替 then 函数

### 9. 有效使用操作符

```dart
var car = van == null ? bus : audi;         // Old pattern
var car = audi ?? bus;                      // New pattern
var car = van == null ? null : audi.bus;    // Old pattern
var car = audi?.bus;                        // New pattern
(item as Car).name = 'Mustang';         // Old pattern
if (item is Car) item.name = 'Mustang'; // New pattern
```

### 10. 利用字符串模板内插

```dart
// Inappropriate

var discountText = 'Hello, ' + name + '! You have won a brand new ' + brand.name + 'voucher! Please enter your email to redeem. The offer expires within ' + timeRemaining.toString() ' minutes.';

// Appropriate

var discountText = 'Hello, $name! You have won a brand new ${brand.name} voucher! Please enter your email to redeem. The offer expires within ${timeRemaining} minutes.';

```

### 11. 使用 for/while 代替 foreach/map

您可以在本文中检查循环的比较

比较 Dart 的线圈ーー哪一个最快？

编写 Flutter apps 的语言 Dart 有许多不同的循环，它们可以循环遍历列表或运行一些..

https://itnext.io/comparing-darts-loops-which-is-the-fastest-731a03ad42a2

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/69db28f2e775f05277ea524901981bd5f8e2b26594817929b22698034d37ae92.png)

### 12. 精确显示你的图片和图标

```dart
precacheImage(AssetImage(imagePath), context);For SVGs
you need flutter_svg package.precachePicture(
  ExactAssetPicture(
    SvgPicture.svgStringDecoderBuilder,iconPath),context,
);
```

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/e20970c8abb805bce1fb57306dee087872187a5e378bbb760a382b9307bbb232.gif)

### 13. 使用 SKSL Warmup

```sh
flutter run --profile --cache-sksl --purge-persistent-cache
flutter build apk --cache-sksl --purge-persistent-cache
```

如果一个应用程序在第一次运行时有简洁的动画，之后对于同样的动画变得流畅，那么这很可能是由于着色器编译的延迟。

### 14. 考虑使用 RepaintBoundary widgets

Flutter widgets 与渲染对象相关联。渲染对象有一个名为 paint 的方法，用于执行绘制。但是，即使关联的 widgets 实例不变，也可以调用 paint 方法。这是因为如果其中一个被标记为脏的话，Flutter 可能会对同一层中的其他渲染对象执行重新绘制。当渲染对象需要通过 RenderObject.markneedspaint 重新绘制时，它会告诉它最近的祖先重新绘制。祖先对其祖先执行相同的操作，可能直到根 RenderObject 为止。当一个渲染对象的 paint 方法被触发时，它在同一层中的所有子渲染对象都将被重新绘制。

在某些情况下，当需要重新绘制渲染对象时，同一层中的其他渲染对象不需要重新绘制，因为它们呈现的内容保持不变。换句话说，如果我们只能重新绘制某些渲染对象会更好。使用 RepaintBoundary 对于限制 markneedspain 在渲染树上的传播和 paintChild 在渲染树上的传播非常有用。RepaintBoundary 可以将祖先呈现对象与后代呈现对象解耦。因此，只能重新绘制内容发生变化的子树。使用 RepaintBoundary 可以显著提高应用程序的性能，特别是如果不需要重新绘制的子树需要大量的重新绘制工作时。

### 15. 如果可能的话，使用命名构造器

```dart
// 例如

Listview → Listview.builder
```

### 16. 适当处理数据

不必要的内存使用会在应用程序内悄悄地杀死数据，所以不要忘记处理你的数据

有些软件包为它们的类提供自动释放支持

- [Riverpod](https://pub.dev/packages/riverpod)
- [GetX](https://pub.dev/packages/get)
- [get_it](https://pub.dev/packages/get_it)
- [flutter_hooks](https://pub.dev/packages/flutter_hooks)

如果你不知道什么是 Flutter 挂钩和如何使用它

用 Flutter 钩子去除各种样板代码

您不认为，现在是时候关闭 StatefulWidget 并使用 flutter hook 删除样板代码了吗

https://iisprey.medium.com/get-rid-of-all-kind-of-boilerplate-code-with-flutter-hooks-2e17eea06ca0

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/f0c3b75c87be9a4d853165f1704c8902c2f05d6da492a117575245e0e842a7dd.jpeg)

将 cacheHeight 和 cacheWidth 设置为图像

您可以通过这种方式减少内存使用

### 17. 不要在 List Map 中使用引用

不要这样使用

```dart
List a = [1,2,3,4];
List b;
b = a;
a.remove(1);
print(a);  // [2,3,4]
print(b);  // [2,3,4]
```

由于这个原因，每当您尝试调用列表 a 的任何方法时，都会自动调用列表 b。

比如 a.remove (some) ; 也会从列表 b 中删除该项;

像这样使用它

```dart
List a = [1,2,3,4];
List b;
b = jsonDecode(jsonEncode(a));
a.remove(1);
print(a);  // [2,3,4]
print(b);  // [1,2,3,4]
```

### end

我希望这给你一些见解，以改善您的 Flutter 应用程序的性能。快乐的编码！

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
