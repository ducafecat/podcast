---
title: Flutter Bloc 02 - 基础对象 Stream 流操作
date: 2021-2-20 00:00:00
tags: flutter bloc
categories: Flutter Bloc
---

![](2021-02-21-09-11-23.png)

# 本节目标

- Stream 创建
- StreamController 控制
- StreamSubscription 订阅
- StreamTransformer 转换
- Sink、StreamSink、EventSink 修改数据

## 视频

## 代码

https://github.com/ducafecat/flutter-bloc-learn/tree/master/stream

## 正文

### 核心类

![](2021-02-22-17-26-26.png)

![](2021-02-22-17-34-07.png)

| 名称               | 说明                                   |
| ------------------ | -------------------------------------- |
| Stream             | 事件流或者管道                         |
| StreamController   | 事件管理者                             |
| StreamSubscription | 管理事件订阅，如 cacenl、pause         |
| StreamSink         | 流 Sink 入口，提供如 add、addStream 等 |
| EventSink          | 事件 Sink 入口                         |
| StreamTransformer  | 流转换                                 |

### 准备函数

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

#### 延迟间隔

```dart
periodic() async {
  Stream<int> stream = Stream<int>.periodic(Duration(seconds: 1), (val) => val);
  await printStream(stream);
}

>>
1
2
3
4
5
6

```

#### futrue 数据源

```dart
fromFuture() async {
  Stream<int> stream = Stream<int>.fromFuture(funi);
  await printStream(stream);
}

>>

100

```

#### futrues 多数据源

```dart
fromFutures() async {
  Stream<int> stream = Stream<int>.fromFutures([
    funi,
    funii,
  ]);
  await printStream(stream);
}

>>

100
200

```

### stream 监听

#### 单对单

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

>>

1
2
3
4
5
6

```

#### 广播

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

>>

1
1
2
2
3
3
4
4
5
5

```

#### 操作 task skip

```dart
opt() async {
  Stream<int> stream = Stream<int>.fromIterable([1, 2, 3, 4, 5]);
  stream = stream.take(3);
  // stream = stream.skip(2);

  stream.listen((event) {
    print(event);
  });
}

>> take(3)
1
2
3

>> skip(2)
3
4
5

```

### StreamController 流控制类

![](2021-02-22-17-40-50.png)

#### 单点

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

>>

onListen
onPause
onCancel

```

#### 广播

```dart
scBroadcast() async {
  StreamController sc = StreamController.broadcast();

  StreamSubscription ss1 = sc.stream.listen(print);
  StreamSubscription ss2 = sc.stream.listen(print);

  sc.addStream(Stream.fromIterable([1, 2, 3, 4, 5]));

  await Future.delayed(Duration(seconds: 2));
  sc.close();
}

>>

1
1
2
2
3
3
4
4
5
5

```

### StreamTransformer 流转换

![](2021-02-22-17-45-33.png)

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

>>
2
2
4
4
6
6
8
8
10
10

```

### 执行

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

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
