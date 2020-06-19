---
title: Flutter 实战从零开始 新闻客户端 - 13 使用 supernova、imgcook 导入 sketch psd xd 自动生成用户中心代码
date: 2020-06-18 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](2020-06-18-14-22-12.png)

# 本节目标

- 了解 supernova 代码生成器作用
- 导入 xd 设计稿
- 如何高效使用生成代码

## 正文

### 代码生成器

- [supernova](https://supernova.io)
- [imgcook](https://imgcook.taobao.org)

### 有潜力加入代码生成功能

- [lanhuapp](https://lanhuapp.com/)
- [mockplus](https://app.mockplus.cn/)

### supernova 代码生成器

https://supernova.io/

### 导入 xd 设计稿，生成代码

![](2020-06-18-11-13-44.png)

> 商业设计稿不好直接分享, 可以加微信联系 ducafecat

### 编写用户中心界面代码

#### 组织代码结构

```dart
class _AccountPageState extends State<AccountPage> {
  // 个人页面 头部
  Widget _buildUserHeader() {}

  // 列表项
  Widget _buildCell() {}

  @override
  Widget build(BuildContext context) {
    final appState = Provider.of<AppState>(context);

    return SingleChildScrollView(
      child: Column(
        children: <Widget>[
          _buildUserHeader(),
          _buildCell(),
        ],
      ),
    );
  }
}
```

#### 直接使用生成的代码

- 个人页面 头部

```dart
  Widget _buildUserHeader() {
    return Container(
              height: 333,
              child: Stack(
                alignment: Alignment.center,
                children: [
                  Positioned(
                    left: 0,
                    right: 0,
                    child: Container(
                      height: 333,
                      decoration: BoxDecoration(
                        color: AppColors.primaryBackground,
                      ),
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.end,
                        crossAxisAlignment: CrossAxisAlignment.stretch,
                        children: [
                          Container(
                            height: 2,
                            decoration: BoxDecoration(
                              color: AppColors.primaryElement,
                            ),
                            child: Container(),
                          ),
                        ],
                      ),
                    ),
                  ),
                  Positioned(
                    left: 20,
                    top: 40,
                    right: 20,
                    bottom: 21,
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.stretch,
                      children: [
                        Container(
                          height: 198,
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.stretch,
                            children: [
                              Align(
                                alignment: Alignment.topCenter,
                                child: Container(
                                  width: 108,
                                  height: 108,
                                  child: Stack(
                                    alignment: Alignment.center,
                                    children: [
                                      Positioned(
                                        top: 0,
                                        child: Container(
                                          width: 108,
                                          height: 108,
                                          decoration: BoxDecoration(
                                            color: AppColors.primaryBackground,
                                            boxShadow: [
                                              Shadows.primaryShadow,
                                            ],
                                            borderRadius: Radii.k54pxRadius,
                                          ),
                                          child: Container(),
                                        ),
                                      ),
                                      Positioned(
                                        top: 10,
                                        child: Image.asset(
                                          "assets/images/image.png",
                                          fit: BoxFit.none,
                                        ),
                                      ),
                                    ],
                                  ),
                                ),
                              ),
                              Spacer(),
                              Container(
                                margin: EdgeInsets.only(bottom: 9),
                                child: Text(
                                  "Cameron Rogers",
                                  textAlign: TextAlign.center,
                                  style: TextStyle(
                                    color: AppColors.primaryText,
                                    fontFamily: "Montserrat",
                                    fontWeight: FontWeight.w400,
                                    fontSize: 24,
                                  ),
                                ),
                              ),
                              Text(
                                "@boltrogers",
                                textAlign: TextAlign.center,
                                style: TextStyle(
                                  color: AppColors.primaryText,
                                  fontFamily: "Avenir",
                                  fontWeight: FontWeight.w400,
                                  fontSize: 16,
                                ),
                              ),
                            ],
                          ),
                        ),
                        Spacer(),
                        Container(
                          height: 44,
                          child: FlatButton(
                            onPressed: () => this.onButtonPressed(context),
                            color: Color.fromARGB(255, 41, 103, 255),
                            shape: RoundedRectangleBorder(
                              borderRadius: BorderRadius.all(Radius.circular(6)),
                            ),
                            textColor: Color.fromARGB(255, 255, 255, 255),
                            padding: EdgeInsets.all(0),
                            child: Text(
                              "Get Premium - \$9.99",
                              textAlign: TextAlign.center,
                              style: TextStyle(
                                color: AppColors.secondaryText,
                                fontFamily: "Montserrat",
                                fontWeight: FontWeight.w400,
                                fontSize: 18,
                              ),
                            ),
                          ),
                        ),
                      ],
                    ),
                  ),
                ],
              ),
            );
  }
```

- 列表项

```dart
  Widget _buildCell() {
    return Container(
              height: 60,
              child: Stack(
                alignment: Alignment.centerLeft,
                children: [
                  Positioned(
                    left: 0,
                    right: 0,
                    child: Container(
                      height: 60,
                      decoration: BoxDecoration(
                        color: AppColors.secondaryElement,
                      ),
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.end,
                        crossAxisAlignment: CrossAxisAlignment.stretch,
                        children: [
                          Container(
                            height: 1,
                            decoration: BoxDecoration(
                              color: AppColors.primaryElement,
                            ),
                            child: Container(),
                          ),
                        ],
                      ),
                    ),
                  ),
                  Positioned(
                    right: 0,
                    child: Row(
                      mainAxisAlignment: MainAxisAlignment.end,
                      children: [
                        Container(
                          margin: EdgeInsets.only(right: 11),
                          child: Text(
                            "12",
                            textAlign: TextAlign.right,
                            style: TextStyle(
                              color: AppColors.primaryText,
                              fontFamily: "Avenir",
                              fontWeight: FontWeight.w400,
                              fontSize: 18,
                            ),
                          ),
                        ),
                        Container(
                          width: 24,
                          height: 24,
                          margin: EdgeInsets.only(right: 20),
                          child: Image.asset(
                            "assets/images/icon.png",
                            fit: BoxFit.none,
                          ),
                        ),
                      ],
                    ),
                  ),
                  Positioned(
                    left: 0,
                    child: Stack(
                      alignment: Alignment.center,
                      children: [
                        Positioned(
                          left: 0,
                          right: 19,
                          child: Container(),
                        ),
                        Positioned(
                          left: 20,
                          right: 0,
                          child: Text(
                            "Favorite channels",
                            textAlign: TextAlign.left,
                            style: TextStyle(
                              color: AppColors.primaryText,
                              fontFamily: "Montserrat",
                              fontWeight: FontWeight.w400,
                              fontSize: 18,
                            ),
                          ),
                        ),
                      ],
                    ),
                  ),
                ],
              ),
            );
  }
```

#### 抽取代码 lib/common/widgets/app.dart

```dart
/// 10像素 Divider
Widget divider10Px({Color bgColor = AppColors.secondaryElement}) {
  return Container(
    height: duSetWidth(10),
    decoration: BoxDecoration(
      color: bgColor,
    ),
  );
}
```

#### 修改代码 \_buildUserHeader

```dart
  // 个人页面 头部
  Widget _buildUserHeader() {
    return Container(
      height: duSetWidth(333),
      child: Stack(
        alignment: Alignment.center,
        children: [
          // 背景
          Positioned(
            left: 0,
            right: 0,
            child: Container(
              height: duSetWidth(333),
              decoration: BoxDecoration(
                color: AppColors.primaryBackground,
              ),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.end,
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  Container(
                    height: duSetWidth(2),
                    decoration: BoxDecoration(
                      color: AppColors.tabCellSeparator,
                    ),
                    child: Container(),
                  ),
                ],
              ),
            ),
          ),
          Positioned(
            left: 20,
            top: 40,
            right: 20,
            bottom: 21,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                // 头像
                Container(
                  height: duSetWidth(198),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.stretch,
                    children: [
                      Align(
                        alignment: Alignment.topCenter,
                        child: Container(
                          width: duSetWidth(108),
                          height: duSetWidth(108),
                          child: Stack(
                            alignment: Alignment.center,
                            children: [
                              Positioned(
                                top: 0,
                                child: Container(
                                  width: duSetWidth(108),
                                  height: duSetWidth(108),
                                  decoration: BoxDecoration(
                                    color: AppColors.primaryBackground,
                                    boxShadow: [
                                      Shadows.primaryShadow,
                                    ],
                                    borderRadius: BorderRadius.all(
                                        Radius.circular(duSetWidth(108) / 2)),
                                  ),
                                  child: Container(),
                                ),
                              ),
                              Positioned(
                                top: 10,
                                child: Image.asset(
                                  "assets/images/account_header.png",
                                  height: duSetWidth(88),
                                  width: duSetWidth(88),
                                  fit: BoxFit.fill,
                                ),
                              ),
                            ],
                          ),
                        ),
                      ),
                      // 文字
                      Spacer(),
                      Container(
                        margin: EdgeInsets.only(bottom: 9),
                        child: Text(
                          Global.profile.displayName,
                          textAlign: TextAlign.center,
                          style: TextStyle(
                            color: AppColors.primaryText,
                            fontFamily: "Montserrat",
                            fontWeight: FontWeight.w400,
                            fontSize: 24,
                          ),
                        ),
                      ),
                      Text(
                        "@boltrogers",
                        textAlign: TextAlign.center,
                        style: TextStyle(
                          color: AppColors.primaryText,
                          fontFamily: "Avenir",
                          fontWeight: FontWeight.w400,
                          fontSize: 16,
                        ),
                      ),
                    ],
                  ),
                ),
                // 按钮
                Spacer(),
                Container(
                  height: 44,
                  child: FlatButton(
                    onPressed: () => {},
                    color: Color.fromARGB(255, 41, 103, 255),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.all(Radius.circular(6)),
                    ),
                    textColor: Color.fromARGB(255, 255, 255, 255),
                    padding: EdgeInsets.all(0),
                    child: Text(
                      "Get Premium - \$9.99",
                      textAlign: TextAlign.center,
                      style: TextStyle(
                        color: AppColors.primaryElementText,
                        fontFamily: "Montserrat",
                        fontWeight: FontWeight.w400,
                        fontSize: 18,
                      ),
                    ),
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
```

#### 修改代码 \_buildCell

```dart

  // 列表项
  Widget _buildCell({
    String title,
    String subTitle,
    int number,
    bool hasArrow = false,
    VoidCallback onTap,
  }) {
    return GestureDetector(
      onTap: onTap,
      child: Container(
        height: duSetWidth(60),
        color: Colors.white,
        child: Stack(
          alignment: Alignment.centerLeft,
          children: [
            // 背景
            Positioned(
              left: 0,
              right: 0,
              child: Container(
                height: duSetWidth(60),
                decoration: BoxDecoration(
                  color: AppColors.primaryBackground,
                ),
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.end,
                  crossAxisAlignment: CrossAxisAlignment.stretch,
                  children: [
                    Container(
                      height: duSetWidth(1),
                      decoration: BoxDecoration(
                        color: AppColors.tabCellSeparator,
                      ),
                      child: Container(),
                    ),
                  ],
                ),
              ),
            ),
            // 右侧
            Positioned(
              right: 0,
              child: Row(
                mainAxisAlignment: MainAxisAlignment.end,
                children: [
                  // 数字
                  number == null
                      ? Container()
                      : Container(
                          margin: EdgeInsets.only(right: 11),
                          child: Text(
                            number.toString(),
                            textAlign: TextAlign.right,
                            style: TextStyle(
                              color: AppColors.primaryText,
                              fontFamily: "Avenir",
                              fontWeight: FontWeight.w400,
                              fontSize: duSetFontSize(18),
                            ),
                          ),
                        ),
                  // 箭头
                  hasArrow == false
                      ? Container()
                      : Container(
                          width: duSetWidth(24),
                          height: duSetWidth(24),
                          margin: EdgeInsets.only(right: 20),
                          child: Icon(
                            Icons.arrow_forward_ios,
                            color: AppColors.primaryText,
                          ),
                        ),
                ],
              ),
            ),

            // 标题
            title == null
                ? Container()
                : Positioned(
                    left: 20,
                    child: Text(
                      title,
                      textAlign: TextAlign.left,
                      style: TextStyle(
                        color: AppColors.primaryText,
                        fontFamily: "Montserrat",
                        fontWeight: FontWeight.w400,
                        fontSize: duSetFontSize(18),
                      ),
                    ),
                  ),

            // 子标题
            subTitle == null
                ? Container()
                : Positioned(
                    right: 20,
                    child: Text(
                      subTitle,
                      textAlign: TextAlign.left,
                      style: TextStyle(
                        color: AppColors.primaryText,
                        fontFamily: "Montserrat",
                        fontWeight: FontWeight.w400,
                        fontSize: duSetFontSize(18),
                      ),
                    ),
                  ),
          ],
        ),
      ),
    );
  }
```

#### 修改代码 build

```dart

  @override
  Widget build(BuildContext context) {
    final appState = Provider.of<AppState>(context);

    return SingleChildScrollView(
      child: Column(
        children: <Widget>[
          _buildUserHeader(),
          divider10Px(),
          _buildCell(
            title: "Email",
            subTitle: "boltrogers@gmail.com",
          ),
          divider10Px(),
          _buildCell(
            title: "Favorite channels",
            number: 12,
            hasArrow: true,
          ),
          _buildCell(
            title: "Bookmarks",
            number: 294,
            hasArrow: true,
          ),
          _buildCell(
            title: "Popular categories",
            number: 7,
            hasArrow: true,
          ),
          divider10Px(),
          _buildCell(
            title: "Newsletter",
            hasArrow: true,
          ),
          _buildCell(
            title: "Settings",
            hasArrow: true,
          ),
          divider10Px(),
          _buildCell(
            title: "Switch Gray Filter",
            hasArrow: true,
            onTap: () => appState.switchGrayFilter(),
          ),
          _buildCell(
            title: "Log out",
            hasArrow: true,
            onTap: () => goLoginPage(context),
          ),
          divider10Px(),
        ],
      ),
    );
  }
```

### 技巧

- vscode 固定代码

![](2020-06-18-11-15-41.png)

## 总结

![](2020-06-19-07-22-10.png)

## 资源

### 设计稿蓝湖预览

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

### 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.13

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
