---
title: Flutter Bloc 01 - 快速上手 计算器
date: 2020-12-08 00:00:00
tags: flutter bloc
categories: Flutter Bloc
---

![](2020-12-08-17-54-43.png)

# 本节目标

- 为什么要用 bloc
- bloc vs provider
- 学习路线推荐
- 安装 bloc vscode 插件
- 配置 bloc 依赖包
- 编写计算器示例

## 视频

https://www.bilibili.com/video/BV1ef4y1e79o/

## 代码

https://github.com/ducafecat/flutter-bloc-learn/tree/master/ducafecat_bloc_start_example

## 正文

### 为什么要用 bloc

![](2020-12-08-17-28-24.png)

- 状态管理（这是必须的）

- 三层分离

  - 表现层（Presentation)
  - 业务逻辑（Business Logic)
  - 数据层（Data)
    - 数据源/库（Repository)
    - 数据提供者（Data Provider)

- 规范组内开发

- 方便的 测试、记录 用户行为

```dart
class SimpleBlocDelegate extends BlocDelegate {
  @override
  void onEvent(Bloc bloc, Object event) {
    super.onEvent(bloc, event);
    print('${bloc.runtimeType} $event');

    // 所有的UI事件
    // 可用 umeng 这样平台 进行跟踪
  }

  @override
  void onError(Bloc bloc, Object error, StackTrace stacktrace) {
    super.onError(bloc, error, stacktrace);
    print('${bloc.runtimeType} $error');

    // 所有发生的错误
    // 用 sentry 记录错误
  }

  @override
  void onTransition(Bloc bloc, Transition transition) {
    super.onTransition(bloc, transition);
    print(transition);

    // 所有的 State 变化
  }
}

```

### bloc vs provider

- bloc 是一种 mvvm 基于 事件、状态 驱动的

- provider 是基于方法的

### bloc 学习路线

- Flutter Bloc 快速上手 -> Stream -> Cubit -> Bloc

- 理解基于 mvvm 组件化拆分

### 安装 bloc vscode 插件

bloc

![](2020-12-08-17-35-16.png)

### 创建项目

- pubspec.yaml

```yaml
name: ducafecat_bloc_start_example
...
dependencies:
  ...

  bloc: ^6.1.0
  flutter_bloc: ^6.1.0
  equatable: ^1.2.5
```

- 目录结构

![](2020-12-08-17-38-28.png)

counter 计算器业务下创建 bloc view 目录，这样就分离了

### 编写 bloc

- lib/counter/bloc/counter_bloc.dart

```dart
import 'dart:async';

import 'package:bloc/bloc.dart';
import 'package:equatable/equatable.dart';
import 'package:meta/meta.dart';

part 'counter_event.dart';
part 'counter_state.dart';

class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterInitial(0));

  int counterNum = 0;

  @override
  Stream<CounterState> mapEventToState(
    CounterEvent event,
  ) async* {
    if (event is CounterIncrement) {
      yield* _mapIncrementEventToState(event);
    } else if (event is CounterSubduction) {
      yield* _mapSubductionEventToState(event);
    }
  }

  Stream<CounterState> _mapIncrementEventToState(
      CounterIncrement event) async* {
    this.counterNum += 1;
    yield CounterChange(this.counterNum);
  }

  Stream<CounterState> _mapSubductionEventToState(
      CounterSubduction event) async* {
    this.counterNum -= 1;
    yield CounterChange(this.counterNum);
  }
}

```

- lib/counter/bloc/counter_event.dart

```dart
part of 'counter_bloc.dart';

@immutable
abstract class CounterEvent extends Equatable {
  @override
  List<Object> get props => [];
}

class CounterIncrement extends CounterEvent {}

class CounterSubduction extends CounterEvent {}

```

- lib/counter/bloc/counter_state.dart

```dart
part of 'counter_bloc.dart';

@immutable
abstract class CounterState extends Equatable {
  final int value;

  const CounterState(this.value);

  @override
  List<Object> get props => [value];
}

class CounterInitial extends CounterState {
  CounterInitial(int value) : super(value);
}

class CounterChange extends CounterState {
  CounterChange(int value) : super(value);
}

```

### 编写 view

- lib/counter/view/page.dart

```dart
import 'package:ducafecat_bloc_start_example/counter/bloc/counter_bloc.dart';
import 'package:ducafecat_bloc_start_example/counter/view/view.dart';
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class CounterPage extends StatelessWidget {
  const CounterPage({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => CounterBloc(),
      child: CounterView(),
    );
  }
}

```

- lib/counter/view/view.dart

```dart
import 'package:ducafecat_bloc_start_example/counter/bloc/counter_bloc.dart';
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class CounterView extends StatelessWidget {
  const CounterView({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Counter')),
      body: Center(
        child: Column(
          children: [
            BlocBuilder<CounterBloc, CounterState>(
              builder: (context, state) {
                return Text('${state.value}');
              },
            ),
            RaisedButton(
              child: Text('加法'),
              onPressed: () {
                BlocProvider.of<CounterBloc>(context).add(CounterIncrement());
              },
            ),
            RaisedButton(
              child: Text('加法'),
              onPressed: () {
                BlocProvider.of<CounterBloc>(context).add(CounterSubduction());
              },
            )
          ],
        ),
      ),
    );
  }
}

```

### 替换主程序 widget

- lib/main.dart

```dart
class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      ...
      home: CounterPage(), //MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

## 参考

- https://bloclibrary.dev/
- https://pub.flutter-io.cn/packages/bloc
- https://pub.flutter-io.cn/packages/flutter_bloc
- https://pub.flutter-io.cn/packages/equatable
- https://pub.flutter-io.cn/packages/provider

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
