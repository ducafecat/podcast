---
title: Flutter 零基础入门中文教学 - 04 配置 Android 开发环境
date: 2019-06-20 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- 安装 Android Studio
- 配置 Flutter 连接 Android Studio
- 配置 Android 模拟器
- 在 Android 模拟器中运行 Flutter App

## 1. 安装 Android Studio

https://developer.android.google.cn/studio

### 1.2 "unable to access android sdk add-on list" 点击取消

![unable to access android sdk add-on list](image-20190522104947380.png)

### 1.3 自定义安装，全选项目

![all](image-20190522105204530.png)

### 1.4 配置模拟器

- 进去 AVD Manage

![](image-20190522110737156.png)

- 不要选最新的模拟器镜像

![](2019-07-02-14-13-42.png)

- 配置模拟器参数

大家机器好点的，就多给点内存和空间吧，这样模拟器运行的快些

![](image-20190522112214757.png)

- 运行模拟器

![](image-20190522112401274.png)

## 2. 配置环境变量

```sh
> vi ~/.bash_profile

# android
export ANDROID_HOME=~/Library/Android/sdk
export PATH=${PATH}:${ANDROID_HOME}/platform-tools
export PATH=${PATH}:${ANDROID_HOME}/tools

source ~/.bash_profile
```

## 3. 运行 Flutter

### 3.1 创建项目

- crate & run

```sh
> flutter create my_app
> cd my_app
> flutter run

Using hardware rendering with device Android SDK built for x86. If you get graphics artifacts, consider enabling software rendering
with "--enable-software-rendering".
Launching lib/main.dart on Android SDK built for x86 in debug mode...
Initializing gradle...                                              1.4s
Resolving dependencies...                                           2.2s
Running Gradle task 'assembleDebug'...
Running Gradle task 'assembleDebug'... Done                         2.2s
Built build/app/outputs/apk/debug/app-debug.apk.
Installing build/app/outputs/apk/app.apk...                         2.2s
D/EGL_emulation( 5614): eglMakeCurrent: 0xe2c05300: ver 3 0 (tinfo 0xe2c03350)
Syncing files to device Android SDK built for x86...
 2,067ms (!)

🔥  To hot reload changes while running, press "r". To hot restart (and rebuild state), press "R".
An Observatory debugger and profiler on Android SDK built for x86 is available at: http://127.0.0.1:64823/uqW8O20byg8=/
For a more detailed help message, press "h". To detach, press "d"; to quit, press "q".
```

![](2019-07-02-14-20-41.png)

## 参考

- [Flutter SDK MacOS install](https://flutter.dev/docs/get-started/install/macos)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
