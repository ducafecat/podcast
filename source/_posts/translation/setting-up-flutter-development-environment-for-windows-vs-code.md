---
title: Windows Flutter 开发环境的建立
tags: flutter
categories: 译文
date: 2021-09-08 00:00:00
---

![](2021-09-07-17-32-43.png)

## 原文

> https://srihari-sundaramurugan.medium.com/setting-up-flutter-development-environment-for-windows-vs-code-93820f4caa57

## 正文

> 作为一个开发人员，我一直想尝试 flutter 已经很久了。当我开始使用 Flutter 时，设置它花费了我大量的时间，我不得不通过各种教程和文章来设置开发环境。我写这篇博客是因为我不想让我的开发同事们在这个过程中面对我不得不面对的错误。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c8fae13f1b6e9207d47a8438764a7aec57480872f6dc52336bf6a2aed3154667.png)

## 系统要求:

要安装和运行 Flutter，您的开发环境必须满足以下最低要求:

- **Operating Systems 操作系统**: Windows 7 SP1 或更高版本\(64 位\) ，基于 x86-64
- **Disk Space 磁盘空间**: 1.64 GB \(不包括 IDE/工具的磁盘空间\)
- **Tools 工具** : Flutter 取决于这些工具在您的环境中是否可用
- [Windows PowerShell 5.0](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-windows-powershell) or newer \(this is pre-installed with Windows 10\) 或更新\(这是预装 Windows 10\)
- [Git for Windows 适用于 Windows 的 Git](https://git-scm.com/download/win) 2. x, with the 2. x，加上**Use Git from the Windows Command Prompt 使用命令提示符的 Git** option. 选择
- 如果已经安装了 Git for Windows，请确保您可以运行`git` 命令来自命令提示符或 PowerShell

# 需要下载/安装的东西:

以下是您在此过程中将要下载/安装的内容:

- **Flutter Flutter** SDK

> 软件开发软件开发工具包

- **Andriod Studio**
- **Java SE** Development Kit 开发工具包
- **Git 饭桶**for windows 为了窗户
- **VS Code VS 代码** \(Visual Studio Code\)

> 我们开始吧？

## Flutter SDK \(软件开发工具包\) :

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/528da7c315fd64c6963a9529e57f90f3a005b6d732914540ead9eb3270cbc0b6.png)

A clip from 一个剪辑自<https://flutter.dev/docs/get-started/install/windows>

您可以从正式的 Flutter 安装文档中下载 Flutter SDK 的最新稳定版本。目前 windows 的稳定版本是 2.2.3-stable，当你阅读这篇博客时可能会有所不同。下载当时可用的最新版本。

- . zip 中提取 Flutter SDK
- 在你的 c 盘中创建一个文件夹`C:\src` 并粘贴提取的 Flutter SDK

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/1336b4381497954d6c5a5c3958b51a6ca1099af8a1fb513c64dd938c0ff05c3b.png)

C 驱动下的 Flutter SDK

- 现在进入 Flutter 文件夹，然后进入垃圾箱
- 复制垃圾桶的路径

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/9ff69420d5a03e1d60e360f8fb48b2c9e805d4166cb2a030bbec3ca3bee8313d.png)

- 打开窗口搜索并键入`env`.

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/93ded20c98806cdf0119634814314a0bc7a026e2ffbddada4042522b2ecebc16.png)

- 现在打开“编辑环境变量为您的帐户”

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/d4a3821c825e36677f5dc4d2762fe5c5afede101827d539c8bbc19cb745cb67c.png)

- 双击`path`

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/2c2d9a0adce17303ed350a325dadf55ab507dda076d0f8d48c36702640461b21.png)

- Click 点击`New` 并粘贴的路径`flutter\bin` 然后按「确定」
- 现在，打开 cmd \(或任何终端窗口\)并输入以下命令

`dart where flutter dart`

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/81878f1cf6c17c252104f7c28299d31a2beac02688b10b71d34d414992da8cc5.png)

如果您得到了 Flutter 和省道文件的位置，您已经成功地安装了 Flutter SDK 并设置了它的路径。

## **Java SE** 开发工具包:

> 如果您的 pc 上已经安装了 JDK，请确保将其设置为“ JAVA.home”路径。

**现在，让我们设置 java se:**

- Download “Java SE 8” from 从网站下载「 javase8」[JDK SE 8 DOWNLOAD 8 DOWNLOAD](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html).
- Under “Java SE Development Kit 8u301”, you’ll see this 在“ javase 开发工具包 8u301”下，您将看到这个

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bc96265feb8f73b23292d48d8818a69a9e2cb82cf40790380d42e39fc2923585.png)

<https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html>

- 下载并安装 x64/x86 w.r.t. 您的 PC 的规范
- 安装后，导航到 `\bin` : 在我的案例中:

`dart C:Program FilesJavajdk1.8.0_301bin`

- 复制路径，打开“编辑环境变量为您的帐户”
- 创建一个新变量`JAVA_HOME` .

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/abfc757e152824f4f22d4a41b068ec9e7fc1fe1b78f14f947df8535cdd94dc5b.png)

JAVA_HOME Environment Variable 环境变量

您已经完成了 JDK 的设置

## Andriod Studio:

> 我们正在下载 andriod studio 来建立 andriod 模拟器。

- 下载 Andriod Studio[ANDRIOD STUDIO](https://developer.android.com/studio).
- Install it. 安装它
- 现在，打开 CMD/Terminal，运行以下命令:

`dart flutter doctor`

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/62084fbc0c62df82f04a07364703ea39b72a72c04c9aa624c6473c5c6bb0c3dc.png)

> 你可能不会得到绿蜱的一切，检查只有 andriod 工作室，因为现在。

- 如果 Flutter 找不到它，就跑 `flutter config --android-studio-dir <directory>` 设置 Android Studio 安装的目录
- 打开 Andriod Studio

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/792bb40366aba820edd5aec1d8a3b7c73f797e6e3c8675e6928d135ff354156c.png)

> 如果你想在 Andriod Studio 开发应用程序,
>
> \- Go to plugins, search for “Dart” and “Flutter”.
>
> \- 下载它们，并重新启动 andriod studio。现在你可以在仪表板上看到一个“创建新的 Flutter 项目”。

- Select 选择 `More Actions`.

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/0ec615045f383a6ed9893c0d3fc9c6f996e0a3705117e564451d755f5e8fa1ee.png)

Andriod Studio

- Choose 选择`AVD Manager` .

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/b4534218f41630a78d555cd6b4d0ef3da6ece5bc87b8e7a9a112033a40ab8110.png)

AVD Manager — Andriod Studio

- Click 点击`+ Create Virtual Device…` .

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/3fa7890b0537419b29e57300385be62a8a621da82f325bd34e725d33ef259844.png)

Create Virtual Device — AVD Manager — Andriod Studio

- 选择一个你喜欢的 Andriod 设备，然后点击下一步

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/97d86ce409f48f9cf11eb21aa54efa74f0c6b5d98d6e11bad5e1447ca16b4a0a.png)

Create Virtual Device — AVD Manager — Andriod Studio

- 点击下载你想要的 Andriod 版本`Download` \(blue\), under \(蓝色\) ，在下面`Release Name` .
- 一旦下载完毕，点击下一步

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/3a3171298be84fe19e782193cf2bb626c4b2760cc5a8f7f93eacd1677cac5f89.png)

Create Virtual Device — AVD Manager — Andriod Studio

- Now, Finish. 现在，完成

🍻Cheers to you, you’re almost done\!. 🥳

为你干杯，你就快完成了

## Visual Studio 代码:

> 向我最喜欢的 IDE 问好！

- Download VS Code from 下载 VS 代码[VS CODE DOWNLOAD VS 代码下载](https://code.visualstudio.com/download).
- 安装并打开 VS 代码
- 开放式扩展

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bcae04a74a9ddfb3eaee8dadb750a53a43289289e84b82b2451673483badd1ef.png)

Extensions — VS Code

- Search for 搜寻`Flutter` in the extensions, and install this: 然后安装这个:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/d37562009b2c5170fdf4c107ee7d1b90747ca85a7212a85fa69530e841c89371.png)

VS Code Flutter 扩展

> 好了，一切都结束了。现在，是时候给心房 Flutter 医生做检查了。

- 打开 CMD/终端，然后运行

`dart flutter doctor`

- 您可能仍然有一个未检查的框

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/010a6535f853385b994c9fcd76bdd489228f49b5f83207011e54899d170cc55a.png)

Flutter Doctor Flutter

- 在你的终端上运行这个命令,

`dart flutter doctor \--andriod-licenses`

- Now, run 现在，跑吧`flutter doctor` again. 再一次

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/0b5a21e4827f6085be4eac4ee4d2d18804a04a3a1634c1fa07cd99ad68d8ef3d.png)

Flutter Doctor Flutter

> Yipieee，现在你已经准备好开发 Flutter 应用程序了。

写博客会让我感到快乐，并鼓励我写更多这样的博客。

---

© 猫哥

- https://ducafecat.tech/

- https://github.com/ducafecat

- 微信群 ducafecat

- b 站 https://space.bilibili.com/404904528

![](https://ducafecat.tech/img/public-qrcode.png)

## 往期

### 开源

#### GetX Quick Start

https://github.com/ducafecat/getx_quick_start

#### 新闻客户端

https://github.com/ducafecat/flutter_learn_news

### strapi 手册译文

https://getstrapi.cn

### 微信讨论群 ducafecat

### 系列集合

#### 译文

https://ducafecat.tech/categories/%E8%AF%91%E6%96%87/

#### 开源项目

https://ducafecat.tech/categories/%E5%BC%80%E6%BA%90/

#### Dart 编程语言基础

https://space.bilibili.com/404904528/channel/detail?cid=111585

#### Flutter 零基础入门

https://space.bilibili.com/404904528/channel/detail?cid=123470

#### Flutter 实战从零开始 新闻客户端

https://space.bilibili.com/404904528/channel/detail?cid=106755

#### Flutter 组件开发

https://space.bilibili.com/404904528/channel/detail?cid=144262

#### Flutter Bloc

https://space.bilibili.com/404904528/channel/detail?cid=177519

#### Flutter Getx4

https://space.bilibili.com/404904528/channel/detail?cid=177514

#### Docker Yapi

https://space.bilibili.com/404904528/channel/detail?cid=130578
