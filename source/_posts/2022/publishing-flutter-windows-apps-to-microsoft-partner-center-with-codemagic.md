---
title: 使用 Codemagic 将 Flutter Windows 应用程序发布到 Microsoft 合作伙伴中心
tags: flutter
categories: 译文
date: 2022-02-13 00:00:00
---

![](2022-02-13-22-18-18.png)

## 原文

https://medium.com/flutter-community/publishing-flutter-windows-apps-to-microsoft-partner-center-with-codemagic-b1962575510c

## 前言

> 这篇文章最初发表在 Codemagic 博客上，由 Souvik Biswas 撰写

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/cd5a9faeb54f0b7ef8314ae016a9b301fb5c297200bfcf70b406c1e18e5e40fc.png)

Flutter 允许您使用单个代码库为移动设备、网络、桌面和嵌入式设备构建应用程序。2.0 的引入使得试用桌面应用程序变得更加容易，因为这个选项现在可以在 stable 频道上使用。

本文将帮助您开始使用 Flutter 构建 Windows 桌面应用程序，生成一个版本 MSIX 构建，并使用 Codemagic 将该应用程序发布到微软合作伙伴中心。

如果你正在寻找一个建立 Flutter 桌面应用程序的更一般的入门指南，包括设计自适应布局，请查看这篇文章。

https://blog.codemagic.io/flutter-desktop-apps-intro/

## 代码

https://github.com/sbis04/flutter_desktop_sample
https://github.com/sbis04/flutter_desktop_sample

## 正文

### 为 Windows 创建一个 Flutter 应用程序

在你开始创建一个新的 Flutter 应用程序之前，你应该在你的 Windows 系统上安装 Flutter SDK。如果你没有安装 Flutter，按照安装指南这里。

https://docs.flutter.dev/get-started/install/windows

> 如果你已经在你的系统上安装了 Flutter，确保版本在 2.0 以上。您可以使用 Flutter -- version 命令检查您的 Flutter 版本。

要构建 Flutter 窗口应用程序，您应该在您的系统上安装 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。在安装 Visual Studio 时，如果你想构建 win32 应用程序，可以使用“带 c + + 的桌面开发”工作负载，如果你想构建 UWP 应用程序，可以使用“通用 Windows 平台开发”工作负载。

默认情况下，Flutter 使用 win32 来构建 Windows 应用程序:

```dart
flutter config --enable-windows-desktop
```

为了构建 UWP \(通用 Windows 平台\)应用程序，你需要在 Flutter 的开发通道。运行以下命令:

```dart
flutter channel dev
flutter upgrade
flutter config --enable-windows-uwp-desktop
```

运行 `flutter doctor` ，检查是否有任何未解决的问题。要验证窗口是否列为可用设备之一，请运行 `flutter devices` 命令。

要创建一个新的 Flutter 应用程序，请使用以下命令:

```dart
flutter create <project_name>
```

> 将 `_<project_name>_` 替换为您希望在项目中使用的名称ーー例如，flutter create flutter_desktop_sample。

上面的命令将创建一个 Flutter 计数器应用程序项目。你可以使用以下命令在 Windows 系统上运行它:

```dart
flutter run -d windows
```

要使用 UWP 运行应用程序，请使用以下命令:

```dart
flutter run -d winuwp
```

Windows UWP 应用程序需要一个沙盒环境才能运行，所以系统会提示您启动它。在一个单独的窗口中打开具有管理员权限的 PowerShell，并运行以下命令:

```dart
checknetisolation loopbackexempt -is -n=[APP_CONTAINER_NAME]
```

运行此进程后，返回到前一个窗口并按“ y”。这应该可以在 Windows 上启动 UWP 应用程序。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bce0454256f2088c01917e0aabf24aaec96fdc8e85066f5aef9bb9bbbbd00bfc.gif)

### 生成应用程序的可执行文件

为 Flutter Windows 应用程序生成.exe 可执行文件非常简单，只需运行以下命令:

```dart
flutter build windows
```

您可以通过访问 `<project_root>/build/windows/runner/Release/<app_name>.exe` 来找到生成的文件。执行。这个 `.exe` 文件可以分发给任何用户，用户可以直接在自己的系统上运行它。

有一个更安全和推荐的替代方法 `.exe` 文件ーー MSIX 包。MSIX 包的一些优点是:

- Windows 使用独立的环境安全地安装 MSIX 生成。它还有助于完全卸载应用程序。当你使用一个 `.exe` 即使在应用程序被删除之后，注册表文件中的更改仍然保留。
- 要将您的应用程序发布到 Microsoft Store，您需要一个 MSIX 包 `.exe` 文件不能直接发布。
- 可以有两种类型的 MSIX 包: 一种在本地运行，另一种用于分发到 Microsoft Store。可以将要生成的 MSIX 包的类型指定为生成参数。

在研究这两种类型的 MSIX 构建之前，让我们先创建一个 Microsoft 合作伙伴中心帐户。

### 创建 Microsoft 合作伙伴中心帐户

您需要一个 Microsoft 合作伙伴中心帐户才能使用 Microsoft Store 分发 Windows 应用程序。此外，在构建可分发的 MSIX 构建时，您需要指定一些应该与您的 Partner Center 应用程序中的属性相匹配的属性。

按照以下步骤创建和配置 Microsoft 合作伙伴中心应用程序:

- 你需要一个微软开发者帐户来发布应用到微软商店。你可以在这里注册一个。
  https://developer.microsoft.com/en-us/microsoft-store/register/

- 登录到微软合作伙伴中心。如果你没有帐户，你可以很容易地创建一个新的帐户。
  https://partner.microsoft.com/en-us/dashboard

- 从 Microsoft 合作伙伴中心的主页上，单击“添加程序”。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/b20b03f3c61c79ccb89e4305adb77dcbd0582f16936b620eb88c6a510c5b74c8.png)

- 在 Windows \& Xbox 下面选择 Get started，如果需要的话填写任何细节。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c5e34764895382c61c7a8d4d7520e8e3dfbf73165e95314179d555b453c98805.png)

- 进入 `Windows & Xbox > Overview`，点击 Create a new app 按钮。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/70f4d1c4297a4f0543877eefe9d169c1e296f2a7d5e8dad470fbcf2255f0cbf5.png)

- 输入应用程序的名称。点击保留产品名称。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/eeb7e5e869db30c9c6ed369a3a70e1a62953592df6dd0b15932951ed6a5a83d2.png)

这将在 Microsoft 合作伙伴中心上创建一个应用程序，并导航到应用程序概览页面。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/7845cda33accf5316307d54eaffd924d39617037e58b8b0360cbe3eac9ba764e.png)

在配置 MSIX 发行版本时，您将需要从这个 Microsoft 合作伙伴中心应用程序获得一些信息。

### 为 MSIX 构建进行配置

生成 MSIX 安装程序的最简单方法是使用名为 MSIX 的 Flutter 包。这是一个命令行工具，帮助创建一个 [msix](https://pub.dev/packages/msix) 安装程序从 Flutter 窗口建立文件。

在 `pubspec.yaml` 文件中添加 `dev_dependencies` 下的包:

```dart
dev_dependencies:
  msix: ^2.6.5
```

如果没有指定其他值，那么在构建 MSIX 时，包使用一些默认配置值。可以在 `pubspec.yaml` 文件中提供 MSIX 配置。

要生成一个本地 MSIX，在 `pubspec.yaml` 文件末尾添加以下配置:

```dart
msix_config:
  display_name: <AppName>
  publisher_display_name: <PublisherName>
  identity_name: <PublisherName.AppName>
  msix_version: 1.0.0.0
  logo_path: ./logo/<file_name.png>
```

在上面的配置中，用适当的值替换尖括号:

- `display_name`: 应用程序的名称，将显示给用户。
- `publisher_display_name`: 要显示给用户的发行者名称\(可以是个人名称或公司名称\)。
- `identity_name`: Windows 应用的唯一标识符。
- `msix_version`: 指定应用程序的构建版本。使用“ Major. Minor. Build. Revision”格式，其中“ Major”不能为“0”。
- `logo_path`: 徽标文件的相对路径\(可选\)。如果没有提供，则使用默认的 Flutter 徽标。

您可以在 `<project_directory>/logo` 文件夹中添加徽标文件。理想情况下，标志应该是一个 256x256 分辨率的正方形图像。你可以在这里阅读更多关于徽标大小的信息。

https://docs.microsoft.com/en-us/windows/apps/design/style/app-icons-and-logos

运行以下命令为您的 Flutter Windows 应用程序生成 MSIX 包:

```dart
flutter pub run msix:create
```

> 这个命令使用 `pubspec.yaml` 文件中定义的配置来生成 MSIX。
>
> 注意: 任何没有通过 Microsoft 合作伙伴中心发布的 MSIX 包都是未签名的包。要安装并运行一个未签名的 MSIX 软件包，您必须在本地信任并签名该应用程序，以便能够在 Windows 上运行它。

您可以通过访问 `<project_root>buildwindowsrunnerRelease<app_name>.msix` 来找到生成的包。M6.但是如果你双击。如果要在 Windows 系统上打开 msix 包，安装按钮将被禁用。

你需要签署与您的 Flutter 窗口应用程序本地安装在您的 Windows 系统的 MSIX 包。要做到这一点，请遵循以下步骤:

- 右键单击`.msix` 文件并选择 Properties。
- 转到属性对话框中的数字签名选项卡。
- 选择“ Msix Testing”签名者并单击“ Details”，然后单击“ View Certificate”。
- 单击“安装证书”开始安装证书。
- 在对话框中选择本地机器。单击“下一步”。
- 在“将所有证书放入以下存储区”下，单击“浏览”
- 选择“受信任根证书颁发机构”文件夹。单击“确定”。
- 点击下一步，然后完成
- 现在，双击文件。您将看到现在启用了 Install 按钮。点击它安装应用程序。

有了这个，您的 Flutter 窗口应用程序将安装在您的系统上。你可以通过在开始菜单中搜索找到它。

### 在 Microsoft 合作伙伴中心分发您的包

现在，让我们看看如何使用 Microsoft Partner Center 发布包。修改 `pubspec.yaml` 文件中的 MSIX 配置，如下所示:

```dart
msix_config:
  display_name: <AppName>
  publisher_display_name: <PublisherName>
  identity_name: <PublisherName.AppName>
  publisher: <PublisherID>
  msix_version: 1.0.0.0
  logo_path: ./logo/<file_name.png>
  store: true
```

这里定义了另外两个属性:

- `publisher`: 在 Microsoft 合作伙伴中心应用程序中显示的 Publisher ID。
- `store`: 将此设置为 `true` 将使用 Microsoft Partner Center 生成一个 MSIX 包可分发。

此外，确保 `display_name`、 `publisher_display_name` 和 `identity_name` 的属性值与合作伙伴中心中的值相匹配。通过在 Microsoft Partner Center 应用程序中导航到 Product management > Product Identity，您将找到这些值。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/361c992e877f0c189b1181a90d8b789853fe7ba8f81bfad28df2d416365aebd3.png)

> 注意: 如果您正在生成用于发布到 Microsoft Store 的 MSIX 包，那么您将无法在本地安装或运行证书。该软件包只能通过 Microsoft 合作伙伴中心发布。

现在，您已经配置您的 Flutter Windows 应用程序的发行版，让我们建立和分发它使用 Codemagic。一旦你开始为你的应用程序添加新的特性，Codemagic 使得分发更新的版本变得更加容易和快速。

### 将项目添加到 Codemagic

将项目添加到 Codemagic 的最简单方法是通过 Git 提供程序。将您的项目添加到 Git 提供程序中，比如\(GitHub、 Bitbucket 或 GitLab\) ，并按照下面的步骤开始使用 Codemagic:

- 登录 [Codemagic](https://codemagic.io/login)。如果你是一个新用户，请[sign up](https://codemagic.io/signup)。

- 连接到 Git 提供者，你已经上传了您的 Flutter 项目通过进入 [Settings](https://codemagic.io/settings) 下的集成。请确保您已经授权到上传应用程序的存储库。

- 导航到 [Applications](https://codemagic.io/apps) 页面，然后点击添加应用程序。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/11e0c2df251213162328caaf4df21dde664dba7afd678bce702ce8910d900948.png)

- 选择 Git provider:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/b58efa91c637757636e3b17f10165e46a7b1bf2d42b4ad5fd2862580abaea42e.png)

- 单击 Next: Authorize integration to Authorize Codemagic。如果您已经授权了您选择的 Git provider，请单击 Next: Select repository。

如果您使用 GitHub 作为您的 Git 提供者，那么您需要在选择存储库之前执行一个额外的步骤。单击 Install GitHub App 设置集成。您可以在这里了解更多关于如何配置 GitHub 应用程序集成的信息。

https://docs.codemagic.io/getting-started/adding-apps-from-custom-sources/#configuring-the-github-app-integration

- 现在，从下拉菜单中选择存储库，并选择 Flutter app 作为项目类型。然后单击 Finish: Add application:

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/c0b2841c8d66092ee83e9b382b4cfc8180274c4036238112c251d7898d6ed267.png)

- 您将被带到项目设置。工作流编辑器选项卡将被默认选中。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a86cb81e56b170a1379a93fa9da4ce33972a3df8cefb32fb12232acaf9a01e50.png)

### 使用 Codemagic 构建和发布

你可以使用 [Workflow Editor](https://docs.codemagic.io/flutter-configuration/flutter-projects/) 编辑器或 [Codemagic YAML](https://docs.codemagic.io/yaml/yaml-getting-started/) 文件在 Codemagic 上构建一个 Flutter 应用程序。要在 Codemagic 上构建 Windows 应用程序，您需要通过访问[这个页面](https://codemagic.io/billing/user)来启用计费。

### 启用 Microsoft 合作伙伴中心集成

Codemagic 使用 [Microsoft Store submission API](https://docs.microsoft.com/en-us/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services) 将 Windows 应用程序发布到 Microsoft Store。要使用这种集成，必须将 Microsoft 合作伙伴中心帐户链接到 Azure AD 应用程序，并向 Codemagic 提供必要的信息\(租户名、租户 ID、客户 ID 和客户机密\)。

按照下面的步骤生成必要的信息:

- 通过点击齿轮图标进入你的帐户设置。
- 浏览组织概要 > 租户。如果已经有一个现有租户，则可以将其与合作伙伴中心帐户关联。但是，建议为 Codemagic 创建一个新的租户。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/3edec69e2faae0192f3c0dba8c60e0b69e9a2224a237e60df631e0e57ee35b33.png)

- 要创建新租户，请单击 Create 并填写所需信息。

- 使用新租户访问你的 [Azure AD](https://azure.microsoft.com/en-us/services/active-directory/) 账户。

- 进入应用程式注册\(从左边的菜单\) ，然后点击「 + 新注册」。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/07d4b7a69ecd0a04b859bf7b566488e98c924f972617d9c69fc3bc359b91af62.png)

-输入应用程序的名称，并使用 Single tenant 选项。您可以将 Redirect URI 字段保持为空。点击注册。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/022db05f731fe1b2b93ba44c9e76bc5b5b1470efc248864df91b81f8e7c2b4d5.png)

- 进入证书和机密\(从左边的菜单\) ，进入客户机密标签，并单击“ + 新客户机机密”。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/0565f8942d8be6b5fd1eb0c2629353ed66bf9acb89334f919ae07b0940c73045.png)

- 输入描述，选择客户机密的有效期。点击添加。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/dabd9b5061d0de7ed4e30063a6885331f25032b4d626a8cff0f7d224ab1e9a1f.png)

注意 Value 字段，这是客户机机密。

- 最后，转到合作伙伴中心 > 帐户设置 > 用户管理。选择 Azure AD 应用程序，然后点击“ + Create Azure AD application”。选择您创建的应用程序，单击 Next，并将 Developer 角色赋予它。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/eb4da0c330d669f8631be81193489a0453d1bb4e40298ac94792af49564e0670.png)

现在，您可以进入 Codemagic 网页并启用 Microsoft 合作伙伴中心集成。它可以在用户设置 > 个人项目集成部分中启用，也可以在团队设置 > 团队帐户中共享项目的团队集成中启用。

按照以下步骤启用 Microsoft 合作伙伴中心集成:

- 在“集成”下，单击“合作伙伴中心”旁边的“连接”按钮。
- 输入所需的信息。通过进入合作伙伴中心 > 帐户设置 > 组织配置文件 > 租户，可以找到租户名和租户 ID。在 Azure AD 的概览页面上找到客户端 ID，然后输入您早先注意到的客户端秘密。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/9b18e857b41f95a3afdf3b4e521b550d7c40f14bf0be0f65deea5837edaf1db5.png)

- 点击保存。

### 配置代码魔法使用工作流编辑器颤动窗口应用程序

要使用工作流编辑器配置您的项目，请转到 Codemagic 上的 Workflow Editor 选项卡:

- 在“为平台生成”下选择“ Windows”。

- 将 VM 实例更改为 Windows。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bccd894b7483d3255017f292163317616c6fc44893cbdbd61bef07cc48cf38c4.png)

- 向下滚动到 Build 选项卡。选择生成项目所需的 Flutter 版本，选中 Create a Windows MSIX package 复选框，并将模式设置为 Release。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/70b855449e040a048513308c82e1714dd26e701fca1fc3c8f202428cfea5fa8d.png)

- 转到“分发”选项卡，以配置 Microsoft 合作伙伴中心分发。单击此处可展开 Microsoft 合作伙伴中心选项卡。选中“启用发布到 Microsoft 商店”复选框。

- 从下拉菜单中选择租户。输入我们在 [Configuring MSIX](https://blog.codemagic.io/publishing-flutter-windows-apps/#configuring-for-msix-build) 部分中讨论的其余信息。确保此信息与您的合作伙伴中心应用程序相匹配。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/da40cd20aeb1a30ac79a06e5e58efb61fee07a329b9345cdda226e48a9180368.png)

- 点击保存更改。

> 注意: 您必须手动将应用程序的第一个版本发布到合作伙伴中心。您可以从构建工件中下载 MSIX 包。

您已完成 Windows 发布工作流的配置。单击此页面顶部的“启动第一个构建”，然后单击“启动新构建”以启动构建过程。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a4a67a824c77e922e0282a3d15ec9898fc29759fd3556b9dda855220fd747cae.png)

### 使用 Codemagic.yaml 为 Flutter Windows 应用程序配置 Codemagic

要使用 YAML 配置构建，请转到 Flutter 项目，在根目录 `codemagic.yaml` 中创建一个新文件。

Add the following template to the file:

将以下模板添加到文件中:

```dart
workflows:
  my-workflow:
    name: Workflow name
    instance_type: mac_mini
    max_build_duration: 60
    environment:
      groups:
        - ...
      flutter: stable
    cache:
      cache_paths:
        - ~/.pub-cache
    scripts:
      - ...
    artifacts:
      - ...
    publishing:
      email:
        recipients:
          - name@example.com
```

这是一个在 Codemagic 上构建应用程序的基本工作流模板。

https://docs.codemagic.io/yaml/yaml-getting-started/

要生成一个 MSIX 包并使用 Microsoft 合作伙伴中心发布它，请按以下步骤修改工作流:

- 定义一个合适的工作流名称，并使用 Windows VM 实例:

`workflows: windows-release-workflow: name: Windows release workflow instance_type: windows_x2 max_build_duration: 60`

对于 Windows 生成，将 `instance_type` 设置为 `windows_x2`。

- 添加以下环境变量:

`environment: groups: \- windows-signing flutter: master`

您需要以加密的格式将客户机机密上传到 Codemagic 的应用程序环境变量\(`windows-signing `是应用程序环境变量的名称\)。

- 转到 Codemagic 上的项目页面，然后单击 Switch to YAML configuration。转到 Environment variables 选项卡，并添加凭据。输入变量名为 `PARTNER_CLIENT_SECRET`，然后将客户端机密值粘贴到变量值字段中。创建一个名为 `windows-signing` 的组。确保选中了安全复选框，然后单击“添加”。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/7187d92ae60673daa6f689e37a11a7d3cba498811aec63e573e6d44f864ec77b.png)

- 首先，在 `scripts` 部分，获取 Flutter 包:

`scripts: \- name: Get Flutter packages script: flutter packages pub get`

- 启用 Windows 平台:

`- name: Configure for Windows script: flutter config \--enable-windows-desktop`

- 使用 Flutter 构建 Windows 应用程序:

`- name: Build Windows script: flutter build windows`

- 生成可分发的 MSIX 包:

`- name: Package Windows script: flutter pub run msix:create`

- 如果您已经将 MSIX 配置添加到 `pubspec.yaml` 文件中，那么上面的命令应该适用于您。否则，您可以直接在 CLI 工具中提供配置，而无需对您的项目 `pubspec.yaml` 文件进行任何修改:

`- name: Package Windows script: | flutter pub add msix flutter pub run msix:create \--store \--publisher-display-name=<PublisherName> \--display-name=<AppName> \--publisher=<PublisherID> \--identity-name=<PublisherName.AppName> \--version=1.0.0.0`

- 为了检索生成的 MSIX，将 `artifacts` 路径更新为:

`artifacts: \- build/windows/**/*.msix`

- 在 `publishing` 部分，用你自己的 `email` 代替电子邮件。

- 若要向 Microsoft 伙伴中心发布，请在发布下添加以下内容 `publishing`:

`publishing: partner_center: store_id: <STORE_ID> tenant_id: <TENANT_ID> client_id: <CLIENT_ID> client_secret: $PARTNER_CLIENT_SECRET`

- 用适当的值替换尖括号。您将从 Azure AD Overview 页面获得 `tenant_id` 和 `client_id` 。你可以通过进入产品管理 > 产品标识，在微软合作伙伴中心应用程序中找到 `store_id` 。`client_secret` 是从前面定义的应用程序/环境变量中检索到的。

这样就完成了 `codemagic.yaml` 文件的配置。现在，只需提交并将文件添加到 Git 提供程序中，就可以开始了。

> 注意: 你应该通过微软合作伙伴中心手动发布第一个版本的应用。从仪表板上选择你的应用程序，点击开始提交，然后填写详细信息提交应用程序。第一个版本应该在使用 Codemagic 发布下一个版本之前完全发布。

要开始使用 YAML 文件构建 Flutter Windows 应用程序，请访问 Codemagic 上的项目页面，然后单击 Start your first build。选择您的工作流程，然后单击“启动新生成”以启动生成过程。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bdd33c0ffcabd0a1e875e309f1090b2a441f1e8343f552e7da552a1adc6b81e5.png)

## 结束了

祝贺你\! 你已经成功地建立了你的第一个 Flutter Windows 应用程序，并且使用 Codemagic 将它发布到微软商店。

一旦你开始给你的应用添加新的功能，你可以通过简单地更新构建编号和运行 Codemagic 构建，轻松地发布你的 Windows 应用的下一个版本。当然，你也可以添加其他工作流，这样 Codemagic 就可以为 Windows、 iOS、 Android 或 Linux 构建 Flutter 应用程序。

您可以在这里找到样本 Flutter Windows 应用程序的完整 codemagic.yaml 配置文件。在这个 GitHub 仓库上查看 Flutter 桌面应用程序示例。

https://github.com/sbis04/flutter_desktop_sample/blob/main/codemagic.yaml

https://github.com/sbis04/flutter_desktop_sample

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
