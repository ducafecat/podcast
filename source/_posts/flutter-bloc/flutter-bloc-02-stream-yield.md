---
title: Flutter Bloc 02 - 基础对象 stream yield
date: 2021-2-20 00:00:00
tags: flutter bloc
categories: Flutter Bloc
---

![](2021-02-21-09-11-23.png)

# 本节目标

- stream 创建、订阅、观察
- yield 生成器

## 视频

## 代码

https://github.com/ducafecat/flutter-bloc-learn/tree/master/stream_yield

## 正文

### 准备工具函数

```dart
// 打印流列表
printStream(Stream<Object> stream) async {
  await for (var val in stream) {
    print(val);
  }
}

// 异步函数i
Future<int> funi = Future(() {
  return 100;
});

// 异步函数ii
Future<int> funii = Future(() {
  return 200;
});
```

### stream 创建

- 延迟间隔

```dart
periodic() async {
  Stream<int> stream = Stream<int>.periodic(Duration(seconds: 1), (val) => val);
  await printStream(stream);
}
```

- futrue 数据源

```dart
fromFuture() async {
  Stream<int> stream = Stream<int>.fromFuture(funi);
  await printStream(stream);
}
```

- futrues 多数据源

```dart
fromFutures() async {
  Stream<int> stream = Stream<int>.fromFutures([
    funi,
    funii,
  ]);
  await printStream(stream);
}
```

### stream 监听

- 单对单

```dart
listen() async {
  Stream<int> stream = Stream<int>.periodic(Duration(seconds: 1), (val) => val);
  stream.listen(
    (event) {
      print(event);
    },
    onError: (err) {
      print(err);
    },
    onDone: () {},
    cancelOnError: true,
  );
}
```

- 广播

```dart
boardcast() async {
  Stream<int> stream = Stream<int>.periodic(Duration(seconds: 1), (val) => val)
      .asBroadcastStream();
  stream.listen((event) {
    print(event);
  });
  stream.listen((event) {
    print(event);
  });
}
```

- task skip

```dart
opt() async {
  Stream<int> stream = Stream<int>.fromIterable([1, 2, 3, 4, 5]);
  stream = stream.take(3);
  // stream = stream.skip(2);

  stream.listen((event) {
    print(event);
  });
}
```

### stream 流控制类

- StreamController 单点

```dart
scListen() async {
  StreamController sc = StreamController(
      onListen: () => print("onListen"),
      onPause: () => print("onPause"),
      onResume: () => print("onResume"),
      onCancel: () => print("onCancel"),
      sync: false);

  // 订阅对象
  StreamSubscription ss = sc.stream.listen(((event) {
    print(event);
  }));

  sc.add(100);

  // 暂停
  ss.pause();
  // 恢复
  ss.resume();
  // 取消
  ss.cancel();

  // 关闭流
  sc.close();
}
```

- StreamController 广播

```dart
scBroadcast() async {
  StreamController sc = StreamController.broadcast();

  sc.stream.listen(print);
  sc.stream.listen(print);

  sc.addStream(Stream.fromIterable([1, 2, 3, 4, 5]));

  await Future.delayed(Duration(seconds: 2));
  sc.close();
}

```

- StreamTransformer

```dart
scTransformer() async {
  StreamController sc = StreamController<int>.broadcast();

  StreamTransformer stf = StreamTransformer<int, double>.fromHandlers(
    handleData: (int data, EventSink sink) {
      sink.add((data * 2).toDouble());
    },
    handleError: (error, stacktrace, sink) {
      sink.addError('wrong: $error');
    },
    handleDone: (sink) {
      sink.close();
    },
  );

  Stream stream = sc.stream.transform(stf);
  stream.listen(print);
  stream.listen(print);

  sc.addStream(Stream<int>.fromIterable([1, 2, 3, 4, 5]));

  await Future.delayed(Duration(seconds: 2));
  sc.close();
}
```

### stream 执行

```dart

main(List<String> args) async {
  print('--- start ---');

  // await periodic();
  // await fromFuture();
  // await fromFutures();

  // await listen();
  // await boardcast();
  // await opt();

  // await scListen();
  // await scBroadcast();
  // await scTransformer();

  print('--- end ---');
}

```

### yield 异步生成器

```dart
// 生成多个异步函数
Stream<int> createStream(int max) async* {
  for (var i = 0; i < max; i++) {
    yield* fun(i);
  }
}

// 异步函数
Stream<int> fun(int val) async* {
  yield val;
}

// 异步函数执行并求和
Future<int> sumStream(Stream<int> items) async {
  int sumNum = 0;
  await for (var val in items) {
    print(val);
    sumNum += val;
  }
  return sumNum;
}

main(List<String> args) async {
  print('--- start ---');

  Stream<int> streamItems = createStream(10);
  int sumNum = await sumStream(streamItems);

  print('sumNum -> $sumNum');

  print('--- end ---');
}
```

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
