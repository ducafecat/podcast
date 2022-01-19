---
title: Flutter 后台运行代码
tags: flutter
categories: 译文
date: 2022-01-20 00:00:00
---

![](2022-01-20-07-24-43.png)

## 原文

> https://www.dltlabs.com/blog/flutter-run-code-in-the-background-513940

## 参考

- https://pub.dev/packages/background_fetch

## 正文

今天，我将解释如何在 Flutter 创建一个后台任务。

在此之前，让我们理解什么是后台任务。后台任务是在后台运行的应用程序的辅助进程，即使应用程序没有运行或处于终止状态。

这一功能对于需要在后台执行任务而不需要用户打开应用程序的应用程序来说是有益的ーー例如，每 15 分钟调用 api 获取数据。

让我们在一个示例项目中实现一个后台任务，以便更好地理解这一操作的含义。

### 步骤:

- pubspec.yaml

```
flutter pub add background_fetch
flutter pub get
```

- 在 main.dart 文件中导入后台包，并注册 HeadlessTask，以便在应用程序终止后接收 backgroundFetch 事件。

例如:

```dart
void backgroundFetchHeadlessTask(HeadlessTask task) async {var taskId = task.taskId;
if(taskId == ‘your_task_id’) {
print(‘your_task_id’);
print(‘[BackgroundFetch] Headless event received.’);
_//TODO: perform tasks like — call api, DB and local notification etc…
_}
}
void main() {
runApp(MyApp());
_//Registering backgroundFetch to receive events after app is terminated.
// Requires {stopOnTerminate: false, enableHeadless: true}
_BackgroundFetch._registerHeadlessTask_(backgroundFetchHeadlessTask);
}
```

这里我们必须传递一个顶级函数。让我们在 `registerHeadlessTask` 方法中给它命名为 `call back dispatcher`。然后我们定义需要在后台运行的任务:

### 配置 BackgroundFetch

```dart
Future<void> initPlatformState() async {
_// Configure BackgroundFetch.
_var status = await BackgroundFetch._configure_(BackgroundFetchConfig(
minimumFetchInterval: 15,
forceAlarmManager: false,
stopOnTerminate: false,
startOnBoot: true,
enableHeadless: true,
requiresBatteryNotLow: false,
requiresCharging: false,
requiresStorageNotLow: false,
requiresDeviceIdle: false,
requiredNetworkType: NetworkType.NONE,
), _onBackgroundFetch, _onBackgroundFetchTimeout);
print(‘[BackgroundFetch] configure success: $status’);
_// Schedule backgroundfetch for the 1st time it will execute with 1000ms delay.
// where device must be powered (and delay will be throttled by the OS).
_BackgroundFetch.scheduleTask(TaskConfig(
taskId: “com.dltlabs.task”,
delay: 1000,
periodic: false,
stopOnTerminate: false,
enableHeadless: true
));
}
```

调用 `initState` 中的 `initPlatformState` 方法并设置 `BackgroundFetchConfig` 类的配置。换句话说，在传递其他参数的同时，提供注册一次性任务或周期性任务的选项。

在这里，如果存在多个任务，任务 id 可以帮助我们轻松地识别单个任务。输入数据可以是处理任务所需的任何信息。

如果我们希望在应用程序处于终止状态时继续工作，请将 `stopOnTerminate` 参数的值设置为 `false` 。如果 `stopOnTerminate` 设置为 `true` ，后台服务将在应用程序终止时终止。

```dart
void _onBackgroundFetchTimeout(String taskId) {
print(“[BackgroundFetch] TIMEOUT: $taskId”);
BackgroundFetch.finish(taskId);
}
```

当操作系统没有执行后台任务或者任务无法在给定时间内运行时，将调用 `onBackgroundFetchTimeout` 方法。在这种方法中，我们可以用任务 Id 来处理任务。

```dart
void _onBackgroundFetch(String taskId) async {
if(taskId == ‘your_task_id’) {
print(‘[BackgroundFetch] Event received’);
//TODO: perform your task like : call the API’s, call the DB and local notification.
}
}
```

当后台服务执行事件时，将调用 `onBackgroundFetch` 方法。在这个方法中，我们将接收任务 id 作为一个参数，它可以用来处理任务。在需要调用 api 将数据保存到数据库或显示本地通知等情况下，这一点非常重要。

默认情况下，调用任务之后的时间间隔的频率为 15 分钟。如果你想把它设置成其他的东西，你也可以在这里做。在 Android 上，后台进程的最小时间间隔为 15 分钟。如果该值小于 15，Android 默认使用 15 分钟。

Also, we must also remember to make changes inside the info.plist and manifest.xml file for both iOS \& Android. We need to set some of the permissions, and we also need to copy and paste other settings. If you need these settings, you can get them at the following links: [Android](https://github.com/transistorsoft/flutter_background_fetch/blob/master/help/INSTALL-ANDROID.md) [, OS](https://github.com/transistorsoft/flutter_background_fetch/blob/master/help/INSTALL-IOS.md).

此外，我们还必须记住对 Android IOS 的 `info.plist` 和 `manifest.xml` 文件进行更改。我们需要设置一些权限，还需要复制和粘贴其他设置。如果你需要这些设置，你可以通过以下链接获得: [IOS](https://github.com/transistorsoft/flutter_background_fetch/blob/master/help/INSTALL-IOS.md) , [Android](https://github.com/transistorsoft/flutter_background_fetch/blob/master/help/INSTALL-ANDROID.md)。

谢谢你的阅读！

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
