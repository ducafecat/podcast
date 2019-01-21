---
title: Dart语言学习 - 32 线程隔离 isolate
date: 2019-01-21 15:52:55
tags: dart
categories: Dart语言学习
---

# 本节目标

- 了解线程隔离

# 环境

- Dart 2.1.0

# isolate

在Dart中实现并发可以用Isolate，它是类似于线程(thread)但不共享内存的独立运行的worker，是一个独立的Dart程序执行环境。其实默认环境就是一个main isolate。

在Dart语言中，所有的Dart代码都运行在某个isolate中，代码只能使用所属isolate的类和值。不同的isolate可以通过port发送message进行交流。

# 示意图

![](2019-01-21-16-09-42.png)

- `ReceivePort` 创建入口点
- `Isolate.spawn` 连接进程
- `SendPort.send` 发送消息

# echo 例子

```dart
import 'dart:async';
import 'dart:isolate';

main() async {
  var receivePort = new ReceivePort();
  await Isolate.spawn(echo, receivePort.sendPort);

  // first 是 echo 线程的消息入口
  var sendPort = await receivePort.first;

  var msg = await sendReceive(sendPort, "foo");
  print('received $msg');
  msg = await sendReceive(sendPort, "bar");
  print('received $msg');
}

// 隔离的入口点
echo(SendPort sendPort) async {
  // 打开接收端口接收传入的消息。
  var port = new ReceivePort();

  // 通知任何其他隔离此隔离侦听的端
  sendPort.send(port.sendPort);

  // 循环接收消息
  await for (var msg in port) {
    var data = msg[0];
    SendPort replyTo = msg[1];
    replyTo.send(data);
    if (data == "bar") port.close();
  }
}

// 发送并接收消息
Future sendReceive(SendPort port, msg) {
  ReceivePort response = new ReceivePort();
  port.send([msg, response.sendPort]);
  return response.first;
}
```

# 代码

- []()

# 参考

- [isolates](https://www.dartlang.org/guides/language/language-tour#isolates)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
