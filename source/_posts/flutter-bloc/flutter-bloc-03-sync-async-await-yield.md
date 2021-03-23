---
title: Flutter Bloc 03 - 基础对象 同步、异步 await yield 操作
date: 2021-3-16 00:00:00
tags: flutter bloc
categories: Flutter Bloc
---

![](2021-03-16-15-41-57.png)

# 本节目标

- 同步、异步 `sync` `async`
- 关键字 `await` `yield`
- 加上 `*` 的区别

## 视频

https://www.bilibili.com/video/BV1JZ4y1w7hX/

## 代码

https://github.com/ducafecat/flutter-bloc-learn/tree/master/sync-async

## 正文

## 在 BLOC 中常见 yield yield\* Stream<T>

计算器 [Bloc 代码](https://github.com/ducafecat/flutter-bloc-learn/tree/master/ducafecat_bloc_start_example)

我们可以发现在 bloc 模块中，非常多 `yield*` `yield` `async*` ，如何正确使用还是很重要的，所以这篇文章把同步、异步的对应的操作符都整理出来。

```dart
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterInitial(0));

  int counterNum = 0;

  @override
  Stream<CounterState> mapEventToState(
    CounterEvent event,
  ) async* {
    if (event is CounterIncrementEvent) {
      yield* _mapIncrementEventToState(event);
    } else if (event is CounterSubductionEvent) {
      yield* _mapSubductionEventToState(event);
    }
  }

  Stream<CounterState> _mapIncrementEventToState(
      CounterIncrementEvent event) async* {
    this.counterNum += 1;
    yield CounterChange(this.counterNum);
  }

  Stream<CounterState> _mapSubductionEventToState(
      CounterSubductionEvent event) async* {
    this.counterNum -= 1;
    yield CounterChange(this.counterNum);
  }
}

```

### 同步 sync\* + yield

同步 sync 后返回 Iterable<T> 可序列化对象

- 代码

```dart
main() {
  getList(10).forEach(print);
}

Iterable<int> getList(int count) sync* {
  for (int i = 0; i < count; i++) {
    yield i;
  }
}
```

- 输出

```
0
1
2
3
4
5
6
7
8
9
Exited
```

- 我如果把 `sync` 的 `*` 去掉,编辑器会提示这是固定格式。

![](2021-03-23-08-05-28.png)

### 同步 sync* + yield*

带上 \* 因为 yield 返回对象是 Iterable<T>

- 代码

```dart
main() {
  getList(10).forEach(print);
}

Iterable<int> getList(int count) sync* {
  yield* generate(count);
}

Iterable<int> generate(int count) sync* {
  for (int i = 0; i < count; i++) {
    yield i;
  }
}
```

- 输出

```
0
1
2
3
4
5
6
7
8
9
Exited

```

- 我把 `yield` 的 `*` 去掉后，提示返回 `Iterable<T>` 必须带上 `*`

![](2021-03-23-08-09-58.png)

### 异步 async + await

Future + async + await 经典配合

常见场景，等待异步完成，比如拉取数据、 IO 操作

- 代码

```dart
main() {
  print("start..........");
  getList(10).then(print);
}

Future<int> getList(int count) async {
  await sleep();
  for (int i = 0; i < count; i++) {
    return i;
  }
  return 99;
}

Future sleep() async {
  return Future.delayed(Duration(seconds: 3));
}
```

- 输出

```
start..........
0
Exited
```

这里就直接返回了, 没有后续的任何操作。

### 异步 async\* + yield

带上 `*` 后，yield 返回 Stream<T> 对象

接收方用 listen(...)

- 代码

```dart
main() {
  getList(10).listen(print);
}

Stream<int> getList(int count) async* {
  for (int i = 0; i < count; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}
```

- 输出

```
0
1
2
3
4
5
6
7
8
9
Exited
```

- yield 必须和 `async*` 或 `sync*` 配套使用

![](2021-03-23-08-14-38.png)

### 异步 async* + yield*

yield\* 后返回的是另一个 Stream<T> 对象

- 代码

```dart
main() {
  getList(10).listen(print);
}

Stream<int> getList(int count) async* {
  yield* generate(count);
}

Stream<int> generate(int count) async* {
  for (int i = 0; i < count; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

```

- 输出

```
0
1
2
3
4
5
6
7
8
9
Exited
```

- 返回 `Stream<T>` 类型必须是用 `yield*` 的方式

![](2021-03-23-08-16-23.png)

- ***

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)

[https://ducafecat.gitee.io](https://ducafecat.gitee.io)
