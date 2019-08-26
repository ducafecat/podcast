---
title: Dart语言学习 - 32 线程隔离 isolate
date: 2019-01-20 00:52:55
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

// 第1步：定义主线程
main() async {
  // 第3步：编写回调Port
  var receivePort = new ReceivePort();
  await Isolate.spawn(echo, receivePort.sendPort);

  // 第6步：保存隔离线程回调Port
  var sendPort = await receivePort.first;
  
	// 第7步：发送消息
  var msg = await sendReceive(sendPort, "foo");
  print('received $msg');
  msg = await sendReceive(sendPort, "bar");
  print('received $msg');
}

// 第2步：定义隔离线程的入口点
echo(SendPort sendPort) async {
  // 第4步：编写回调Port
  var port = new ReceivePort();

  // 第5步：告诉主线程回调入口点
  sendPort.send(port.sendPort);

  // 第8步：循环接收消息
  await for (var msg in port) {
    // 数组 msg[0] 是数据
    var data = msg[0];
    // 数组 msg[1] 是发送方Port
    SendPort replyTo = msg[1];
    // 回传发送方 数据
    replyTo.send(data);
    // 如果数据时 bar 关闭当前回调
    if (data == "bar") port.close();
  }
}

/*
主线程 发送消息函数
数组 msg[0] 是数据
数组 msg[1] 是发送方Port
返回 隔离线程 Port
*/
Future sendReceive(SendPort port, msg) {
  ReceivePort response = new ReceivePort();
  port.send([msg, response.sendPort]);
  return response.first;
}
```

# 代码

- [32-隔离](https://github.com/ducafecat/dart-learn/tree/master/32-%E9%9A%94%E7%A6%BB)

# 参考

- [isolates](https://www.dartlang.org/guides/language/language-tour#isolates)

----

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
