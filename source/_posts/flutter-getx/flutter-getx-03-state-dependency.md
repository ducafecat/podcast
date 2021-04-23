---
title: Flutter Getx 03 - 状态管理、依赖加载
date: 2021-4-16 00:00:00
tags: flutter getx
categories: Flutter Getx
---

![](2021-04-16-18-03-08.png)

## 本节目标

- 4 种状态管理
  - Obx
  - GetX
  - GetBuilder
  - ValueBuilder
- 防抖、限流 函数
  - ever
  - once
  - debounce
  - interval
- 依赖注入
  - Get.put
  - Get.lazyPut
- 视图组件
  - GetView

## 视频

https://www.bilibili.com/video/BV1PA411V7qc/

## 代码

https://github.com/ducafecat/getx_quick_start

## 参考

- https://pub.flutter-io.cn/packages/get#reactive-state-manager
- https://dart.dev/guides/language/extension-methods

## 正文

### 状态管理

#### Obx

- lib/pages/state_obx/index.dart

```dart
class StateObxView extends StatelessWidget {
  StateObxView({Key? key}) : super(key: key);

  final count = 0.obs;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Obx(...)"),
      ),
      body: Center(
        child: Column(
          children: [
            Obx(() => Text("count1 -> " + count.toString())),
            Obx(() => Text("count2 -> " + count.toString())),

            //
            Divider(),
            ElevatedButton(
              onPressed: () {
                count.value++;
              },
              child: Text('add'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- obs、extension、RxInt、Rx<T>

```dart
...

extension StringExtension on String {
  /// Returns a `RxString` with [this] `String` as initial value.
  RxString get obs => RxString(this);
}

extension IntExtension on int {
  /// Returns a `RxInt` with [this] `int` as initial value.
  RxInt get obs => RxInt(this);
}

extension DoubleExtension on double {
  /// Returns a `RxDouble` with [this] `double` as initial value.
  RxDouble get obs => RxDouble(this);
}

extension BoolExtension on bool {
  /// Returns a `RxBool` with [this] `bool` as initial value.
  RxBool get obs => RxBool(this);
}

extension RxT<T> on T {
  /// Returns a `Rx` instace with [this] `T` as initial value.
  Rx<T> get obs => Rx<T>(this);
}

```

- 小结

适合界面上 简单状态管理，写起来很快。

#### GetX

- 编写控制器 lib/pages/state_getx/controller.dart

```dart
class CountController extends GetxController {
  final _count = 0.obs;
  set count(value) => this._count.value = value;
  get count => this._count.value;

  final _count2 = 0.obs;
  set count2(value) => this._count2.value = value;
  get count2 => this._count2.value;

  add() => _count.value++;
  add2() => _count2.value++;
}
```

- 编写视图 lib/pages/state_getx/index.dart

```dart

class StateGetxView extends StatelessWidget {
  StateGetxView({Key? key}) : super(key: key);

  final controller = CountController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Getx"),
      ),
      body: Center(
        child: Column(
          children: [
            GetX<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetX - 1");
                return Text('value 1 -> ${_.count}');
              },
            ),
            GetX<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetX - 2");
                return Text('value 2 -> ${_.count}');
              },
            ),
            Divider(),

            //
            GetX<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetX - 3");
                return Column(
                  children: [
                    Text('value 3 -> ${_.count}'),
                    ElevatedButton(
                      onPressed: () {
                        _.add();
                      },
                      child: Text('count1'),
                    )
                  ],
                );
              },
            ),
            Divider(),

            // count2
            GetX<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetX - 4");
                return Text('value 4 -> ${_.count2}');
              },
            ),
            Divider(),

            // 按钮
            ElevatedButton(
              onPressed: () {
                controller.add();
              },
              child: Text('count1'),
            ),

            ElevatedButton(
              onPressed: () {
                controller.add2();
              },
              child: Text('count2'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- 小结

适合控制多控制器、多状态更新，可精细控制初始、局部渲染。

#### GetBuilder

- 控制器 lib/pages/state_getBuilder/controller.dart 同上，不再重复
- 视图 lib/pages/state_getBuilder/index.dart

```dart
class StateGetBuilderView extends StatelessWidget {
  StateGetBuilderView({Key? key}) : super(key: key);

  final controller = CountController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("GetBuilder"),
      ),
      body: Center(
        child: Column(
          children: [
            GetBuilder<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetBuilder - 1");
                return Text('value -> ${_.count}');
              },
            ),
            GetBuilder<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetBuilder - 2");
                return Text('value -> ${_.count}');
              },
            ),
            Divider(),

            //
            GetBuilder<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetBuilder - 3");
                return Column(
                  children: [
                    Text('value -> ${_.count}'),
                    ElevatedButton(
                      onPressed: () {
                        _.add();
                      },
                      child: Text('GetBuilder -> add'),
                    )
                  ],
                );
              },
            ),
            Divider(),

            // count2
            GetBuilder<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetBuilder - 4");
                return Text('value count2 -> ${_.count2}');
              },
            ),
            Divider(),

            // id2
            GetBuilder<CountController>(
              id: "id2",
              init: controller,
              initState: (_) {},
              builder: (_) {
                print("GetBuilder - 4");
                return Text('id2 -> value count2 -> ${_.count2}');
              },
            ),
            Divider(),

            // 按钮
            ElevatedButton(
              onPressed: () {
                controller.add();
              },
              child: Text('add'),
            ),

            ElevatedButton(
              onPressed: () {
                controller.add2();
              },
              child: Text('add2'),
            ),

            ElevatedButton(
              onPressed: () {
                controller.update();
              },
              child: Text('controller.update()'),
            ),

            ElevatedButton(
              onPressed: () {
                controller.update(["id2"]);
              },
              child: Text('controller.update(id2)'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- 小结

  和 `GetX` 比起来，多了手动控制更新，有两点需要注意。

  1. `controller.update();` 触发更新
  2. `id: "id2",` 标记哪个 `builder` ，触发方式 `controller.update(["id2"]);` ，可传多个 `Array` 类型。

#### ValueBuilder

- lib/pages/state_valueBuilder/index.dart

```dart
class StateValueBuilderView extends StatelessWidget {
  StateValueBuilderView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("ValueBuilder"),
      ),
      body: Column(
        children: [
          Center(
            child: ValueBuilder<int?>(
              initialValue: 10,
              builder: (value, updateFn) {
                return Column(
                  children: [
                    Text("count -> " + value.toString()),
                    ElevatedButton(
                      onPressed: () {
                        updateFn(value! + 1);
                      },
                      child: Text('ValueBuilder -> add'),
                    )
                  ],
                );
              },
              // builder: (value, updateFn) => Switch(
              //   value: value,
              //   onChanged:
              //       updateFn, // same signature! you could use ( newValue ) => updateFn( newValue )
              // ),
              // if you need to call something outside the builder method.
              onUpdate: (value) => print("Value updated: $value"),
              onDispose: () => print("Widget unmounted"),
            ),
          ),
        ],
      ),
    );
  }
}
```

- 小结

适合局部的状态管理，很灵活。

#### 防抖、限流

- 控制器 lib/pages/state_workers/controller.dart

```dart
class CountController extends GetxController {
  final _count = 0.obs;
  set count(value) => this._count.value = value;
  get count => this._count.value;

  add() => _count.value++;

  @override
  void onInit() {
    super.onInit();

    // 每次
    ever(_count, (value) {
      print("ever -> " + value.toString());
    });

    // 第一次
    once(_count, (value) {
      print("once -> " + value.toString());
    });

    // 防抖 2 秒内
    debounce(
      _count,
      (value) {
        print("debounce -> " + value.toString());
      },
      time: Duration(seconds: 2),
    );

    // 定时器 1 秒
    interval(
      _count,
      (value) {
        print("interval -> " + value.toString());
      },
      time: Duration(seconds: 1),
    );
  }
}
```

- 视图 lib/pages/state_workers/index.dart

```dart
class StateWorkersView extends StatelessWidget {
  StateWorkersView({Key? key}) : super(key: key);

  final controller = CountController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("GetBuilder"),
      ),
      body: Center(
        child: Column(
          children: [
            // 显示
            GetX<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                return Text('value -> ${_.count}');
              },
            ),

            // 按钮
            ElevatedButton(
              onPressed: () {
                controller.add();
              },
              child: Text('add'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- 小结

`ever` 适合做监听、日志收集

`debounce` 适合做搜索输入框

### 依赖注入

#### Get.put

- 控制器 lib/pages/dependency_put_find/controller.dart

```dart
class CountController extends GetxController {
  final _count = 0.obs;
  set count(value) => this._count.value = value;
  get count => this._count.value;

  add() => _count.value++;

  @override
  void onInit() {
    super.onInit();
    print("onInit");
  }

  @override
  void onClose() {
    super.onClose();
    print("onClose");
  }
}
```

- 第一个视图 lib/pages/dependency_put_find/index.dart

```dart
class StateDependencyPutFindView extends StatelessWidget {
  StateDependencyPutFindView({Key? key}) : super(key: key);

  final controller = Get.put<CountController>(CountController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Dependency"),
      ),
      body: Center(
        child: Column(
          children: [
            GetX<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                return Text('value -> ${_.count}');
              },
            ),
            Divider(),

            // 按钮
            ElevatedButton(
              onPressed: () {
                controller.add();
              },
              child: Text('add'),
            ),

            // 跳转
            ElevatedButton(
              onPressed: () {
                Get.to(NextPageView());
              },
              child: Text('next page'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- 第二个视图 lib/pages/dependency_put_find/next_page.dart

```dart
class NextPageView extends StatelessWidget {
  NextPageView({Key? key}) : super(key: key);

  final controller = Get.find<CountController>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("NextPage"),
      ),
      body: Center(
        child: Column(
          children: [
            GetX<CountController>(
              init: controller,
              initState: (_) {},
              builder: (_) {
                return Text('value -> ${_.count}');
              },
            ),
            Divider(),
          ],
        ),
      ),
    );
  }
}
```

#### Get.lazyPut + GetView 懒加载

- 控制器 lib/pages/dependency_lazyPut/controller.dart

```dart
class CountController extends GetxController {
  final _count = 0.obs;
  set count(value) => this._count.value = value;
  get count => this._count.value;

  add() => _count.value++;

  @override
  void onInit() {
    super.onInit();
    print("onInit");
  }

  @override
  void onClose() {
    super.onClose();
    print("onClose");
  }
}
```

- 第一个视图 lib/pages/dependency_lazyPut/index.dart

```dart
class StateDependencyLazyPutView extends StatelessWidget {
  StateDependencyLazyPutView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Dependency - LazyPut"),
      ),
      body: Center(
        child: Column(
          children: [
            GetX<CountController>(
              init: Get.find<CountController>(),
              initState: (_) {},
              builder: (_) {
                return Text('value -> ${_.count}');
              },
            ),
            Divider(),

            // 按钮
            ElevatedButton(
              onPressed: () {
                Get.find<CountController>().add();
              },
              child: Text('add'),
            ),

            // 跳转
            ElevatedButton(
              onPressed: () {
                Get.to(NextPageView());
              },
              child: Text('Next GetView Page'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- 第二个视图 lib/pages/dependency_lazyPut/next_getview_page.dart

```dart
class NextPageView extends GetView<CountController> {
  NextPageView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("GetView Page"),
      ),
      body: Center(
        child: Column(
          children: [
            Obx(() => Text('value -> ${controller.count}')),
            Divider(),

            // 按钮
            ElevatedButton(
              onPressed: () {
                controller.add();
              },
              child: Text('add'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- 绑定 lib/pages/dependency_lazyPut/bindings.dart

```dart
class DependencyLazyPutBinding implements Bindings {
  @override
  void dependencies() {
    Get.lazyPut<CountController>(() => CountController());
  }
}
```

- 路由 lib/common/routes/app_pages.dart

```dart
          GetPage(
              name: AppRoutes.DependencyLazyPut,
              binding: DependencyLazyPutBinding(),
              page: () => StateDependencyLazyPutView()),
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

#### Dart 编程语言基础

https://space.bilibili.com/404904528/channel/detail?cid=111585

#### Flutter 零基础入门

https://space.bilibili.com/404904528/channel/detail?cid=123470

#### Flutter 实战从零开始 新闻客户端

https://space.bilibili.com/404904528/channel/detail?cid=106755

#### Flutter 组件开发

https://space.bilibili.com/404904528/channel/detail?cid=144262

#### Flutter 组件开发

https://space.bilibili.com/404904528/channel/detail?cid=144262

#### Flutter Bloc

https://space.bilibili.com/404904528/channel/detail?cid=177519

#### Flutter Getx4

https://space.bilibili.com/404904528/channel/detail?cid=177514

#### Docker Yapi

https://space.bilibili.com/404904528/channel/detail?cid=130578
