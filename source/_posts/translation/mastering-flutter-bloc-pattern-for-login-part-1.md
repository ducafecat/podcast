---
title: Flutter BLoC 用户登录 1
tags: flutter
categories: 译文
date: 2021-05-06 00:00:00
---

![](2021-05-06-08-01-18.png)

## 原文

> https://medium.com/theotherdev-s/mastering-flutter-bloc-pattern-for-login-part-1-94082e139725

## 前言

![](2021-05-06-07-51-51.png)

首先，由于这不是一个基本的教程，我们理所当然地认为这是一个路线的知识，我们也包含了一点点的 `validation` 与 `formoz` 包来创建可重用的模型; 这不是本教程的目的，以显示这将如何工作，您将看到这在下一个教程。对于登录部分，出于教程的目的，我们还使用了 `BLoC (Cubit)` 的子集，因此您将看到这两者之间的区别。

## 代码，可以先阅读代码，再看文档

https://github.com/Alessandro-v/bloc_login

## 参考

- https://pub.flutter-io.cn/packages/equatable
- https://pub.flutter-io.cn/packages/validation
- https://pub.flutter-io.cn/packages/flutter_bloc
- https://pub.flutter-io.cn/packages/formoz

## 正文

![](2021-05-06-07-54-44.png)

### 开始

在我们开始之前，让我们在 pubspec.yaml 中添加一些必要的包:

```
equatable: ^2.0.0
flutter_bloc: ^7.0.0
formz: ^0.3.2
```

添加 `equatable` 包只会使您的工作更加容易，但是如果您想手动比较类的实例，只需要重写 "==" 和 hashCode。

### 登录状态

让我们从一个包含表单状态和所有字段状态的类开始:

```dart
class LoginState extends Equatable {
  const LoginState({
    this.email = const Email.pure(),
    this.password = const Password.pure(),
    this.status = FormzStatus.pure,
    this.exceptionError,
  });
  final Email email;
  final Password password;
  final FormzStatus status;
  final String exceptionError;
  @override
  List<Object> get props => [email, password, status, exceptionError];
  LoginState copyWith({
    Email email,
    Password password,
    FormzStatus status,
    String error,
  }) {
    return LoginState(
      email: email ?? this.email,
      password: password ?? this.password,
      status: status ?? this.status,
      exceptionError: error ?? this.exceptionError,
    );
  }
}
```

现在让我们创建我们的 LoginCubit，它将负责执行逻辑，例如通过 emit 获取电子邮件和输出新状态:

```dart
class LoginCubit extends Cubit<LoginState> {
  LoginCubit() : super(const LoginState());
  void emailChanged(String value) {
    final email = Email.dirty(value);
    emit(state.copyWith(
      email: email,
      status: Formz.validate([
        email,
        state.password
      ]),
    ));
  }
  void passwordChanged(String value) {
    final password = Password.dirty(value);
    emit(state.copyWith(
      password: password,
      status: Formz.validate([
        state.email,
        password
      ]),
    ));
  }
  Future<void> logInWithCredentials() async {
    if (!state.status.isValidated) return;
    emit(state.copyWith(status: FormzStatus.submissionInProgress));
    try {
      await Future.delayed(Duration(milliseconds: 500));
      emit(state.copyWith(status: FormzStatus.submissionSuccess));
    } on Exception catch (e) {
      emit(state.copyWith(status: FormzStatus.submissionFailure, error: e.toString()));
    }
  }
}
```

但是我们如何将腕尺与我们的用户界面连接起来呢？下面是对 `BlocProvider` 的解救，这是一个小部件，它使用: `BlocProvider.of<logincubit>(context)` 为其子部件提供一个区块

```dart
BlocProvider(
  create: (_) => LoginCubit(),
  child: LoginForm(),
),
```

### 登入表格

既然现在似乎都在他自己的地方，是时候解决我们的最后一块 puzzle，整个用户界面

```dart
class LoginForm extends StatelessWidget {
  const LoginForm({Key key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return BlocConsumer<LoginCubit, LoginState>(
        listener: (context, state) {
          if (state.status.isSubmissionFailure) {
            print('submission failure');
          } else if (state.status.isSubmissionSuccess) {
            print('success');
          }
        },
        builder: (context, state) => Stack(
          children: [
            Positioned.fill(
              child: SingleChildScrollView(
                padding: const EdgeInsets.fromLTRB(38.0, 0, 38.0, 8.0),
                child: Container(
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.stretch,
                    mainAxisAlignment: MainAxisAlignment.start,
                    children: [
                      _WelcomeText(),
                      _EmailInputField(),
                      _PasswordInputField(),
                      _LoginButton(),
                      _SignUpButton(),
                    ],
                  ),
                ),
              ),
            ),
            state.status.isSubmissionInProgress
                ? Positioned(
              child: Align(
                alignment: Alignment.center,
                child: CircularProgressIndicator(),
              ),
            ) : Container(),
          ],
        )
    );
  }
}
```

为了对 Cubit 发出的新状态做出反应，我们需要将我们的表单包裹在一个 BlocConsumer 中，现在我们将暴露一个监听者和一个建造者。

- Listener

这里我们将监听状态更改，例如，在响应 API 调用时显示错误或执行导航。

- Builder

在这里，我们将显示 ui 反应状态的变化，我们的 `Cubit`。

### 用户界面

我们的用户界面由一个列和 5 个子元素组成，但是我们只展示 2 个简短的小部件:

```dart
class _EmailInputField extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<LoginCubit, LoginState>(
      buildWhen: (previous, current) => previous.email != current.email,
      builder: (context, state) {
        return AuthTextField(
          hint: 'Email',
          key: const Key('loginForm_emailInput_textField'),
          keyboardType: TextInputType.emailAddress,
          error: state.email.error.name,
          onChanged: (email) => context
              .read<LoginCubit>()
              .emailChanged(email),
        );
      },
    );
  }
}
class _LoginButton extends StatelessWidget {
  const _LoginButton({Key key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return BlocBuilder<LoginCubit, LoginState>(
      buildWhen: (previous, current) => previous.status != current.status,
      builder: (context, state) {
        return CupertinoButton(
            child: Text('Login'),
            onPressed: state.status.isValidated
                ? () => context.read<LoginCubit>().logInWithCredentials()
                : null
        );
      },
    );
  }
}
```

这两个小部件都包装在一个 `BlocBuilder` 中，只有当肘位为它们各自的评估属性发出新的状态时，`BlocBuilder` 才负责重新构建这些小部件，因此，例如，如果用户没有在 email 字段中键入任何内容，`EmailInputField` 将永远不会被重新构建。

相反，如果所有字段都经过验证，按钮将调用 `logInWithCredentials()` 函数，该函数将根据 API 响应发出一个新状态(失败或成功)。

## 老铁记得 点赞、转发 ，我将更有动力呈现 Flutter 好文~~~~

---

© 猫哥

https://ducafecat.tech/

https://github.com/ducafecat

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
