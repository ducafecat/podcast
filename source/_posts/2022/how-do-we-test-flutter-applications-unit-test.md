---
title: 如何测试 Flutter 应用? ー 单元测试
tags: flutter
categories: 译文
date: 2022-04-21 00:00:00
---

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/20220421063539.png)

## 原文

> https://medium.com/@sanketsmasurkar/how-do-we-test-flutter-applications-unit-test-beb6c8d9d5ac

## 参考

- https://docs.flutter.dev/cookbook/testing/unit/introduction

## 正文

大家好！

首先让我们了解一下为什么在应用程序还处于开发阶段时，我们需要对其执行自动化测试。

自动化测试是一种使用自动化工具测试工具在软件应用程序上编写和运行测试用例套件的方法。

如果我们在开发阶段测试应用程序，我们可以通过使用自动化测试来检测可能导致出现例行更改的新错误，从而节省时间。

**Flutter 自动测试的几种类型:**

- _Unit Test 单元测试_
- _Widget Test 小部件测试_
- _Integration Test 综合测试_

> 注意: 我们只在本文中讨论单元测试。

**什么是单元测试和测试用例？**

单元测试用于验证单个函数、方法或类的正确性。要测试此函数、方法或类，必须编写测试用例。为了验证预期输出而必须满足的条件称为测试用例。

> 实施程序:

在本文中，我们将为登录页面运行测试用例，该页面有两个字段: email id 和 password。

### 步骤 1: 确保在 pubspec yaml 文件中存在 `flutter_test` 依赖项。

```dart
dev_dependencies:
  flutter_test:
    sdk: flutter
```

### 步骤 2: 创建验证器类。

我们所有的验证方法都将在这个类中编写。这些方法将用于我们的登录页面(在 email 和密码 _textformfield_ 验证器参数中)和 _login_unit_test_ 类中。

- ** 密码 TextFormField\_**

```dart
TextFormField(
  validator: Validators.validatePwd,
)
```

- **_Validator Class Validator 类_**

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a522687227ae954b402cf9cf78f5f750feeabb9dd5622209e932a4da9fe95605.png)

### 步骤 3: 在测试目录中创建 login_unit_test 类，并为 Login 编写测试用例。

现在，我们已经到了编写测试用例的阶段。

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/bf09b4d8537d24ba4f910adce55f9eefa9a86d04a6d5f9bffe3da8805800f814.png)

```dart
void main() {

  test('Verify invalid email address', () {
    String emailId = 'abc@gmail';

    var result = Validators._validateEmailId_(emailId);
    expect(result, 'Enter valid Email ID.');

  });

  test('Verify valid password', () {
    String pwd = 'Qwe@12345';

    var result = Validators._validatePwd_(pwd);
    expect(result, null);

  });

}
```

我们使用 **_test_** 函数来定义测试用例。我们必须在这个函数中提供两个强制参数。第一个参数是一个字符串值，即(对特定情况的描述) ，第二个参数是一个函数，我们在其中验证测试用例。

我们使用 **_expect_** 函数将结果与预期进行比较。Expect 函数还有两个必需的参数。第一个参数是验证器函数返回的结果，第二个参数是比较器或预期结果。

测试包提供了测试和期望函数。

> 让我们回到前面的例子。

第一个测试功能是验证电子邮件地址。如您所见，有两个变量: _emailId_ 和 _result_ _emailId_ 之后，Validator 类函数将值返回给 _result_ 变量。

因此，你们中的一些人可能想知道为什么我将第二个测试用例的预期结果值保留为 _null_。

如果您继续执行第 3 步，在那里我们创建了验证器类，您将注意到，如果值不是空的或者与正则表达式匹配，我们将返回一个空值。这就是为什么期望的结果值设置为 null 的原因。确保结果是正确的。

**您也可以将您的测试用例组合在一起。**

```dart
group('Multiple test cases for password verification', () {

  test('Verify password', () {
    String pwd = 'Abc@12345';

    var result = Validators._validatePwd_(pwd);
    expect(result, null);

  });

  test('Verify empty password', () {
    String pwd = '';

    var result = Validators._validatePwd_(pwd);
    expect(result, 'Enter Password');

  });

});
```

### 步骤 4: 运行测试用例。

要运行所有测试用例，只需在终端输入以下命令: _flutter test test/name_of_your_file.dart_

- **当所有测试用例通过时，这就是结果。**

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/a641fc1c9b0244a165a3d5fb732590c4dd5f08ce0caf1ee345f782b1488b4058.png)

- **您也可以通过单击播放按钮来运行单个测试用例，我在下面的图片中高亮显示了这一点。**

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/7c22d01ddeef82a2984b326b3676bfc54256f50e0785d23f30c5f5935218944f.png)

- **如果测试用例失败，下面是结果。**

![](https://ducafecat.oss-cn-beijing.aliyuncs.com/podcast/aa681e4f52dae755738b83a4c13dc3b5016d56ea2b23359678c22b6fd7c73273.png)

> **结语:**

这是一个 Flutter 的单元测试的快速概述。我希望我已经给了您足够的信息来尝试在您的项目中进行单元测试。

---

© 猫哥

- 微信 ducafecat

- [博客 ducafecat.tech](https://ducafecat.tech/)

- [github](https://github.com/ducafecat)

- [bilibili](https://space.bilibili.com/404904528)

![订阅号](https://ducafecat.tech/img/banner-gzh.png)
