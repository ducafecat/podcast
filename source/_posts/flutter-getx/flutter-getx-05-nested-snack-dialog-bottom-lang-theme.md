---
title: Flutter Getx 05 - 嵌套导航、多语言、主题、Snackbar、Dialog、BottomSheet
tags: flutter getx
categories: Flutter Getx
date: 2021-05-09 00:00:00
---

![](2021-05-11-05-58-12.png)

## 本节目标

- 嵌套导航
- 多语言
- 主题
- 3 UI 组件
  - Snackbar
  - Dialog
  - BottomSheet

## 视频

https://www.bilibili.com/video/BV1RU4y1t7DD/

## 代码

https://github.com/ducafecat/getx_quick_start

## 参考

- https://pub.flutter-io.cn/packages/get

## 正文

### 嵌套导航

![](2021-05-11-05-24-21.png)

几个 `Navigator` widget ，并排或者嵌套，他们是通过属性 `key` 来区分的，具体去哪里是通过 `onGenerateRoute` 实现的，在 getx 中 我们要把业务写到 `controller`中，状态切换用 `Obx` 控制 `BottomNavigationBar`，代码如下。

- lib/pages/nested_navigation/controller.dart

```dart
class NestedController extends GetxController {
  static NestedController get to => Get.find();

  var currentIndex = 0.obs;

  final pages = <String>['/list', '/detail', '/login'];

  void changePage(int index) {
    currentIndex.value = index;
    Get.toNamed(pages[index], id: 1);
  }

  Route? onGenerateRoute(RouteSettings settings) {
    if (settings.name == '/login')
      return GetPageRoute(
        settings: settings,
        page: () => LoginView(),
        transition: Transition.topLevel,
      );
    else if (settings.name == '/list')
      return GetPageRoute(
        settings: settings,
        page: () => ListIndexView(),
        transition: Transition.rightToLeftWithFade,
      );
    else if (settings.name == '/detail')
      return GetPageRoute(
        settings: settings,
        page: () => DetailView(),
        transition: Transition.fadeIn,
      );

    return null;
  }
}
```

- lib/pages/nested_navigation/binding.dart

```dart
class NestedBinding extends Bindings {
  @override
  void dependencies() {
    Get.lazyPut(() => NestedController());
  }
}
```

- lib/pages/nested_navigation/index.dart

```dart
class NestedNavView extends GetView<NestedController> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("嵌套路由"),
      ),
      body: Container(
        color: Colors.amber,
        child: Column(
          children: [
            Container(
              child: Text("占位条"),
              height: 100,
            ),
            SizedBox(
              height: 300,
              child: Navigator(
                key: Get.nestedKey(1),
                initialRoute: '/list',
                onGenerateRoute: controller.onGenerateRoute,
              ),
            ),
          ],
        ),
      ),
      bottomNavigationBar: Obx(
        () => BottomNavigationBar(
          items: const <BottomNavigationBarItem>[
            BottomNavigationBarItem(
              icon: Icon(Icons.list),
              label: '列表',
            ),
            BottomNavigationBarItem(
              icon: Icon(Icons.details),
              label: '详情',
            ),
            BottomNavigationBarItem(
              icon: Icon(Icons.login),
              label: '登录',
            ),
          ],
          currentIndex: controller.currentIndex.value,
          selectedItemColor: Colors.pink,
          onTap: controller.changePage,
        ),
      ),
    );
  }
}
```

- lib/common/routes/app_pages.dart

```dart
    GetPage(
      name: AppRoutes.NestedNavigator,
      page: () => NestedNavView(),
      binding: NestedBinding(),
    ),
```

### 多语言

![](2021-05-11-05-30-52.png)

- 编写多语言字典

文件名格式 `[国家]_[语言].dart`

lib/common/lang/en_US.dart

```dart
const Map<String, String> en_US = {
  'title': 'This is Title!',
  'login': 'logged in as @name with email @email',
};
```

lib/common/lang/zh_Hans.dart

```dart
const Map<String, String> zh_Hans = {
  'title': '这是标题',
  'login': '登录用户 @name，邮箱账号 @email',
};
```

lib/common/lang/zh_HK.dart

```dart
const Map<String, String> zh_HK = {
  'title': '這是標題',
  'login': '登錄用戶 @name，郵箱賬號 @email',
};
```

- 继承 Translations

lib/common/lang/translation_service.dart

```dart
class TranslationService extends Translations {
  static Locale? get locale => Get.deviceLocale;
  static final fallbackLocale = Locale('en', 'US');
  @override
  Map<String, Map<String, String>> get keys => {
        'en_US': en_US,
        'zh_Hans': zh_Hans,
        'zh_HK': zh_HK,
      };
}
```

- 初始 GetMaterialApp

lib/main.dart

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      ...

      locale: TranslationService.locale,
      fallbackLocale: TranslationService.fallbackLocale,
      translations: TranslationService(),
    );
  }
}
```

`locale` 当前系统语言

`fallbackLocale` 如果找不到对应字典，默认值

`translations` 字典列表

- 切换 updateLocale

采用扩展操作符方式调用显示，点赞 `xxx.tr,`

切换语言 `Get.updateLocale`

```dart
"title -> " + 'title'.tr,

......

var locale = Locale('zh', 'HK');
Get.updateLocale(locale);
```

lib/pages/lang/index.dart

```dart

class LangView extends StatelessWidget {
  const LangView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("多语言"),
      ),
      body: Center(
        child: Column(
          children: [
            Text(
              "title -> " + 'title'.tr,
              style: TextStyle(fontSize: 24),
            ),
            Divider(),
            Text(
              "login -> " +
                  'login'.trParams(
                      {'name': 'ducafecat', 'email': 'ducafecat@gmail.com'})!,
              style: TextStyle(fontSize: 24),
            ),
            Divider(),
            ListTile(
              title: Text("切换语言"),
              subtitle: Text('zh-HK'),
              onTap: () {
                var locale = Locale('zh', 'HK');
                Get.updateLocale(locale);
              },
            ),
            ListTile(
              title: Text("切换语言"),
              subtitle: Text('zh-Hans'),
              onTap: () {
                var locale = Locale('zh', 'Hans');
                Get.updateLocale(locale);
              },
            ),
            ListTile(
              title: Text("切换语言"),
              subtitle: Text('en-US'),
              onTap: () {
                var locale = Locale('en', 'US');
                Get.updateLocale(locale);
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

### 主题

![](2021-05-11-05-45-18.png)

直接 `Get.changeTheme` 切换 `ThemeData` 数据。

```dart
onTap: () {
  Get.changeTheme(
      Get.isDarkMode ? ThemeData.light() : ThemeData.dark());
},
```

- lib/pages/theme/index.dart

```dart
class ThemeView extends StatelessWidget {
  const ThemeView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("主题"),
      ),
      body: Center(
        child: Column(
          children: [
            Container(
              height: 100,
              child: Align(
                  alignment: Alignment.center,
                  child: Text(
                    "是否黑色主题 -> " + Get.isDarkMode.toString(),
                    style: TextStyle(fontSize: 24),
                  )),
            ),
            Divider(),
            ListTile(
              title: Text("切换主题"),
              subtitle: Text('Get.changeTheme'),
              onTap: () {
                Get.changeTheme(
                    Get.isDarkMode ? ThemeData.light() : ThemeData.dark());
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

### Snackbar

![](2021-05-11-05-46-22.png)

- 调用

```dart
  onTap: () => Get.snackbar(
    "标题",
    "消息",
  ),
```

- 参数

```dart
  void snackbar<T>(
    String title,
    String message, {
    Color? colorText,
    Duration? duration,

    /// with instantInit = false you can put snackbar on initState
    bool instantInit = true,
    SnackPosition? snackPosition,
    Widget? titleText,
    Widget? messageText,
    Widget? icon,
    bool? shouldIconPulse,
    double? maxWidth,
    EdgeInsets? margin,
    EdgeInsets? padding,
    double? borderRadius,
    Color? borderColor,
    double? borderWidth,
    Color? backgroundColor,
    Color? leftBarIndicatorColor,
    List<BoxShadow>? boxShadows,
    Gradient? backgroundGradient,
    TextButton? mainButton,
    OnTap? onTap,
    bool? isDismissible,
    bool? showProgressIndicator,
    SnackDismissDirection? dismissDirection,
    AnimationController? progressIndicatorController,
    Color? progressIndicatorBackgroundColor,
    Animation<Color>? progressIndicatorValueColor,
    SnackStyle? snackStyle,
    Curve? forwardAnimationCurve,
    Curve? reverseAnimationCurve,
    Duration? animationDuration,
    double? barBlur,
    double? overlayBlur,
    SnackbarStatusCallback? snackbarStatus,
    Color? overlayColor,
    Form? userInputForm,
  }) async {
```

### Dialog

![](2021-05-11-05-48-41.png)

- 调用

```dart
  onTap: () => Get.defaultDialog(
    title: "标题",
    content: Column(
      children: [
        Text("第1行"),
        Text("第2行"),
        Text("第3行"),
      ],
    ),
    textConfirm: "确认",
    textCancel: "取消",
    onConfirm: () => Get.back(),
  ),
```

- 参数

```dart
  Future<T?> defaultDialog<T>({
    String title = "Alert",
    TextStyle? titleStyle,
    Widget? content,
    VoidCallback? onConfirm,
    VoidCallback? onCancel,
    VoidCallback? onCustom,
    Color? cancelTextColor,
    Color? confirmTextColor,
    String? textConfirm,
    String? textCancel,
    String? textCustom,
    Widget? confirm,
    Widget? cancel,
    Widget? custom,
    Color? backgroundColor,
    bool barrierDismissible = true,
    Color? buttonColor,
    String middleText = "Dialog made in 3 lines of code",
    TextStyle? middleTextStyle,
    double radius = 20.0,
    //   ThemeData themeData,
    List<Widget>? actions,

    // onWillPop Scope
    WillPopCallback? onWillPop,
  }) {
```

### BottomSheet

![](2021-05-11-05-49-55.png)

- 调用

```dart
  onTap: () => Get.bottomSheet(
    Container(
      height: 200,
      color: Colors.white,
      child: Column(
        children: [
          Text("第1行"),
          Text("第2行"),
          Text("第3行"),
        ],
      ),
    ),
  ),
),
```

- 参数

```dart
extension ExtensionBottomSheet on GetInterface {
  Future<T?> bottomSheet<T>(
    Widget bottomsheet, {
    Color? backgroundColor,
    double? elevation,
    bool persistent = true,
    ShapeBorder? shape,
    Clip? clipBehavior,
    Color? barrierColor,
    bool? ignoreSafeArea,
    bool isScrollControlled = false,
    bool useRootNavigator = false,
    bool isDismissible = true,
    bool enableDrag = true,
    RouteSettings? settings,
    Duration? enterBottomSheetDuration,
    Duration? exitBottomSheetDuration,
  }) {
```

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
