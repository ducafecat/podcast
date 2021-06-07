---
title: 用 Melos 管理多包 Flutter 项目
tags: flutter
categories: 译文
date: 2021-06-08 00:00:00
---

![](2021-06-08-06-23-31.png)

## 猫哥说

我正在的写的新闻客户端代码模板，只是适合最多 100 pages 的轻巧型单包项目。

但是页面再多的话，说明你的项目业务、功能、组件足够复杂，项目也庞大，参与的人也多，这样的话项目就需要多包架构了，做过 android 的朋友最能体会了。

这篇文章就是介绍如何用 melos 来管理多包项目，看看对你是否有帮助吧。

## 老铁记得 转发 ，猫哥会呈现更多 Flutter 好文~~~~

## 微信 flutter 研修群 ducafecat

## 原文

## 代码

https://github.com/SAGARSURI/melos_demo

> integrate_melos 分支是完成的代码

## 参考

- https://docs.page/invertase/melos
- https://invertase.io/

## 正文

大多数情况下，当你创建一个 flutter 项目。你使用一个包。这个项目由一个 pubspec.yaml，lib 文件夹组成。您将所有的特性和实用程序放在同一个包中。但也有一些项目将它们的特性和实用程序分解成多个包。这有助于提高关注点分离，并允许团队开源他们的一些软件包。下面是一个多包项目的示意图:

![](2021-06-08-05-53-01.png)

在这里，我们将项目分为三个层次。第一层是根项目，它包含适用于项目中所有不同包的通用配置。第二层拥有独立的功能包，它们不相互依赖。第三层由多个功能包中使用的实用工具包组成。我不会深入探讨如何创建或结构一个多包 flutter 项目。本文将着重于解决一个典型的多包 flutter 项目所面临的特殊挑战。

### 挑战

在一个简单的程序包中，运行下面的任务是非常简单的:

- flutter pub get
- flutter test
- flutter analyze
- Generating code coverage i.e flutter test --coverage

但是在一个多包的 flutter 项目中运行相同的任务是具有挑战性的，因为你需要在项目中的每个包中运行这些任务，并在任务完成后给出总结结果。现在我们知道挑战是什么了。让我们来讨论一下解决这个问题的可能方法。

### 解决方案

有两种可能的解决方案来解决这个问题。让我们看看第一个:

1. 为各种任务编写 bash 脚本

这绝对是一个解决方案，但不是一个明智的解决方案。您需要首先编写一个脚本，找出项目中的所有包，并在其中运行上述任务之一。您还需要确保以漂亮的格式显示输出，以使内容具有可读性。如果您更喜欢 GUI，那么您需要在 IDE 中创建某种配置，以便通过 GUI 运行脚本

2. 将 Melos 整合到你的项目中

这是一个比我强烈推荐的第一个方案更聪明的解决方案。因此，让我们详细讨论什么是 Melos，以及如何将其集成到您的多包项目中

### 介绍 Melos

> Melos 是一个 CLI 工具，用于管理多个包的 flutter/飞镖项目。

Melos 是由一个著名的小组在 flutter 社区即 invertase。你可以在他们的网站上阅读关于 Melos 的详细信息，但是这里有一个 Melos 提供的特性的快速列表:

- Automatic versioning & changelog generation. 自动版本控制和更新日志生成
- Automated publishing of packages to 将包自动发布到 pub.dev.
- Local package linking and installation. 本地包的链接和安装
- Executing simultaneous commands across packages. 跨包执行同步命令
- Listing of local packages & their dependencies. 本地包及其依赖项的列表

现在让我们看看如何使用 Melos 执行上述所有任务。

- 注意: 如果你想在实践中学习，请下载入门课程。还有另一个分支，您可以在其中找到项目的最终版本。

https://github.com/SAGARSURI/melos_demo

### 安装 Melos

让我们先安装 Melos。我假设您已经安装了 Flutter SDK，并将 Flutter 和 Dart 路径设置为 `bash_profile`。在终端中运行以下命令:

```sh
dart pub global activate melos
```

下一步是在 IDE 中打开初学者项目。我更喜欢使用 Intellij，并且会向你展示一些由 Melos 提供的非常棒的 GUI 特性。项目结构应如下:

![](2021-06-08-05-58-51.png)

现在创建一个名为 melos.yaml 的文件，并将以下内容复制到其中:

```yaml
name: melos_demo

packages:
  - utility/**
  - features/**
  - "*"
```

让我们理解一下上面的脚本:
a) name : 你必须给出项目的名称。您可以在 root 项目的 pubspec.yaml 中找到它.
b) packages : 这个列表应该包含到项目中单个软件包的路径. 可以使用 glob 模式展开格式定义每个路径.

### Melos Bootstrap

现在，从根项目中在终端中运行以下命令，将所有本地包链接在一起，并更新依赖关系，即 flutter pub get。

```sh
melos bootstrap
```

在运行上面的命令之后，你应该会看到如下的输出:

![](2021-06-08-06-02-12.png)

你可以在这里阅读为什么你需要引导 Melos。准确地说，这是在项目中设置 Melos 或执行项目清理时应该执行的重要命令之一。

https://docs.page/invertase/melos/getting-started#why-do-i-need-to-bootstrap

### Melos Clean

当您希望从项目中删除临时文件(构建工件、pub 文件等)时，可以执行此命令。下面是这个命令的样子:

```sh
melos clean
```

### Commands

现在，您将着手创建不同的命令，以实现我们在本文开头提到的任务。

在特定的包中运行测试用例在你的 melos.yaml 文件中写入以下命令:

```yaml
name: melos_demo

packages:
  - utility/**
  - features/**
  - "*"

scripts:
  test:selective_unit_test:
    run: melos exec --dir-exists="test" --fail-fast -- flutter test --no-pub --coverage
    description: Run Flutter tests for a specific package in this project.
    select-package:
      flutter: true
      dir-exists: test
```

让我们理解一下上面的脚本中发生了什么:

1)你已经创建了一个自定义脚本，即 test: selective_unit_test，一旦执行，它会显示一个选项来选择一个你想要运行的单元测试包。

2. Melos 提供了强大的过滤选项来选择符合过滤条件的包。在上面的脚本中，您使用 -- dir-exists = “ test”作为筛选选项。这将过滤由 testfolder 组成的所有包。你可以在他们的网站上找到更多的过滤选项。

3)——如果遇到失败的测试用例，fail-fast 将立即终止脚本执行。

4)您可以使用描述部分为每个脚本提供一个可读的描述。

5)你一定想知道为什么你把这个命令命名为 test: selective_unit_test。下一个命令将回答您的问题。

6)你可以详细阅读 melos exec 做了什么。基本上，它将在项目中的每个包中执行命令或脚本。

https://docs.page/invertase/melos/commands#exec

现在运行以下命令:

```sh
melos run test:selective_unit_test
```

您将看到以下输出:

![](2021-06-08-06-09-26.png)

上面的命令能够找出包含文件夹测试的这些包。输入 2 作为选项，你会看到如下输出:

![](2021-06-08-06-09-41.png)

在所有包中运行测试用例现在编写以下命令，它将运行项目中的所有单元测试用例。这不会提示任何选项的选择:

```yaml
scripts:
  test:
    run: melos run test:selective_unit_test --no-select
    description: Run all Flutter tests in this project.
```

让我们讨论一下上面的命令是做什么的:

1. 这个命令将基本上运行上一个命令--no-select as an argument. 这意味着运行所有的单元测试
2. 你可以使用 melos 来运行这个命令 因为可能有多个变种的测试命令，就像您在前一步骤 i.e 中创建的那样 test:selective_unit_test . 您还可以创建更多的变种，例如 test:e2e_test , test:bdd_test etc. 你可以将所有的变量组合在一起，并在一个命令 i.e 中运行 test .

运行所有包中的 analyzein: 在 script 部分创建以下命令:

```yaml
analyze:
  run: melos exec -- flutter analyze .
  description: Run `dart analyze` in all packages.
```

这里没有什么特别的，您可以执行 melos 运行分析来运行所有包中的分析。

生成整个项目的代码 coverage: 为整个项目生成代码 coverage。项目中包含一个自定义脚本，即组合覆盖。嘘。这将基本上合并来自不同包的所有 lcov.info 文件到一个 lcov.info 文件中。然后，您可以使用此方法将 lcov.info 文件转换为 HTML。

https://stackoverflow.com/a/53663093/4161284

```sh
#!/usr/bin/env bash

escapedPath="$(echo `pwd` | sed 's/\//\\\//g')"

if grep flutter pubspec.yaml > /dev/null; then
  if [ -d "coverage" ]; then
    # combine line coverage info from package tests to a common file
    if [ ! -d "$MELOS_ROOT_PATH/coverage_report" ]; then
      mkdir "$MELOS_ROOT_PATH/coverage_report"
    fi
    sed "s/^SF:lib/SF:$escapedPath\/lib/g" coverage/lcov.info >> "$MELOS_ROOT_PATH/coverage_report/lcov.info"
    rm -rf "coverage"
  fi
fi
```

在脚本部分下面写下以下命令:

```yaml
gen_coverage: melos exec -- "\$MELOS_ROOT_PATH/combine_coverage.sh"
```

Path 会给出存储 MELOS.yaml 的路径即 root 项目。脚本执行完毕后。您可以在项目中看到 coverage_report 文件夹。现在你有一个 lcov.info 文件，它会给你一个整个项目的报告。

最后，你的 melos.yaml 文件看起来像这样:

```yaml
name: melos_demo

packages:
  - utility/**
  - features/**
  - "*"
scripts:
  test:selective_unit_test:
    run: melos exec --dir-exists="test" --fail-fast -- flutter test --no-pub --coverage
    description: Run Flutter tests for a specific package in this project.
    select-package:
      flutter: true
      dir-exists: test

  test:
    run: melos run test:selective_unit_test --no-select
    description: Run all Flutter tests in this project.

  analyze:
    run: melos exec -- flutter analyze .
    description: Run `dart analyze` in all packages.

  gen_coverage: melos exec -- "\$MELOS_ROOT_PATH/combine_coverage.sh"
```

### 图形用户界面选项

如果您不希望通过终端执行这些命令，并希望使用 GUI 运行它们，那么 Melos 可以满足您的要求。添加所有命令后，可以再次运行引导程序命令。这将生成一些配置，你可以看到一些图形用户界面选项如下:

![](2021-06-08-06-13-17.png)

现在您可以执行所有这些命令，而无需在终端中键入任何内容。

### Next Steps

这只是冰山一角。你可以在 Melos 网站上学到更多的过滤选项和命令。希望你喜欢这篇文章。

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
