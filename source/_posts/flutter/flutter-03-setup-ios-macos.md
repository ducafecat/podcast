---
title: Flutter 零基础入门中文教学 - 03 配置 IOS 开发环境
date: 2019-06-19 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- 安装 xcode
- 配置 flutter 连接 xcode
- 在 IOS 模拟器中运行 flutter app

## 1. 安装 XCode

安装 Xcode 9.0 以上版本 (访问 [Apple网站](https://developer.apple.com/xcode/) 下载或者，[Mac App Store](https://itunes.apple.com/us/app/xcode/id497799835) 方式安装).

## 2. 第一次启动 XCode 安装所需组件

![](image-20190628180642579.png)

## 3. 配置 Xcode command-line tools

```sh
> sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer

检验 打印 license
> sudo xcodebuild -license
```

## 4. 启动模拟器

```sh
open -a Simulator
```

## 5. 创建 Flutter 项目

### 5.1 create & run

```sh
> flutter create my_app
> cd my_app
> flutter run

🔥  To hot reload changes while running, press "r". To hot restart (and rebuild state), press "R".
An Observatory debugger and profiler on iPhone Xʀ is available at: http://127.0.0.1:62341/ztmtijcoJrI=/
For a more detailed help message, press "h". To detach, press "d"; to quit, press "q".
```

![模拟器运行](image-20190701160914331.png)

### 5.2 vm 报告

```sh
http://127.0.0.1:62341/ztmtijcoJrI=/#/vm
```

![vm](image-20190701161106890.png)

## 6. 部署到真机

### 6.1 安装软件包

```sh
> brew update

> brew install --HEAD usbmuxd
> brew link usbmuxd
> brew install --HEAD libimobiledevice
> brew install ideviceinstaller ios-deploy cocoapods
> pod setup
```

> 安装 [homebrew](https://brew.sh/)

> pod setup 很慢的问题
>
> 1. 手动下载 git clone https://github.com/CocoaPods/Specs
> 2. 复制 ~/.cocoapods/repos/Specs-master
> 3. 执行 pod update
> 4. 复制 master 下的 .git 到 Specs-master
> 5. 停止 pod update
> 6. 重命名 Specs-master 为 master
> 7. 进入项目的 ios 目录下 pod install 成功

### 6.2 配置AppStore开发者账号

```sh
open ios/Runner.xcworkspace
```

![Add Account](image-20190701161445927.png)

> 开发者登录 https://developer.apple.com/cn/programs/

## 参考

- [Flutter SDK MacOS install](https://flutter.dev/docs/get-started/install/macos)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
