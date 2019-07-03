---
title: Flutter 零基础入门中文教学 - 02 安装 SDK MacOS
date: 2019-06-18 00:00:00
tags: flutter
categories: Flutter零基础入门中文教学
---

## 本节目标

- 采用 git 方式安装 SDK
- 编译代码 flutter tool
- 检查环境 flutter doctor

## 1. 安装 SDK

### 1.1 方式一：下载SDK包

- [SDK包下载](https://flutter.dev/docs/development/tools/sdk/releases)

> 解压到 ~/Documents/sdk/flutter

### 1.2 方式二：git 拉取源码

```sh
> mkdir ~/Documents/sdk
> cd ~/Documents/sdk
> git clone -b stable https://github.com/flutter/flutter.git
```

> 推荐，下次更新直接 `git pull`

## 2. 配置环境变量

```sh
> vi ~/.bash_profile

# flutter
export PATH="${PATH}:~/Documents/sdk/flutter/bin"

# 以下两行适合国内
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

> source ~/.bash_profile
```

## 3. zsh 用户修改配置文件

```sh
> vi ~/.zshrc

最后一行加入
source ~/.bash_profile

重启终端生效
```

## 4. 命令行运行 flutter

```sh
> flutter doctor
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
Building flutter tool...

  ╔════════════════════════════════════════════════════════════════════════════╗
  ║                 Welcome to Flutter! - https://flutter.dev                  ║
  ║                                                                            ║
  ║ The Flutter tool anonymously reports feature usage statistics and crash    ║
  ║ reports to Google in order to help Google contribute improvements to       ║
  ║ Flutter over time.                                                         ║
  ║                                                                            ║
  ║ Read about data we send with crash reports:                                ║
  ║ https://github.com/flutter/flutter/wiki/Flutter-CLI-crash-reporting        ║
  ║                                                                            ║
  ║ See Google's privacy policy:                                               ║
  ║ https://www.google.com/intl/en/policies/privacy/                           ║
  ║                                                                            ║
  ║ Use "flutter config --no-analytics" to disable analytics and crash         ║
  ║ reporting.                                                                 ║
  ╚════════════════════════════════════════════════════════════════════════════╝


Doctor summary (to see all details, run flutter doctor -v):

Oops; flutter has exited unexpectedly.
Crash report written to /Users/ducafecat/flutter_01.log;
please let us know at https://github.com/flutter/flutter/issues.
```

> 第一次运行会进行 build

- CommandLineTools 工具推荐，先安装 xcode （早晚都要安装的）

然后 Terminal 运行 xcode-select --install

![xcode-select --install](image-20190628153509349.png)

## 5. 检查环境 flutter doctor

```sh
> flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.5.4-hotfix.2, on Mac OS X 10.14.5 18F132, locale zh-Hans-CN)
[!] Android toolchain - develop for Android devices (Android SDK version 29.0.0)
    ✗ Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses
[✗] iOS toolchain - develop for iOS devices
    ✗ Xcode installation is incomplete; a full installation is necessary for iOS development.
      Download at: https://developer.apple.com/xcode/download/
      Or install Xcode via the App Store.
      Once installed, run:
        sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
    ✗ libimobiledevice and ideviceinstaller are not installed. To install with Brew, run:
        brew update
        brew install --HEAD usbmuxd
        brew link usbmuxd
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ ios-deploy not installed. To install:
        brew install ios-deploy
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS platform side's plugin code that responds to your plugin usage on the Dart side.
        Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
        For more info, see https://flutter.dev/platform-plugins
      To install:
        brew install cocoapods
        pod setup
[!] Android Studio (version 3.4)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Ultimate Edition (version 2019.1.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.35.1)
    ✗ Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
[!] Connected device
    ! No devices available

! Doctor found issues in 6 categories.
```

## 6. 查看版本 version

```sh
> flutter --version
Flutter 1.5.4-hotfix.2 • channel stable • https://github.com/flutter/flutter.git
Framework • revision 7a4c33425d (9 weeks ago) • 2019-04-29 11:05:24 -0700
Engine • revision 52c7a1e849
Tools • Dart 2.3.0 (build 2.3.0-dev.0.5 a1668566e5)
```

## 参考

- [Flutter SDK MacOS install](https://flutter.dev/docs/get-started/install/macos)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)