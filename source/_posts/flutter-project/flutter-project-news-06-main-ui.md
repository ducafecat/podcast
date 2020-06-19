---
title: Flutter 实战从零开始 新闻客户端 - 06 代码规范、业务代码组织、新闻首页实现
date: 2020-03-31 00:00:00
tags: flutter
categories: Flutter 实战从零开始
---

![](bg.jpeg)

## 1 本节目标

- 代码规范
- 业务代码组织
- 首页代码编写

## 2 代码规范

### 2.1 官方代码规范

https://dart.dev/guides/language/effective-dart

### 2.3 chrome 插件 <彩云小译 - 网页翻译插件>

https://chrome.google.com/webstore/detail/lingocloud-web-translatio/jmpepeebcbihafjjadogphmbgiffiajh

### 2.4 阿里项目规范

https://github.com/alibaba/flutter-go/blob/master/Flutter_Go%20%E4%BB%A3%E7%A0%81%E5%BC%80%E5%8F%91%E8%A7%84%E8%8C%83.md

## 3 业务界面代码组织

### 3.1 redux、fish-redux

- redux 架构

![](2020-04-01-09-50-51.png)

- fish-redux 架构

进一步的细分，进行规范

https://github.com/alibaba/fish-redux/tree/master/doc
https://medium.com/@dave790602/flutter-architecture-fish-redux-9b753912224a

![](2020-04-01-09-52-25.png)

- fish-redux 代码

![](2020-04-01-10-30-19.png)

### 3.2 bloc

- 架构

![](2020-04-01-09-56-04.png)

https://bloclibrary.dev/#/

- 代码组织

![](2020-04-01-10-39-09.png)

### 3.3 简单就是美

![](2020-04-01-10-41-34.png)

### 3.4 如何平衡

- 是否团队开发

* 是否简单业务（20 页面）

![](2020-04-01-14-17-41.png)

- 是否重交互（视频社交、聊天 A）

![](2020-04-01-14-16-06.png)

## 4 新闻首页实现

### 4.1 界面组成分析

- 分类导航、推荐新闻、频道导航

![](2020-04-01-10-55-39.png)

- 新闻列表、广告 ad、邮件订阅

![](2020-04-01-10-57-39.png)

### 4.2 代码框架

```dart
...
class _MainPageState extends State<MainPage> {

  @override
  void initState() {
    super.initState();
    _loadAllData();
  }

  // 读取所有数据
  _loadAllData() async {
  }

  // 分类菜单
  Widget _buildCategories() {
    return Container();
  }

  // 推荐阅读
  Widget _buildRecommend() {
    return Container();
  }

  // 频道
  Widget _buildChannels() {
    return Container();
  }

  // 新闻列表
  Widget _buildNewsList() {
    return Container();
  }

  // ad 广告条
  // 邮件订阅
  Widget _buildEmailSubscribe() {
    return newsletterWidget();
  }

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
        child: Column(
          children: <Widget>[
            _buildCategories(),
            Divider(height: 1),
            _buildRecommend(),
            Divider(height: 1),
            _buildChannels(),
            Divider(height: 1),
            _buildNewsList(),
            Divider(height: 1),
            _buildEmailSubscribe(),
          ],
        ),
      );
  }
}

```

### 4.3 实现业务

- 创建 widget 单独文件

![](2020-04-01-11-00-53.png)

- 分类导航

lib/pages/main/categories_widget.dart

```dart
Widget newsCategoriesWidget({
  List<CategoryResponseEntity> categories,
  String selCategoryCode,
  Function(CategoryResponseEntity) onTap,
}) {
  return SingleChildScrollView(
    scrollDirection: Axis.horizontal,
    child: Row(
      children: categories.map<Widget>((item) {
        return Container(
          alignment: Alignment.center,
          height: duSetHeight(52),
          padding: EdgeInsets.symmetric(horizontal: 8),
          child: GestureDetector(
            child: Text(
              item.title,
              style: TextStyle(
                color: selCategoryCode == item.code
                    ? AppColors.secondaryElementText
                    : AppColors.primaryText,
                fontSize: duSetFontSize(18),
                fontFamily: 'Montserrat',
                fontWeight: FontWeight.w600,
              ),
            ),
            onTap: () => onTap(item),
          ),
        );
      }).toList(),
    ),
  );
}
```

- 频道导航

lib/pages/main/channels_widget.dart

```dart
Widget newsChannelsWidget({
  List<ChannelResponseEntity> channels,
  Function(ChannelResponseEntity) onTap,
}) {
  return Container(
    height: duSetHeight(137),
    child: SingleChildScrollView(
      scrollDirection: Axis.horizontal,
      child: Row(
        children: channels.map<Widget>((item) {
          return Container(
            width: duSetWidth(70),
            height: duSetHeight(97),
            margin: EdgeInsets.symmetric(horizontal: duSetWidth(10)),
            child: InkWell(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  // 图标
                  Container(
                    height: duSetWidth(64),
                    margin: EdgeInsets.symmetric(horizontal: duSetWidth(3)),
                    child: Stack(
                      alignment: Alignment.center,
                      children: [
                        Positioned(
                          left: 0,
                          top: 0,
                          right: 0,
                          child: Container(
                            height: duSetWidth(64),
                            decoration: BoxDecoration(
                              color: AppColors.primaryBackground,
                              boxShadow: [
                                Shadows.primaryShadow,
                              ],
                              borderRadius:
                                  BorderRadius.all(Radius.circular(32)),
                            ),
                            child: Container(),
                          ),
                        ),
                        Positioned(
                          left: duSetWidth(10),
                          top: duSetWidth(10),
                          right: duSetWidth(10),
                          child: Image.asset(
                            "assets/images/channel-${item.code}.png",
                            fit: BoxFit.none,
                          ),
                        ),
                      ],
                    ),
                  ),
                  // 标题
                  Text(
                    item.title,
                    textAlign: TextAlign.center,
                    overflow: TextOverflow.clip,
                    maxLines: 1,
                    style: TextStyle(
                      color: AppColors.thirdElementText,
                      fontFamily: "Avenir",
                      fontWeight: FontWeight.w400,
                      fontSize: duSetFontSize(14),
                      height: 1,
                    ),
                  ),
                ],
              ),
              onTap: () => onTap(item),
            ),
          );
        }).toList(),
      ),
    ),
  );
}
```

- 新闻行 Item

lib/pages/main/news_item_widget.dart

```dart
Widget newsItem(NewsItem item) {
  return Container(
    height: duSetHeight(161),
    padding: EdgeInsets.all(duSetWidth(20)),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceBetween,
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        // 图
        imageCached(
          item.thumbnail,
          width: duSetWidth(121),
          height: duSetWidth(121),
        ),
        // 右侧
        SizedBox(
          width: duSetWidth(194),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              // 作者
              Container(
                margin: EdgeInsets.all(0),
                child: Text(
                  item.author,
                  style: TextStyle(
                    fontFamily: 'Avenir',
                    fontWeight: FontWeight.normal,
                    color: AppColors.thirdElementText,
                    fontSize: duSetFontSize(14),
                    height: 1,
                  ),
                ),
              ),
              // 标题
              Container(
                margin: EdgeInsets.only(top: duSetHeight(10)),
                child: Text(
                  item.title,
                  style: TextStyle(
                    fontFamily: 'Montserrat',
                    fontWeight: FontWeight.w500,
                    color: AppColors.primaryText,
                    fontSize: duSetFontSize(16),
                    height: 1,
                  ),
                  overflow: TextOverflow.clip,
                  maxLines: 3,
                ),
              ),
              // Spacer
              Spacer(),
              // 一行 3 列
              Container(
                child: Row(
                  crossAxisAlignment: CrossAxisAlignment.center,
                  children: <Widget>[
                    // 分类
                    ConstrainedBox(
                      constraints: BoxConstraints(
                        maxWidth: duSetWidth(60),
                      ),
                      child: Text(
                        item.category,
                        style: TextStyle(
                          fontFamily: 'Avenir',
                          fontWeight: FontWeight.normal,
                          color: AppColors.secondaryElementText,
                          fontSize: duSetFontSize(14),
                          height: 1,
                        ),
                        overflow: TextOverflow.clip,
                        maxLines: 1,
                      ),
                    ),
                    // 添加时间
                    Container(
                      width: duSetWidth(15),
                    ),
                    ConstrainedBox(
                      constraints: BoxConstraints(
                        maxWidth: duSetWidth(100),
                      ),
                      child: Text(
                        '• ${duTimeLineFormat(item.addtime)}',
                        style: TextStyle(
                          fontFamily: 'Avenir',
                          fontWeight: FontWeight.normal,
                          color: AppColors.thirdElementText,
                          fontSize: duSetFontSize(14),
                          height: 1,
                        ),
                        overflow: TextOverflow.clip,
                        maxLines: 1,
                      ),
                    ),
                    // 更多
                    Spacer(),
                    InkWell(
                      child: Icon(
                        Icons.more_horiz,
                        color: AppColors.primaryText,
                        size: 24,
                      ),
                      onTap: () {},
                    ),
                  ],
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

- 邮件订阅

lib/pages/main/newsletter_widget.dart

```dart
Widget newsletterWidget() {
  return Container(
    margin: EdgeInsets.all(duSetWidth(20)),
    child: Column(
      children: <Widget>[
        // newsletter
        Row(
          children: <Widget>[
            Text(
              'Newsletter',
              style: TextStyle(
                fontFamily: 'Montserrat',
                fontSize: duSetFontSize(18),
                fontWeight: FontWeight.w600,
                color: AppColors.thirdElement,
              ),
            ),
            Spacer(),
            IconButton(
              icon: Icon(
                Icons.close,
                color: AppColors.thirdElementText,
                size: duSetFontSize(17),
              ),
              onPressed: () {},
            ),
          ],
        ),

        // email
        inputEmailEdit(
          marginTop: 19,
          keyboardType: TextInputType.emailAddress,
          hintText: "Email",
          isPassword: false,
          controller: null,
        ),

        // btn subcrible
        Padding(
          padding: EdgeInsets.only(top: 15),
          child: btnFlatButtonWidget(
            onPressed: () {},
            width: duSetWidth(335),
            height: duSetHeight(44),
            fontWeight: FontWeight.w600,
            title: "Subscribe",
          ),
        ),

        // disc
        Container(
          margin: EdgeInsets.only(top: duSetHeight(29)),
          width: duSetWidth(261),
          child: Text.rich(TextSpan(children: <TextSpan>[
            TextSpan(
              text: 'By clicking on Subscribe button you agree to accept',
              style: new TextStyle(
                color: AppColors.thirdElementText,
                fontFamily: "Avenir",
                fontWeight: FontWeight.w400,
                fontSize: duSetFontSize(14),
              ),
            ),
            TextSpan(
              text: ' Privacy Policy',
              style: new TextStyle(
                color: AppColors.secondaryElementText,
                fontFamily: "Avenir",
                fontWeight: FontWeight.w400,
                fontSize: duSetFontSize(14),
              ),
              recognizer: TapGestureRecognizer()
                ..onTap = () {
                  toastInfo(msg: 'Privacy Policy');
                },
            ),
          ])),
        ),
      ],
    ),
  );
}
```

- 推荐阅读

lib/pages/main/recommend_widget.dart

```dart
Widget recommendWidget(NewsRecommendResponseEntity newsRecommend) {
  return Container(
    margin: EdgeInsets.all(duSetWidth(20)),
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: <Widget>[
        // 图
        imageCached(
          newsRecommend.thumbnail,
          width: duSetWidth(335),
          height: duSetHeight(290),
        ),
        // 作者
        Container(
          margin: EdgeInsets.only(top: duSetHeight(14)),
          child: Text(
            newsRecommend.author,
            style: TextStyle(
              fontFamily: 'Avenir',
              fontWeight: FontWeight.normal,
              color: AppColors.thirdElementText,
              fontSize: duSetFontSize(14),
            ),
          ),
        ),
        // 标题
        Container(
          margin: EdgeInsets.only(top: duSetHeight(10)),
          child: Text(
            newsRecommend.title,
            style: TextStyle(
              fontFamily: 'Montserrat',
              fontWeight: FontWeight.w600,
              color: AppColors.primaryText,
              fontSize: duSetFontSize(24),
              height: 1,
            ),
          ),
        ),
        // 一行 3 列
        Container(
          margin: EdgeInsets.only(top: duSetHeight(10)),
          child: Row(
            crossAxisAlignment: CrossAxisAlignment.center,
            children: <Widget>[
              // 分类
              ConstrainedBox(
                constraints: const BoxConstraints(
                  maxWidth: 120,
                ),
                child: Text(
                  newsRecommend.category,
                  style: TextStyle(
                    fontFamily: 'Avenir',
                    fontWeight: FontWeight.normal,
                    color: AppColors.secondaryElementText,
                    fontSize: duSetFontSize(14),
                    height: 1,
                  ),
                  overflow: TextOverflow.clip,
                  maxLines: 1,
                ),
              ),
              // 添加时间
              Container(
                width: duSetWidth(15),
              ),
              ConstrainedBox(
                constraints: const BoxConstraints(
                  maxWidth: 120,
                ),
                child: Text(
                  '• ${duTimeLineFormat(newsRecommend.addtime)}',
                  style: TextStyle(
                    fontFamily: 'Avenir',
                    fontWeight: FontWeight.normal,
                    color: AppColors.thirdElementText,
                    fontSize: duSetFontSize(14),
                    height: 1,
                  ),
                  overflow: TextOverflow.clip,
                  maxLines: 1,
                ),
              ),
              // 更多
              Spacer(),
              InkWell(
                child: Icon(
                  Icons.more_horiz,
                  color: AppColors.primaryText,
                  size: 24,
                ),
                onTap: () {},
              ),
            ],
          ),
        ),
      ],
    ),
  );
}
```

## 蓝湖设计稿

https://lanhuapp.com/url/lYuz1
密码: gSKl

> 蓝湖现在收费了，所以查看标记还请自己上传 xd 设计稿
> 商业设计稿文件不好直接分享, 可以加微信联系 ducafecat

## YAPI 接口管理

http://yapi.demo.qunar.com/

## 代码

https://github.com/ducafecat/flutter_learn_news/releases/tag/v1.0.6

## 参考

- [Flutter Go 代码开发规范 0.1.0 版](https://github.com/alibaba/flutter-go#the-flutter-go-roadmap%E8%B7%AF%E7%BA%BF%E5%9B%BE-for-2019)
- [effective-dart](https://dart.dev/guides/language/effective-dart)
- [bloc](https://bloclibrary.dev/#/)

## VSCode 插件

- Flutter、Dart
- [Flutter Widget Snippets](https://marketplace.visualstudio.com/items?itemName=alexisvt.flutter-snippets)
- [Awesome Flutter Snippets](https://marketplace.visualstudio.com/items?itemName=Nash.awesome-flutter-snippets)
- [Paste JSON as Code](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)
- [bloc](https://marketplace.visualstudio.com/items?itemName=FelixAngelov.bloc)

## 视频

- [b 站](https://space.bilibili.com/404904528/channel/detail?cid=106755)
- [油管镜像](https://www.youtube.com/watch?v=Uucg6GGGBsY&list=PL274L1n86T80VZR30KaLOKV6jqwTq5E8D)

---

© 猫哥

[https://ducafecat.tech](https://ducafecat.tech/)
